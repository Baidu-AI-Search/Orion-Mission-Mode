# AI 推理应用中的自带密钥（BYOK）最佳实践指南

*一份面向构建 LLM API（如 OpenAI、Anthropic、Google Vertex AI 和 AWS Bedrock）BYOK 支持的开发者工具公司的综合工程指南。*

---

## 执行摘要

AI 推理应用中的自带密钥（BYOK）是一种架构模式，用户提供自己的上游 LLM 提供商 API 凭证，而非依赖共享的、由平台管理的密钥。对于开发者工具公司而言，BYOK 深刻改变了计费关系、安全责任和可观测性模式：你的服务永远不会成为 token 消费的记录商户，你无需向提供商预付大额余额，企业客户保留了数据主权和成本控制权。但这些优势伴随着严肃的工程义务——泄露一个用户的 API 密钥、错误处理服务账户凭证，或意外记录包含其他嵌入密钥的提示词，都可能迅速侵蚀信任。

本指南系统阐述了安全架构、验证流程、隐私实践、提供商差异、成本工具、常见陷阱和架构替代方案。代码示例使用 Python/TypeScript 及主流 SDK（OpenAI、Anthropic、Vertex AI SDK、boto3）。所有指导均假设面向多租户的生产 SaaS 环境；对于内部工具可按比例适用。

---

## 1. 安全架构

BYOK 最重要的规则是：**应用必须将用户密钥视为有毒物质**。用户的密钥是一个不记名令牌，可以消费真实费用、访问微调模型、导出数据，在某些提供商控制台中还能读取组织用量。一个密钥的泄露绝不应影响其他租户、你自己的基础设施密钥或应用本身。

### 1.1 客户端 vs. 服务端密钥处理

存在两种主要模式；大多数应用需要明确选择其一，而非默认使用最容易编码的方式。

**服务端存储（推荐用于大多数 SaaS）。** 密钥通过 TLS 传输到你的后端，使用信封加密（见 §1.3）静态加密存储，仅在进行出站推理调用所需的短暂窗口内解密。应用自身的服务器永远不持久化明文密钥。优点：你可以一致地实现验证、缓存、成本计量、速率限制和提供商端重试。缺点：你的基础设施成为高价值目标；一次泄露可能暴露大量密钥。

**客户端 / 零知识代理。** 密钥永远不离开用户设备；浏览器或原生客户端直接调用提供商，或用户运行本地代理（如 VS Code 扩展、桌面守护进程或自托管网关）注入凭证。应用仅接收渲染后的输出和用户明确选择加入的遥测数据。优点：极大降低了你的服务的爆炸半径；天然适合注重隐私的开发工具。缺点：你失去了服务端精确计量的能力，CORS 和 CSP 约束限制了大多数提供商的直接浏览器调用，故障排查变得更困难。

**实际中的混合模式（如官方 VS Code Copilot BYOK 功能和基于 OpenRouter 的客户端所用）：** 通过你的后端路由推理调用以获得可观测性，但使用客户端持有的密码短语派生密钥加密存储密钥（如通过 Web Crypto 的 AES-GCM，密钥从不触网的密码派生），使你的服务器仅持有不透明的密文。这显著更复杂，但兼得两者之利。

### 1.2 传输安全

- 对**入站**（用户 → 你的 API）和**出站**（你的 API → 提供商）两跳均强制使用 TLS 1.2+ 和现代密码套件。完全禁用明文 HTTP。
- 客户端提交密钥时使用 `POST`（不使用查询字符串——查询字符串会泄露到服务器日志、CDN 日志和浏览器历史中）。优先在 JSON body 中传输密钥而非表单字段；表单解析器有时会自动记录字段值。
- 出站到提供商时，尽可能固定预期的提供商主机名（如 `api.openai.com`、`api.anthropic.com`）并拒绝任何重定向（大多数提供商不重定向，但配置错误的企业代理或 TLS 中间人设备可能会）。
- 永远不要在 webhook URL、错误追踪负载（Sentry、Datadog）或分析事件中传输用户密钥。
- 如果你有直接调用提供商的浏览器扩展或桌面客户端，请注意大多数 LLM 提供商**不会为浏览器调用设置宽松的 CORS 头**；从 content script 调用它们会失败或将密钥暴露给页面上运行的任何脚本。在这些情况下通过薄代理路由。

### 1.3 使用信封加密的静态加密

存储用户 API 密钥的黄金标准是基于托管 KMS 的**信封加密**，而非使用环境变量中静态主密钥的临时 AES。

- **客户主密钥（CMK）** 存在于你的云 KMS（AWS KMS、GCP Cloud KMS、Azure Key Vault 或 HashiCorp Vault Transit）中。CMK 永远不离开 HSM 边界，并按你的策略自动轮换。
- 当用户提交密钥时，在本地生成随机 256 位**数据加密密钥（DEK）**，使用它对明文密钥进行 AES-256-GCM 加密，然后请求 KMS 用 CMK 包装（加密）DEK。仅存储*被包装的 DEK*、密文、AES-GCM nonce 和认证标签——永远不存储明文 DEK。
- 使用密钥时，通过 KMS 调用解包 DEK，在内存中解密，签署出站请求，然后在调用返回后立即清零明文缓冲区。

```python
import os, json
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import boto3

KMS_KEY_ID = "arn:aws:kms:us-east-1:123456789012:key/aaaaaaaa-bbbb-cccc"
kms = boto3.client("kms")

def encrypt_user_key(plaintext: str) -> dict:
    # Generate a one-time DEK locally
    dek = AESGCM.generate_key(bit_length=256)
    # Ask KMS for a wrapped copy of the DEK (plaintext DEK never persists)
    wrapped = kms.encrypt(KeyId=KMS_KEY_ID, Plaintext=dek)["CiphertextBlob"]
    nonce = os.urandom(12)
    aesgcm = AESGCM(dek)
    # Bind the ciphertext to a logical context so it can't be swapped across tenants
    ctx = b"byok-user-key-v1"
    ct = aesgcm.encrypt(nonce, plaintext.encode("utf-8"), associated_data=ctx)
    return {
        "wrapped_dek": wrapped.hex(),
        "nonce": nonce.hex(),
        "ciphertext": ct.hex(),
        "aad": ctx.decode(),
        "kms_key_id": KMS_KEY_ID,
        "alg": "AES-256-GCM",
    }

def decrypt_user_key(blob: dict) -> str:
    wrapped = bytes.fromhex(blob["wrapped_dek"])
    dek = kms.decrypt(CiphertextBlob=wrapped)["Plaintext"]
    aesgcm = AESGCM(dek)
    pt = aesgcm.decrypt(
        bytes.fromhex(blob["nonce"]),
        bytes.fromhex(blob["ciphertext"]),
        associated_data=blob["aad"].encode(),
    )
    return pt.decode("utf-8")
```

操作注意事项：

- 每个环境（dev/staging/prod）使用独立的 CMK，对于企业级客户，考虑使用每客户 CMK（在 BYOK 之上的"自持密钥"——有时称为 HYOK）。
- 永远不要打印或记录明文 DEK 或明文用户密钥。在语言允许的情况下覆写内存中的缓冲区（Python 中是尽力而为；Rust/Go 中是可靠的）。
- 应用最小权限 IAM，使只有你的推理 worker 角色能调用 `kms:Decrypt`，且仅限特定 CMK ARN。
- 对于 AWS KMS，使用 `EncryptionContext` 参数（如 `{"tenant_id": user_id, "secret_type": "llm_api_key"}`），使 KMS 调用可审计且跨租户密文交换在密码学上被阻止。
- 对于长期存活的服务账户（GCP JSON 密钥、AWS 访问密钥对），考虑将它们作为使用相同方案加密的二进制 blob 存储；不要将它们嵌入环境文件中。

### 1.4 内存处理和密钥生命周期

- 在**使用前立即**解密密钥，将它们保存在请求范围变量中，而非全局缓存。如果你需要 token 的性能缓存（如短期 OAuth 访问令牌），使用严格短于其自然过期时间的 TTL 缓存，并在有访问控制的内存存储中（如进程内 LRU，默认不使用 Redis——如果必须使用 Redis，在放入前加密值）。
- 实现每密钥轮换：让用户从你的仪表板替换或撤销密钥，无需提交工单。密钥轮换时，永久删除旧密文，如果你的 KMS 支持，安排密钥材料删除。
- 设置**全局终止开关**：一个管理员端点，在怀疑泄露时立即禁用所有 BYOK 密钥（如通过设置 `revoked_at` 时间戳），而不销毁密文。

### 1.5 网络隔离

将允许解密和使用 BYOK 密钥的 worker 放在专用网络段中（独立 VPC/子网、独立 Kubernetes 命名空间、独立服务账户）。这些 worker 应具备：

- 出站仅允许访问已知 LLM 提供商端点和 KMS 端点；拒绝通用互联网出站以防止通过攻击者控制的主机进行数据外泄。
- 入站仅接受来自你的 API 网关通过 mTLS 的连接。
- 不访问密钥存储表之外的其他敏感数据存储（分析数据库、CRM、支付系统）。

---

## 2. 密钥验证

相当比例的用户会粘贴无效密钥：错误的提供商、拼写错误、截断的值、已撤销的密钥，或——关键的——一个看起来有效但属于与预期不同的项目/组织的密钥。验证应尽早捕获这些问题，在用户花费数小时在你的应用中调试晦涩的 401 之前。

### 2.1 存储前验证流程

当用户提交密钥时，按以下顺序执行检查：

1. **格式健全性检查。** 每个提供商有可识别的前缀：OpenAI 密钥以 `sk-` 开头（较新的项目密钥以 `sk-proj-` 开头），Anthropic 密钥以 `sk-ant-api03-` 开头，Google Vertex 使用服务账户 JSON（不是单个字符串），AWS 使用 20 字符的 `AKIA...` 访问密钥 ID 配对密钥。拒绝不匹配预期前缀的密钥并提供有帮助的消息；这可防止"我把 GitHub token 粘贴到了 OpenAI 字段"的错误。
2. **实时轻量探测。** 对提供商进行最低成本的已认证调用，确认密钥有效**且**账户有权访问至少一个模型。对于 OpenAI，`GET /v1/models` 免费且对有效密钥返回 200。对于 Anthropic，使用最便宜的 Haiku 模型进行 `max_tokens=1` 的最小 `POST /v1/messages` 是标准探测。对于 Vertex，调用 `projects.locations.publishers.models.list` 或运行 1-token 生成。对于 Bedrock，调用 `bedrock:ListFoundationModels`。
3. **范围/权限检查。** 某些密钥是只读的，某些限制在特定项目，某些——如 GCP 服务账户或 AWS IAM 用户——可能完全缺少推理权限。以*与你的应用在生产中调用完全相同的方式*（相同模型系列、Vertex/Bedrock 相同区域）尝试探测，这样你就不会存储一个通过模型列表但实际完成会失败的密钥。
4. **速率限制和配额快照。** 对于暴露计费端点的提供商（OpenAI 的 `/v1/organization/*`、`/dashboard/billing/subscription`），在验证时记录用户当前的硬限制和软限制。这为成本透明度（§5）提供支持，并让你在密钥处于将很快耗尽的免费层时发出警告。

### 2.2 验证尝试的速率限制

验证端点可被滥用：窃取了候选密钥列表的攻击者可以使用你的验证端点作为预言机来检查哪些有效。缓解措施：

- 在"添加密钥"端点上应用严格的每 IP 和每账户速率限制（如每用户每分钟 5 次尝试，每 IP 20 次）。
- 在一个账户连续 N 次失败后（如 3 个无效密钥），锁定该账户的密钥管理进行冷却，并要求重新认证。
- 永远不要以泄露信息给攻击者的方式区分"密钥格式无效"和"密钥被提供商拒绝"的响应体；两者都应返回相同的通用"验证失败"，且开发者可见的错误代码仅对提交用户可见。
- 如果看到枚举模式，对源 IP 发出短期（如 5 分钟）断路器。

```python
from fastapi import HTTPException
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)

@limiter.limit("5/minute")
@app.post("/api/v1/keys")
async def add_key(req, provider: str, key: str, user=Depends(current_user)):
    if not format_matches(provider, key):
        # Count toward the rate limit but return generic error
        raise HTTPException(400, "Key could not be validated")
    try:
        await probe_key(provider, key)
    except AuthError:
        await audit_log(user.id, "key_validation_failed", provider)
        raise HTTPException(400, "Key could not be validated")
    await store_encrypted_key(user.id, provider, key)
    return {"status": "ok"}
```

### 2.3 优雅处理过期或已撤销的密钥

密钥在生产中因多种原因失效：用户在提供商控制台轮换了它、其计费过期、管理员在组织范围内撤销了它，或 OAuth 访问令牌简单地过期了。你的运行时必须：

- 捕获来自提供商的 401/403 响应，并将其转换为面向用户的"密钥过期或已撤销"状态，而非通用错误。在你的 UI 中显示醒目横幅，要求用户更新密钥。
- 实现**自动优雅降级**：如果用户有多个密钥（如同时有 OpenAI 和 Anthropic 密钥）且其中一个失败，对不需要特定模型的请求回退到另一个，并通知用户。
- 对于 OAuth 风格的凭证（Google、某些企业 Azure OpenAI 配置），使用存储的刷新令牌透明地刷新访问令牌。如果刷新失败（如用户撤销了应用授权），按与已撤销 API 密钥相同方式处理。
- **不要因单个 401 自动暂停用户账户**；瞬态 401 会发生（时钟偏差、提供商事故、区域认证闪断）。仅在一个窗口内连续 N 次失败后（如 10 分钟内 3 次失败）将密钥标记为"损坏"，并提示用户重新验证。
- 记录失败原因（`invalid`、`expired`、`billing`、`rate_limited`）用于诊断目的，但**不要将原始提供商错误体包含在你的日志中**除非你已脱敏——提供商错误有时会回显 Authorization 头或请求片段。

---

## 3. 隐私影响

BYOK 改变了你、用户和 LLM 提供商之间的隐私关系。这里的透明度是产品特性，不是合规勾选项。

### 3.1 数据流图

在**共享代理模型**下（大多数 AI SaaS 的默认方式）：

```
用户提示 → 你的服务器 → [你的共享 API 密钥] → LLM 提供商 → 响应
```

你的服务器看到明文提示和响应，你是提供商的客户；提供商可能根据其 API 数据保留策略保留提示/响应数据（如 OpenAI 默认保留 API 数据 30 天用于滥用监控，除非客户选择了零数据保留）。

在**BYOK 服务端**模式下：

```
用户提示 → 你的服务器（解密用户密钥）→ [用户自己的密钥] → 提供商 → 响应
```

你的服务器仍然看到提示和响应（毕竟你是中间的工具），但*提供商的*日志和计费附加到用户的账户，而非你的。用户控制自己的零数据保留设置、组织数据策略和提供商端的模型访问限制。

在**BYOK 客户端**（零知识）模式下：

```
用户提示 → [用户密钥本地持有] → 提供商 → 响应 → 你的服务器（可选遥测）
```

你的服务器永远不会看到密钥；根据设计，它们甚至可能看不到提示/响应。这是最强的隐私姿态，但限制了你能提供的功能（服务端缓存、评估、团队管理员审计日志、服务端工具调用）。

### 3.2 你的应用能看到什么

在你的隐私政策和产品内对用户明确说明你能看到哪些数据：

- **是的，你能看到**提示和响应——当请求通过你的后端路由时（这在任何服务端代理模型中都成立，无论是否 BYOK）。
- **你看不到**用户在提供商端的计费详情、支付方式或组织级使用仪表板——除非你使用用户密钥调用提供商特定的计费 API，这应该是选择加入的。
- **你可以选择不看**提示/响应——如果你提供零知识模式；在产品语言中要精确（"端到端加密"有特定且可审计的含义；"我们不查看你的数据"不是技术声明）。

### 3.3 日志注意事项

这是 BYOK 事故的最大单一来源：

- **从每个 HTTP 日志中剥离 Authorization 头**，包括入站（你的 API 日志）和出站（你的出站 HTTP 客户端日志）。大多数日志中间件默认记录头；配置显式的脱敏列表。
- **从错误报告中剥离用户密钥**（Sentry、Bugsnag、Rollbar）。这些工具自动捕获请求体、头和局部变量；出站调用期间的崩溃可以轻易地将明文密钥序列化到栈帧中。使用 SDK 的 `before_send` / `denyUrls` / 清理钩子来丢弃或掩码任何名为 `api_key`、`Authorization`、`x-api-key` 等的字段。
- **默认不记录完整的提示或响应。** 即使用户同意了 BYOK，他们的提示可能包含自己的密钥（调试时粘贴的数据库凭证、第三方 API 密钥、PII）。如果必须采样日志用于调试，使用正则表达式脱敏检测到的密钥（常见密钥前缀如 `sk-[A-Za-z0-9_-]{20,}`、`sk-ant-[A-Za-z0-9_-]{20,}`、`AKIA[0-9A-Z]{16}`、`AIza[0-9A-Za-z_-]{35}` 等）和哈希信用卡模式。
- **审计日志记录密钥管理操作**，而非密钥内容：谁添加、轮换或撤销了密钥，何时，从哪个 IP。这些日志本身是敏感的——作为受限访问的审计材料对待。
- **永远不要在 API 响应中回显密钥**（即使掩码），除了显示最后 4 个字符用于标识。许多客户支持截图泄露了密钥的前 N + 后 N 个字符。

```python
# Example: structlog processor that redacts common secret patterns
import re, structlog

SECRET_PATTERNS = [
    re.compile(r"sk-[A-Za-z0-9_-]{20,}"),
    re.compile(r"sk-ant-api03-[A-Za-z0-9_-]{20,}"),
    re.compile(r"AKIA[0-9A-Z]{16}"),
    re.compile(r"AIza[0-9A-Za-z_-]{35}"),
    re.compile(r"(?i)authorization:\s*bearer\s+[A-Za-z0-9._-]+"),
]

def redact_secrets(_, __, event_dict):
    for k, v in list(event_dict.items()):
        if isinstance(v, str):
            for p in SECRET_PATTERNS:
                v = p.sub("[REDACTED]", v)
            event_dict[k] = v
    return event_dict

structlog.configure(processors=[redact_secrets, structlog.processors.JSONRenderer()])
```

### 3.3 提示日志和数据保留

- 无限期存储推理元数据（时间戳、模型、token 计数、延迟、用户/组织 ID、状态）如有需要；仅在有明确、可逆目的（调试、用户可见历史、评估）且有面向用户的保留控制时才存储完整的提示/响应。
- 为用户工作区提供"零日志"模式：提示流式传输到提供商并被你的服务器立即丢弃，仅保留 token 计数用于成本计量。
- 当用户删除账户时，根据你的数据保留策略清除密文密钥 blob 和任何关联的提示历史。

---

## 4. 提供商差异

每个主要 LLM 提供商处理认证和密钥管理的方式不同。对它们的抽象不当是微妙 bug 的常见来源。

### 4.1 OpenAI

- **认证方式：** `Authorization: Bearer sk-...` 头中的 Bearer-token API 密钥。密钥长期有效直到被撤销。
- **密钥类型：** 旧版 `sk-...` 密钥，较新的**项目范围** `sk-proj-...` 密钥（推荐——它们限制在单个项目并携带细粒度权限）。鼓励用户为你的应用创建项目范围密钥，而非使用其个人组织密钥。
- **管理密钥 vs. 用户密钥：** 组织所有者可以创建不绑定人类的服务账户密钥；这些是团队工作区 BYOK 的理想选择。
- **免费层限制：** 免费试用密钥有严格速率限制（~3 RPM，低每日上限），试用积分耗尽后停止工作。在验证期间检测此情况并警告用户。
- **数据保留：** 用户可在组织级别选择加入**零数据保留**；设置后，OpenAI 不保留 API 请求/响应用于 30 天滥用监控。引导用户到此设置。
- **使用量端点：** `/v1/organization/usage/completions`（含日期范围）和 `/dashboard/billing/subscription` 返回使用量和硬限制；它们需要具有管理/计费范围的密钥。
- **探测调用：** 使用用户密钥的 `GET https://api.openai.com/v1/models`。
- **注意事项：** OpenAI 平台 API 密钥与 ChatGPT Plus 订阅凭证**不同**——这是支持工单的常见来源。平台 API 没有 OAuth 流程。

### 4.2 Anthropic

- **认证方式：** `x-api-key: sk-ant-api03-...` 头（注意：Anthropic 使用自定义头，而非标准 `Authorization: Bearer`）。
- **密钥格式：** 所有当前密钥以 `sk-ant-api03-` 开头。较旧的 `sk-ant-` 密钥已弃用。
- **管理控制台：** Anthropic 支持工作区，每个工作区有独立 API 密钥；推荐为团队 BYOK 使用每工作区密钥。
- **探测调用：** `POST /v1/messages`，使用 `{"model":"claude-3-haiku-20240307","max_tokens":1,"messages":[{"role":"user","content":"hi"}]}` ——截至撰写时没有免费的模型列表端点。
- **速率限制：** Anthropic 速率限制基于层级（Free、Builder、Scale），通过 `anthropic-ratelimit-*` 响应头报告。在你的仪表板中向用户展示这些。
- **数据保留：** Anthropic 默认不使用 API 输入进行训练（截至其 2023 年隐私更新），但在有限窗口内保留数据用于滥用监控，除非客户签约零保留。
- **注意事项：** 不要混淆 `api.anthropic.com`（直接 API）和 `console.anthropic.com`（Web 控制台）。AWS Bedrock 和 Google Vertex AI 托管的 Claude 使用**完全不同的认证**（见 §4.4/4.5）。

### 4.3 Google Vertex AI (Gemini)

- **认证方式：** 默认不是简单的 API 密钥。Vertex AI 使用 GCP **服务账户**（JSON 密钥文件）或通过 Application Default Credentials 铸造的 OAuth 2.0 访问令牌。简单的 API 密钥字符串（`AIza...`）仅适用于受限的 Gemini Developer API / Generative Language API，其有独立配额且不是 Vertex AI 本身。
- **所需字段：** 调用 Vertex AI 需要 (a) GCP 项目 ID，(b) 区域（如 `us-central1`），(c) 服务账户 JSON 密钥或 OAuth 客户端凭证。
- **服务账户处理：** 用户将上传或粘贴一个 JSON 文档。将此 JSON 视为密钥——它包含私钥。通过尝试轻量预测来验证服务账户具有 `roles/aiplatform.user` 角色（或更受限的自定义角色）。
- **个人用户的 OAuth：** 对于消费者应用，使用 OAuth 2.0 配合 `https://www.googleapis.com/auth/cloud-platform` 范围（或更窄的 `https://www.googleapis.com/auth/cloud-platform.read-only` 加 Vertex 特定范围），并让用户在引导流程中创建项目 + 启用 Vertex AI。访问令牌是短期的（~1 小时）；你必须安全存储刷新令牌并自动刷新。
- **探测调用：** 使用最小负载对 `gemini-1.5-flash` 调用 `projects.locations.publishers.models.streamGenerateContent`。
- **成本：** Vertex 定价因区域而异；使用 Cloud Billing Catalog API 或 Vertex 定价表进行成本估算。
- **注意事项：** 用户经常粘贴 generative-language `AIza...` 密钥期望获得 Vertex 级别的访问；区分这些并路由到正确端点。作为 JSON 上传的服务账户密钥必须被解析并作为多字段密钥处理，而非字符串。

### 4.4 AWS Bedrock

- **认证方式：** **仅 IAM。** Bedrock 不接受简单的 API 密钥字符串。用户必须提供以下之一：
  - **IAM 访问密钥对**（`AWS_ACCESS_KEY_ID` + `AWS_SECRET_ACCESS_KEY`，可选带 `AWS_SESSION_TOKEN` 用于角色假设），
  - **IAM 角色 ARN**，你的 AWS 账户可通过跨账户信任假设（企业首选——无长期密钥），
  - **OIDC/SAML 联合角色**（如果你的应用支持）。
- **区域很重要：** Bedrock 模型可用性是区域性的；用户必须在特定区域启用模型。你必须接受并存储区域连同凭证。
- **模型访问：** 在调用模型之前，用户必须通过 Bedrock 控制台请求访问。有效的密钥对不意味着模型访问——你的探测必须调用实际模型（如对 `anthropic.claude-3-haiku` 或 `amazon.titan-text-express-v1` 的 `InvokeModel`），而不仅是 `ListFoundationModels`。
- **跨账户角色假设流程（推荐用于企业 BYOK）：** 客户在其账户中创建 IAM 角色，信任策略授予你的 AWS 账户 `sts:AssumeRole`，权限策略允许 `bedrock:InvokeModel`（可选 `bedrock:ListFoundationModels`）。他们给你角色 ARN；你的 worker 在调用时使用自己的 AWS 凭证假设该角色并获得短期会话凭证。这意味着**你永远不持久化长期 AWS 密钥**——巨大的安全胜利。
- **探测调用：** `sts:GetCallerIdentity` 验证凭证（免费，不需要模型权限），然后最小 `bedrock:InvokeModel` 确认模型访问。
- **注意事项：** 访问密钥对极其强大——它们授予附加策略允许的任何 IAM 权限，如果用户过度授权可能包括完整 S3/EC2 访问。强烈鼓励跨账户角色假设并记录最小 IAM 策略。永远不记录 `AWS_SECRET_ACCESS_KEY` 或会话令牌。

```json
// Minimal customer-managed IAM policy for Bedrock BYOK
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel", "bedrock:InvokeModelWithResponseStream"],
      "Resource": "arn:aws:bedrock:*::foundation-model/*"
    }
  ]
}
```

### 4.5 汇总表

| 提供商 | 凭证类型 | 探测端点 | 推荐存储方式 |
|---|---|---|---|
| OpenAI | 长期 `sk-...` Bearer token | `GET /v1/models` | 信封加密字符串 |
| Anthropic | 长期 `sk-ant-api03-...`（自定义头） | 最小 Haiku 调用 | 信封加密字符串 |
| Google Vertex AI | 服务账户 JSON 或 OAuth 刷新令牌 | 最小 Gemini Flash 调用 | 信封加密 JSON / token 捆绑 |
| Google AI Studio | 短 `AIza...` API 密钥 | 最小 Gemini 调用 | 信封加密字符串 |
| AWS Bedrock | IAM 访问密钥对或跨账户角色 ARN | `sts:GetCallerIdentity` + 1-token 调用 | 角色 ARN 明文存储；访问密钥信封加密（不推荐） |
| Azure OpenAI | API 密钥（`api-key` 头）或 Azure AD token | 最小 Chat Completions 调用 | 信封加密 |

---

## 5. 成本透明度

用户使用 BYOK 的核心动机是直接定价且无中间商加价。讽刺的是，这意味着你的工具必须更努力地使成本可见——因为当意外账单来临时，用户会先问*你*，即使费用来自 OpenAI。

### 5.1 调用前成本估算

在每次完成之前，使用以下信息计算估算成本：

- 提示的输入 token 计数（使用提供商的 tokenizer：OpenAI 的 `tiktoken`、Anthropic 的 `count_tokens` API 或本地 Haiku tokenizer、Vertex 的 `countTokens` RPC）。
- 可配置的 `max_tokens` 上限（以及合理的默认值，因为许多用户不设置此项）。
- 作为代码库中版本化 JSON 维护的每模型定价表；随发布一起推送更新，并定期从提供商定价页面刷新。

醒目地显示估算（如"此请求将使用 ~1.2k 输入 token 和最多 500 输出 token ≈ $0.008（GPT-4o）"）。对于批量/长时间运行的任务（评估、批量摘要），显示汇总估算并在超过阈值时要求明确确认。

### 5.2 实时使用量仪表板

为每个工作区提供使用量仪表板，显示每个提供商和模型的：

- 请求数、输入 token、输出 token 和总估算成本——跨时间窗口（每小时/每日/每月）。
- 估算成本与提供商报告使用量的对比（在可用时每日调用提供商使用量 API）。突出差异；它们通常表明用户在你的应用之外有额外流量，或你的定价表已过时。
- 团队工作区的每用户和每密钥细分，使管理员能看到哪个团队成员在驱动支出。

### 5.3 预算提醒和硬上限

让用户设置每密钥的每日和每月支出预算，带通知阈值（如 50%、80%、95%、100%）。达到硬上限时，停止为该密钥发出推理调用并显示清晰的"预算已达"屏幕；不要静默失败，也不要继续重试。

- 在发送请求*之前*实施预算检查（使用运行总计），而非之后。
- 对于流式响应，在块到达时跟踪 token 计数；如果流正在轨道上超出预算，干净地关闭连接并呈现部分响应。
- 也遵守提供商端预算：如果你检测到用户已设置 OpenAI 硬限制，永远不要让你的客户端重试队列推动他们超过它。

### 5.4 直通定价和你的费用

如果你在提供商成本之上收取自己的平台费用（而非纯 BYOK 模型，你单独为席位/功能计费），要明确说明计算方式。许多 BYOK 工具作为纯席位订阅运营，从不触及推理计费；这是最干净的模式，避免了受监管的资金传输问题。如果你确实添加附加费，在仪表板中显示单位经济学（"你向 OpenAI 支付 $X，我们收取 Y% 平台费 = 总计 $Z"）。

---

## 6. 常见陷阱

以下失败模式在 BYOK 产品的事后分析中反复出现。

**1. 在默认中间件中记录密钥。** 第一大根因。Web 框架、反向代理、API 网关和 APM 代理（New Relic、Datadog、Elastic APM）经常默认记录请求头和正文。显式配置允许列表日志字段，并添加正则脱敏（见 §3.3）作为纵深防御层。

**2. 在错误消息/栈跟踪中泄露密钥。** 当出站 HTTP 调用失败时，大多数 HTTP 库在异常对象中包含请求 URL 和头。如果这些对象冒泡到错误追踪器，密钥随之传播。在每个错误追踪 SDK 中注册全局清理器。

**3. 浏览器扩展中的客户端暴露。** 将用户密钥注入请求的浏览器扩展与用户访问的每个页面一起运行。带有 XSS 的恶意页面或具有过宽 `matches` 模式的 content script 可以从扩展存储中读取密钥。永远不要在 `localStorage`/`chrome.storage.local` 中存储明文密钥而不在用户密码短语下使用 `chrome.storage.local.set({encrypted: ...})`；优先使用 `session` 范围存储；且永远不要将密钥注入页面 DOM。

**4. CORS 谬误。** "我们直接让浏览器调用 OpenAI。" OpenAI 和 Anthropic 不设置 `Access-Control-Allow-Origin: *`，即使设置了，浏览器中的密钥意味着你页面上的任何脚本（包括第三方分析、广告脚本和被入侵的 npm 依赖）都可以窃取密钥。除非密钥仅在特权扩展后台 worker 或原生应用中持有和使用，否则通过后端路由。

**5. 混淆共享密钥和 BYOK 速率限制。** 当用户自带密钥时，速率限制应用于*该密钥在提供商处*，而非你的服务。共享代理池在你的密钥间分配负载；BYOK 意味着每个用户有自己的（通常低得多的）配额。清楚地沟通每密钥 RPM/TPM，在你的 UI 中展示 `retry-after` 和 `x-ratelimit-remaining` 头，并实现每*密钥*（而非全局）的客户端排队和指数退避。

**6. 未固定提供商主机名。** 如果你从用户输入构建提供商端点（如支持 Azure OpenAI 自定义 base URL 或 OpenAI 兼容网关），验证 base URL 在允许列表中，或至少其主机解析为公网 IP 并使用 TLS。恶意用户否则可以提供指向你内部元数据服务（169.254.169.254）的 URL 并通过 SSRF 外泄云凭证。

**7. 在环境变量或 .env 文件中本地存储密钥。** 当你团队的开发者本地运行 BYOK 代码时，他们可能会试图将测试密钥放入 `.env`。这些文件会被提交、屏幕共享和存档备份。即使在开发中也使用本地密钥存储（macOS Keychain、`pass`、`aws secretsmanager get-secret-value`）。

**8. 将所有提供商的密钥视为字符串。** GCP 服务账户是 JSON 文档；AWS Bedrock 偏好角色 ARN 而非长期密钥；Azure OpenAI 支持 Azure AD token；Anthropic 使用非标准头。仅字符串的抽象要么迫使用户将 JSON 粘贴到文本字段（破坏你的正则掩码），要么以后需要混乱的提供商特定代码路径。设计你的密钥表 schema 以容纳结构化凭证捆绑：`{type: "api_key"|"oauth"|"service_account"|"aws_role", ...fields}`。

**9. 当用户在提供商端撤销密钥后未在你这端撤销。** 当用户轮换密钥时，旧密钥可能在几分钟到几小时内仍然有效（取决于提供商缓存）。当旧密钥的验证探测开始失败时，及时删除密文。不要在数据库中留下无效密钥；它们变成虚假的安全感和杂乱来源。

**10. 重复计费或幻影使用量。** 如果你运行的重试逻辑不进行幂等去重，你可能意外发送相同请求多次并为用户未看到的重试计费。在提供商支持时使用 `idempotency` 头（OpenAI 在写端点支持 `Idempotency-Key`），并精确跟踪每请求 token 计费。

---

## 7. 替代方案

BYOK 并非总是正确选择。选择与你的用户、你运营计费的意愿和你的安全姿态匹配的模式。

### 7.1 何时 BYOK 是正确选择

- **开发者工具和 IDE 扩展**——目标受众已经拥有 API 密钥并习惯管理它们（如代码编辑器、本地开发工具、API playground、评估框架）。
- **企业/受监管客户**——需要使用自己的云租户以满足数据驻留、VPC-SC、HIPAA BAA 或 SOC 2 要求；BYOK（特别是 Bedrock/Vertex 上的跨账户角色假设）让他们将提示保留在自己的云边界内。
- **成本敏感的高级用户**——想直接与提供商谈判企业折扣并避免中间商加价。
- **早期初创公司**——不想预付六位或七位数余额给 LLM 提供商来提供服务。

### 7.2 何时共享代理 + 按用户计费更好

- **消费者应用和免费层产品**——入门摩擦必须接近零（"ChatGPT 没问我要密钥；你为什么要？"）。
- **应用的模型质量/性能需要密钥池、优先路由或你托管的微调模型**；代理让你在提供商和模型间进行智能路由。
- **当你需要在提示到达提供商之前集中执行内容策略、审核或 PII 清理时**。
- **当提供商条款要求时**（某些提供商经销商计划禁止 BYOK 风格的直通）。

在共享代理模型中，你维护一个密钥池（理想情况下按区域/工作负载分割），在自己的数据库中实现每用户计量，并通过现有订阅/开票系统向客户收费。你承担欺诈风险、信用卡退单和提供商速率限制管理；作为交换你控制全栈。

### 7.3 混合模式

大多数成熟的开发者工具公司最终提供混合方案：

1. **默认共享代理**（低摩擦入门，用户无需密钥即可试用产品）。
2. **BYOK 作为可选开关**在设置中为高级用户和企业提供，呈现为"使用你自己的 API 密钥"，附带关于成本、数据流和功能差异的清晰警告。
3. **企业 BYOK 集成 SSO/IAM**为大客户服务：Bedrock 上的跨账户 IAM 角色、GCP 上的 Workforce Identity Federation、Azure OpenAI 的 Azure Private Link。
4. **"自带项目"模式**在 OpenAI/Anthropic 上，用户为你的应用创建专用*项目*或*工作区*，使用受限密钥和（可选）SSO 绑定管理员；这给你一个干净的信任边界而无需降级到原始 API 密钥。
5. **纯本地模式**为注重隐私的用户，开源本地代理（类似 BYOKEY、LiteLLM 或 VS Code Copilot BYOK 守护进程）在设备上处理所有凭证，你的 SaaS 仅作为 UI/协调层。

### 7.4 决策清单

在承诺 BYOK 作为主要认证模式之前，问：

- 我们能否承受为数千租户存储不记名令牌的安全表面积？如果不能，从共享代理或零知识客户端模式开始。
- 我们是否有可观测性在几分钟内检测密钥泄露（审计日志、出站调用异常检测、出站控制）？如果没有，在启动 BYOK 之前构建它。
- 我们的支持团队能否跨 4+ 提供商认证系统调试认证失败？如果不能，从一两个提供商开始，仔细构建抽象。
- 我们的定价模型在没有 token 利润时是否合理？BYOK 在你的收入来自席位或功能（而非 token 套利）时效果最好。

---

## 8. 快速启动实施清单

将此作为上线关卡：

- [ ] 密钥通过 TLS 1.2+ 传输，永不出现在查询字符串中
- [ ] 使用托管 KMS 的信封加密静态存储；明文密钥永不被记录或持久化
- [ ] 具有最小权限 IAM 的专用出站锁定推理 worker
- [ ] 在存储任何密钥之前进行格式验证 + 对低成本端点的实时探测
- [ ] 密钥管理端点上的速率限制和锁定以防止预言机攻击
- [ ] 每个日志器、APM 和错误追踪器中密钥/令牌的全局脱敏
- [ ] 提示日志的显式选择加入/退出；提供零日志模式
- [ ] 优雅处理 401/403 并向用户提示重新验证密钥
- [ ] 支持 api-key、oauth、service-account、aws-role 的结构化凭证 schema
- [ ] 逐提供商特性文档（Anthropic 自定义头、Bedrock 仅 IAM、Vertex JSON/区域）
- [ ] 成本仪表板、调用前估算和预算提醒
- [ ] 清晰的隐私披露说明你的服务器看到什么 vs. 什么保留在客户端
- [ ] 经测试的密钥泄露事件响应手册（全局终止开关、批量撤销、客户沟通模板）
- [ ] 混合入门路径：试用共享代理，BYOK 作为升级路径

---

## 参考资料与扩展阅读

- OpenAI API Key Safety Best Practices: https://platform.openai.com/docs/guides/safety-best-practices
- Anthropic API Reference: https://docs.anthropic.com/en/api/getting-started
- Google Vertex AI Authentication: https://cloud.google.com/vertex-ai/docs/authentication
- AWS Bedrock IAM Security: https://docs.aws.amazon.com/bedrock/latest/userguide/security-iam.html
- AWS KMS Envelope Encryption: https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#enveloping
- LiteLLM (multi-provider gateway supporting BYOK patterns): https://github.com/BerriAI/litellm
- Open WebUI / many open-source AI UIs as reference BYOK implementations

---

*版本 1.0 — 2026 年 7 月。请随实施一起维护本文档；当提供商添加新认证机制（OpenAI 的 OAuth、Anthropic 细粒度工作区密钥、新 Bedrock/Vertex 模型系列）时，更新 §4 和决策清单。*
