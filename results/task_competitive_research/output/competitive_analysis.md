# Competitive Analysis: AI Code Assistants (2026)

> **GitHub Copilot** vs. **Cursor** vs. **Kilo Code** — Pricing, features, model support, privacy, and open-source status, with buying recommendations.

---

## Summary Comparison Table

| Dimension | GitHub Copilot | Cursor | Kilo Code |
|---|---|---|---|
| **Vendor / Type** | Microsoft / GitHub (closed-source IDE extension + cloud service) | Anysphere, Inc. (closed-source AI-native IDE, VS Code fork) | Kilo Org (community-led, MIT-licensed open-source VS Code / JetBrains extension + CLI) |
| **Free Tier** | ✅ Copilot Free — limited completions & chat, model access restricted | ✅ Hobby — ~2,000 Tab completions + ~50 slow premium requests/month | ✅ Free — extension itself is free; you pay only for model usage (Kilo API or BYOK); free credits for new users; several free models available (MiniMax, GLM-4.7-Flash, Qwen Code, etc.) |
| **Paid Tiers (Individual)** | Pro **$10/mo** (1,000 AI credits); Pro+ **$39/mo** (3,900 AI credits) | Pro **$20/mo** ($20 usage credits, unlimited Tab); Pro+ **$60/mo** ($60 credits); Premium **$120/mo**; Ultra **$200/mo** (20× Pro credits) | **No fixed subscription required.** Kilo API passes through provider pricing at cost (no markup); Kilo Pro Cloud plan available for hosted agents / code review / cloud sync (pricing by usage). |
| **Team / Enterprise** | Business **$19/user/mo** (1,900 AI credits, pooled); Enterprise **$39/user/mo** (3,900 AI credits, requires GitHub Enterprise Cloud → ~$60/user/mo effective). IP indemnity, SSO, content exclusion, audit logs, DLP. | Business **$40/user/mo** (annual $384/user/yr, ~$32/user/mo). SSO, admin dashboard, Privacy Mode enforced for teams, shared billing. | Self-hosted for free (MIT); Kilo Cloud team / enterprise offering via Kilo API with admin controls, no seat tax — pay only for tokens used. |
| **Billing Model (June 2026+)** | **Subscription + AI credits**. Code completions & Next Edit Suggestions stay free/unlimited; Chat, Agent Mode, CLI, Spaces, code review, and Spark consume credits. Overages billed at $0.01/credit. | **Subscription + Usage Credits** since mid-2025. Tab completions unlimited; premium models and Composer / Agent consume credits. Slow queue fallback removed for Pro in 2025. | **Pay-as-you-go by token.** The extension itself is free; users bring their own keys (BYOK) or use Kilo API, which charges provider cost with zero platform markup. |
| **Code Completion** | ✅ Ghost-text inline completion (Tab); Next Edit Suggestions (NES); multi-language, very polished. | ✅ Cursor Tab — very fast, project-aware ghost completions; highest reported Tab-accept rate (~30%). | ✅ Kilo Complete (ghost-text) via supported models; completion quality depends on chosen model; fast with small local models. |
| **Chat (Q&A)** | ✅ Copilot Chat (side panel + inline), @workspace for repo-wide Q&A, slash commands. | ✅ Cmd/Ctrl+L chat; @-mention files, folders, docs, web; Canvas for long-form text; Deep Research. | ✅ Chat panel with Ask / Code / Architect / Debug / Orchestrator modes; can read & edit files, run terminals. |
| **Multi-file Editing** | ✅ Copilot Edits (multi-file apply across workspace; less aggressive than Cursor Composer). | ✅ **Composer (Cmd+I)** — flagship multi-file editing; can simultaneously modify components, models, schemas, API routes coherently. | ✅ Native multi-file edits through Code/Orchestrator agents; diff-based apply with per-change approval. |
| **Agent Mode** | ✅ Copilot Agent Mode (Beta → GA in 2026); Plan agent + custom agents in Visual Studio; can edit files, run terminal commands, iterate. | ✅ Agent / Composer agent capabilities; can run terminal commands and browser automation; background tasks. | ✅ Full autonomous agent out of the box — terminal execution, browser automation, file edits, self-check/auto-fix loops; `kilo run --auto` for unattended CI/CD. |
| **Tool Use / MCP** | ✅ MCP support in preview; Copilot Extensions (beta) for building internal tools; integrates with GitHub Issues/PRs/Actions natively. | ⚠️ Limited tool-calling inside Composer/Agent; MCP support emerging (not first-class). | ✅ **MCP Marketplace built in** — discover & connect MCP servers for GitHub, databases, APIs, browsers; custom Skills marketplace for extending agent behavior. |
| **Model Flexibility** | Multiple built-in models: GPT-4o/5 family, Claude Sonnet/Opus, Gemini 2.x, Grok; BYOK supported in Copilot SDK (public preview) and in JetBrains/Xcode chat (Anthropic, Azure, Gemini, Groq, OpenAI, OpenRouter). | Built-in models switchable: GPT-4.1/5, Claude Opus 4 / Sonnet 4.5, Gemini 2.5 Pro, Grok; BYOK possible via "custom provider" (OpenAI-compatible endpoint) — requires a paid plan; third-party BYOK proxies also popular. | **500+ models** out of the box via OpenRouter, Kilo API, and direct providers: GPT-5.5, Claude Opus 4.7 / Sonnet 4.6, Gemini 3.1 Pro, Grok Code Fast 1, MiniMax M2.5, GLM-4.7, Qwen Code, Kimi K2.5, MiMo V2, DeepSeek, Ollama/local models, etc. Full BYOK; OpenAI-compatible custom endpoints; per-task model switching. |
| **BYOK (Bring Your Own Key)** | ✅ Yes — via Copilot SDK (all plans) and Chat model picker in JetBrains/Xcode (preview). Supports OpenAI, Azure/Foundry, Anthropic, Ollama, any OpenAI-compatible endpoint. | ⚠️ Partial — Custom OpenAI-compatible endpoint supported, but locked behind a paid subscription; popular proxies (Cursor++, OpenRouter relays) used in the wild. | ✅ **First-class.** Designed for BYOK: plug in OpenAI, Anthropic, Google, xAI, OpenRouter, Ollama, LM Studio, vLLM, Azure, Tencent Token Plan, Aliyun Bailian, DeepSeek, Moonshot, etc. No subscription needed if using your own keys against a local/remote endpoint. |
| **Local / Offline Models** | ⚠️ Indirectly via BYOK to Ollama/Foundry Local; default offering is cloud-only. | ⚠️ Via BYOK → Ollama/OpenAI-compatible endpoint; not a supported first-class flow. | ✅ Native Ollama / LM Studio / vLLM support; can run 100% offline with local models (e.g., Qwen2.5-Coder, Llama-3.1, DeepSeek-Coder). |
| **Privacy / Data Collection** | **Individual plans (Free/Pro/Pro+):** GitHub announced that starting April 24, 2026, interaction data (prompts, outputs, code snippets, file context, filenames) is **collected by default and may be used for model training**; users can opt out in Settings → Copilot → Privacy. **Business/Enterprise:** data is excluded from training by default; offers IP indemnity, content exclusion, private endpoints, audit logs. All completions go through Azure OpenAI; sensitive-pattern filters block secrets. | Default: prompts & code may be sent to Cursor's servers and model providers; free-tier data may be retained. **Privacy Mode** (toggle in Settings or `privacy.mode: true`) prevents code from being stored/used for training and requires providers to honor Zero Data Retention (ZDR). Business plan enforces Privacy Mode for all seats. Can disable cloud embedding sync and use local embedding models. Sensitive-pattern filtering available. | **Open-source, fully auditable.** Code leaves your machine **only** when you call a remote provider you configured. No mandatory telemetry to Kilo Org when using BYOK with your own endpoint/local model; with Kilo API, standard provider privacy terms apply. You control which directories are indexed; self-hosted agents never exfiltrate code. |
| **Can Run Without Sending Code to the Cloud?** | ⚠️ Only via BYOK → a local model (Ollama / Foundry Local); default Copilot requires cloud. | ⚠️ Only via BYOK → local endpoint; Privacy Mode prevents retention but code still hits provider APIs to generate responses. | ✅ Yes — point at Ollama/LM Studio/vLLM on localhost and Kilo Code runs fully offline. |
| **Open Source / License** | ❌ Closed source (proprietary). | ❌ Closed source (proprietary binary distribution; built on open-source VS Code but editor itself is not OSS). | ✅ **MIT License** (full source on GitHub: Kilo-Org/kilocode, ~25k stars as of July 2026). Forked from Roo Code (which forked from Cline). |
| **Self-Hostable?** | ❌ Not self-hostable; BYOK + Azure Private Link approximates enterprise isolation. | ❌ Not self-hostable; must use Cursor binaries and accounts. | ✅ Fully self-hostable from source; VS Code extension, JetBrains plugin, CLI (`kilo` / `@kilocode/cli`), Cloud Agent web UI, and KiloClaw (always-on agent) all run locally or on your own infra. |
| **IDE Support** | VS Code, Visual Studio, JetBrains IDEs, Neovim (preview), Xcode, Eclipse (coming). Most comprehensive IDE coverage. | Cursor only (VS Code fork, ships as its own editor); VS Code settings/extensions importable. | VS Code, JetBrains (IntelliJ, PyCharm, WebStorm, Android Studio…), CLI (macOS/Linux/Windows binaries, Homebrew, AUR, npm), Web (app.kilo.ai/cloud), code-review bot, KiloClaw long-running agent. Works inside Cursor itself as an extension. |
| **Notable Strengths** | Mature ecosystem, tight GitHub integration (PRs, Issues, Actions), best inline-completion polish, strong enterprise compliance (IP indemnity, SSO, audit), BYOK via Copilot SDK. | Best out-of-the-box "AI pair programmer" experience; Composer multi-file edits; strongest Tab-completion UX; AI-native IDE. | Maximum flexibility, MIT-licensed, no lock-in, full BYOK, 500+ models, built-in MCP & Skills marketplaces, multi-agent Orchestrator, CLI + web + IDE coverage, runs fully offline. |
| **Notable Weaknesses** | Agent Mode still maturing vs. competitors; April 2026 default data-retention policy change upset individual users; Enterprise requires GEC bundle → expensive. | Closed source; pricing has shifted from all-you-can-eat to credit-based; Ultra plan hits $200/mo; BYOK is paywalled. | UX rougher around edges (community project); OSS bugs (freezes, index crashes reported); no polished "just works" experience of Cursor/Copilot; responsible for own model costs / rate limits. |

---

## 1. GitHub Copilot

### Pricing (as of mid-2026, post June 1 credit-based rollout)

GitHub Copilot moved from request-based billing to a **subscription + AI Credits** model effective June 1, 2026. Monthly subscription prices stayed flat, but each tier now bundles a fixed pool of credits; advanced features (Chat, Agent Mode, CLI, Spaces, Spark, code review) consume credits, while inline completions and Next Edit Suggestions remain free and unlimited on all paid plans.

- **Copilot Free** — $0/month. Limited completions and chat per month; model choice restricted; primarily for students, teachers (verified), and casual tinkerers.
- **Copilot Pro** — **$10/month** (annual discount ~$100/year). Includes **1,000 AI credits/month** (~$10 value); full model picker for individuals.
- **Copilot Pro+** — **$39/month**. Includes **3,900 AI credits/month**; higher quotas for Agent Mode and premium models.
- **Copilot Business** — **$19 per user/month**. **1,900 AI credits per seat**, pooled at org level; user management, SAML SSO, IP indemnification, content exclusion, public-code filtering, audit logs, policy controls; data excluded from training by default.
- **Copilot Enterprise** — **$39 per user/month**, **requires GitHub Enterprise Cloud** (which adds ~$21/user/month, pushing the effective per-seat cost to roughly **$60/user/month**). **3,900 AI credits per seat**, pooled; org-wide knowledge bases, custom chat grounded in internal repos, Copilot Extensions (private beta), Azure Private Link, FedRAMP / data-residency options (adds 10% premium), priority model access, and full IP indemnity.
- **Promo period (June–August 2026):** Business and Enterprise orgs receive promotional credit bumps (Business ~$30, Enterprise ~$70) during the transition.
- **Overages:** $0.01 per AI credit above the bundle, configurable at the user/cost-center/org level with hard budget caps. Code review additionally consumes GitHub Actions minutes after June 1, 2026.

### Features

- **Inline completion** — the original and still one of the most polished ghost-text experiences; Tab-accept latency is fast; supports nearly every mainstream language out of the box.
- **Next Edit Suggestions (NES)** — predicts your next logical edit after you accept a completion.
- **Copilot Chat** — side panel and inline (`Ctrl+I`) chat with slash commands, `@workspace` to reference the entire repo, and a model picker.
- **Copilot Edits** — multi-file editing across the workspace; more conservative than Cursor Composer but works directly inside VS Code/Visual Studio.
- **Agent Mode** (GA in 2026) — the agent can edit files, run terminal commands, re-run tests, and iterate. In Visual Studio specifically, Microsoft shipped **Plan Agent** (plans before editing) and **Custom Agents** (curated built-in agents for debugging/profiling/testing plus a framework for team-built agents).
- **Deep GitHub integration** — PR summaries, code review, Issue-to-PR workflows, Copilot CLI, Copilot Spaces, Spark (in-repo docs/assistants), and Actions-native code review.
- **MCP / Extensibility** — MCP support is in preview; Copilot Extensions (public beta) let teams build internal tools; Copilot SDK enables programmatic sessions.
- **Security** — built-in secret-pattern detection (blocks AWS keys, SSH private keys, etc.), content exclusion (files/paths never sent), public-code filter.

### Model Support

Copilot no longer relies solely on OpenAI models. As of 2026 the built-in model picker includes a rotating lineup of:
- OpenAI **GPT-4o**, **GPT-4.1**, and **GPT-5-family** models
- Anthropic **Claude Sonnet 4/4.5**, **Claude Opus 4**
- Google **Gemini 2.x Pro**
- xAI **Grok** variants
- Azure OpenAI-hosted models

**BYOK (Bring Your Own Key)** is supported in two forms:
1. **Copilot SDK** (public preview, all plans): accepts OpenAI, Azure AI Foundry, Anthropic, Ollama, Microsoft Foundry Local, and any OpenAI-compatible endpoint (vLLM, LiteLLM, etc.).
2. **Chat model picker in JetBrains/Xcode** (preview): supports Anthropic, Azure, Google Gemini, Groq, OpenAI, and OpenRouter; Eclipse support is coming.

### Privacy & Data

- **For Free / Pro / Pro+ individuals:** Starting April 24, 2026, GitHub **collects Copilot interaction data by default** (prompts, outputs, code snippets around the cursor, filenames, repository structure, chat history, thumbs up/down) and may use it to train AI models. Users can **opt out** at `github.com/settings/copilot/features` → "Allow GitHub to use my data for AI model training" = Disabled. Static private-repo code at rest is not scanned; only live Copilot interactions are captured.
- **For Business / Enterprise:** Data is excluded from model training by contract; IP indemnity is provided; enterprise-grade controls include SSO, audit logs, content exclusion, DLP, Azure Private Link, data residency (with 10% uplift), and SOC 2 / GDPR compliance.
- **Telemetry** in the IDE can be disabled via `enableTelemetry: false` in Copilot advanced settings, which removes the client ID header.
- **Local-only operation** is not native but achievable via BYOK pointed at a local endpoint (Ollama, Foundry Local), though inline completions still expect a cloud-side model in the default setup.

### Open Source & Self-Hosting

Copilot itself is **closed-source, proprietary software**. The Copilot SDK is open for developers to build against, but the service, models, and editor extensions cannot be self-hosted. Enterprises approximate self-hosting via Azure Private Link / Azure AI Foundry deployments.

---

## 2. Cursor

### Pricing (as of mid-2026)

After mid-2025's much-discussed shift from "500 fast requests + unlimited slow" to **subscription + usage credits**, Cursor expanded into several tiers in 2026:

- **Hobby (Free)** — $0/month. ~2,000 Tab completions + ~50 slow premium model requests per month; enough for evaluation and very light hobby use.
- **Pro** — **$20/month** (annual: ~$192/yr, ≈$16/mo). Unlimited Tab completions + **$20 of usage credits** per month for premium models, Composer, and Agent.
- **Pro+** — **$60/month**. Unlimited Tab + **$60 credits**; aimed at developers who regularly use Composer on large refactors.
- **Premium** — **$120/month** (introduced June 2026); higher credit pool for unpredictable heavy users.
- **Ultra** — **$200/month**; **20× the Pro credit allocation**, PR indexing (AI parses pull requests, conflict impact), early access to new features; targets power users and small teams.
- **Business / Teams** — **$40/user/month** (annual ~$384/user/yr ≈$32/user/mo). Admin console, SSO, unified billing, Privacy Mode enforced by default for all seats.

All paid tiers keep **Tab completions unlimited**. Credits are consumed when using premium models (Claude Opus, GPT-5, etc.), Composer multi-file generation, and Agent mode. The "infinite slow queue" that made 2024 Cursor a steal no longer exists, which led to widespread community backlash and a partial refund/credit policy after the June 2025 pricing change.

### Features

- **AI-native IDE** — Cursor is a fork of VS Code, so migration from VS Code is near-zero friction; settings, keybindings, and most extensions import cleanly.
- **Cursor Tab** — widely regarded as one of the fastest, most context-aware inline completion engines; ~30% Tab-accept rate is commonly reported.
- **Cmd+K (inline edit)** and **Cmd+L (chat)** are the core muscle-memory shortcuts.
- **Composer (Cmd+I)** — Cursor's headline multi-file editing feature. Describe a feature (e.g., "add a favorites feature") and it generates components, updates models/schemas, and adds API routes in one coherent pass, respecting cross-file references.
- **Canvas** — a Notion-like document surface inside the editor for iterating on docs, proposals, and long-form text alongside code.
- **Deep Research** — built-in web research that pulls fresh technical docs for up-to-date answers.
- **Agent capabilities** — terminal command execution, browser automation, background/long-running tasks; PR indexing is an Ultra-only feature.
- **@-mentions** — files, folders, docs, symbols, web results can be referenced directly in chat.

### Model Support

Cursor's built-in model picker gives access to the major frontier models:
- OpenAI **GPT-4.1**, **GPT-5** family
- Anthropic **Claude Sonnet 4/4.5**, **Claude Opus 4**
- Google **Gemini 2.5 Pro**
- xAI **Grok**

Cursor selects a default "Auto" model that routes to whichever model is fastest/cheapest for the task; higher-tier plans unlock Opus-tier models without throttling.

**BYOK** is supported via **Settings → Models → Custom Provider**, where you can plug in an OpenAI-compatible endpoint (e.g., OpenRouter, DeepSeek, Azure, a proxy like Cursor++ that redirects to arbitrary providers). However, this feature requires a **paid subscription** even though you are supplying your own keys; the Free tier locks the model selector. Community projects (Cursor++, various OpenRouter relays) have sprung up to work around this and to add Anthropic-native endpoints.

Local/offline models are possible by pointing the custom provider at Ollama or LM Studio, but this is not a first-class, supported configuration — indexing and embeddings still round-trip through Cursor servers unless extra settings are changed.

### Privacy & Data

- **Default behavior:** prompts and code snippets may be sent to Cursor's servers and to model providers; free-tier data may be retained for product improvement.
- **Privacy Mode** (toggle in Settings → General → Privacy, or `"privacy.mode": true` in `settings.json`) enforces Zero Data Retention (ZDR) agreements with OpenAI, Anthropic, Google, and xAI: your prompts/code are not stored or used for training.
- **For Business/Teams:** Privacy Mode is **enforced by default** for all members.
- **Additional hardening options:**
  - Whitelist which directories can be indexed (`cursor.codebase.whitelist`)
  - Disable cloud sync/embedding upload (`cursor.cloud.sync: false`)
  - Use local embedding models instead of OpenAI embeddings API
  - Merkle-tree-based incremental indexing to avoid re-uploading unchanged code
- **Important caveat:** Even with Privacy Mode on, your code is still sent to model-provider APIs to generate completions; Privacy Mode prevents *retention*, not *transmission*. For true offline use, you must BYOK to a local endpoint and also disable cloud embedding.

### Open Source & Self-Hosting

Cursor is **closed source**. You cannot self-host the editor or its backend services. Users depend on Anysphere's servers for authentication, indexing, and (by default) model routing.

---

## 3. Kilo Code

### Pricing

Kilo Code (from Kilo Org, ~25k GitHub stars as of July 2026, MIT-licensed, v7.4.1 at time of writing) has the most unusual pricing model of the three: **the extension, CLI, and source code are free. You pay only for the AI tokens you consume, either through your own provider keys or via Kilo API, which charges provider cost with no markup.**

- **Extension & CLI** — **free and open source**, MIT license. Download from the VS Code Marketplace, JetBrains Marketplace, `npm i -g @kilocode/cli`, Homebrew (`brew install Kilo-Org/tap/kilo`), AUR (`paru -S kilo-bin`), or pre-built binaries for Windows/macOS/Linux.
- **Free credits / free models** — New Kilo API users get a free trial credit. Several built-in providers offer free tiers, including **MiniMax M2.5** (became Kilo's default model in April 2026), **GLM-4.7-Flash**, and **Qwen Code**. For light use, you can literally code "for free" against these free endpoints.
- **Kilo API (paid use)** — pass-through pricing: **500+ models** billed at each provider's official per-token rates, with zero commission. Real-time price estimates are shown in the UI (based on >300B daily tokens of real-world usage, including cache discounts).
- **Kilo Cloud / Pro** — cloud agent (`app.kilo.ai/cloud`), automatic PR code review (`app.kilo.ai/code-reviews`), and KiloClaw (always-on agent, `app.kilo.ai/claw`) are usage-metered services without a seat-based subscription tax.
- **BYOK** — works with no account and no subscription. Plug in OpenAI, Anthropic, Google, xAI, OpenRouter, DeepSeek, Moonshot (Kimi K2.5), Zhipu (GLM), MiniMax, Qwen, Alibaba Bailian, Tencent Token Plan, Xiaomi MiMo, Ollama, LM Studio, vLLM, or any OpenAI-compatible endpoint.

The contrast with Copilot/Cursor is stark: there is no "$20/mo buys you X credits" gate. Heavy users still pay for premium models, but they pay exactly what the model costs and can route to cheaper/open models whenever they like.

### Features

Kilo Code is built on the shoulders of Cline and Roo Code (MIT-licensed predecessors) and extends them aggressively:

- **Multi-IDE + CLI + Web** — VS Code extension, JetBrains (IntelliJ, PyCharm, WebStorm, Android Studio, etc., v2024.3+), CLI (`kilo` command), web Cloud Agent, code-review bot, and KiloClaw always-on agent. It also runs *inside* Cursor as a VS Code extension.
- **Five built-in agent modes (swappable)** — Architect (design/planning), Orchestrator (decomposes large tasks and dispatches sub-agents in parallel), Code (writes/edits code), Debug (diagnoses & fixes), Ask (codebase Q&A without editing). You can build custom agents/modes.
- **Multi-file, multi-step agent** — edits files, runs terminal commands, automates a browser (Playwright-style), installs dependencies, runs tests, and self-checks its own work (attempts to fix errors it introduces). Sub-agents can be spawned concurrently; users report delivering small end-to-end projects (frontend + backend + DB + tests) in under two hours.
- **Kilo Complete** — ghost-text Tab completion, competitive with Cursor Tab when backed by a fast model.
- **MCP Marketplace (built-in)** — first-class MCP server browser and installer, making it trivial to hook up GitHub, databases, browser automation, file systems, internal APIs, and any MCP-compliant tool.
- **Skills Marketplace** — installable agent plugins for repetitive workflows (PR review, scaffolding, deploy scripts, etc.).
- **Memory Bank** — persistent project memory that survives across sessions.
- **Autonomous CI/CD mode** — `kilo run --auto` disables all permission prompts for headless, fully autonomous runs in CI pipelines (use only in sandboxed/trusted environments).
- **Cloud agent & code review** — `app.kilo.ai/cloud` runs Kilo in a browser; `app.kilo.ai/code-reviews` auto-reviews PRs; KiloClaw provides a 24/7 long-running agent.

### Model Support

Kilo Code is model-agnostic to an extreme:
- **500+ models** directly accessible through the UI — GPT-5.5, Claude Opus 4.7, Claude Sonnet 4.6, Gemini 3.1 Pro Preview, Grok Code Fast 1, MiniMax M2.5 (default since April 2026), GLM-4.7/4.7-Flash, Qwen Code, Kimi K2.5, MiMo V2 Pro/Omni, DeepSeek variants, and many more.
- **30+ providers** via direct API and OpenRouter: OpenAI, Anthropic, Google, xAI, OpenRouter, MiniMax, Zhipu AI, Alibaba Qwen/Bailian, Tencent Token Plan, Moonshot Kimi, Xiaomi MiMo, DeepSeek, Ollama, LM Studio, vLLM, Azure OpenAI, any OpenAI-compatible custom endpoint.
- **Per-task model switching** — pick a cheap fast model (MiniMax, GLM-4.7-Flash) for scaffolding and a heavyweight (Opus 4.7, GPT-5.5) for tricky debugging, all within one session.
- **Local/offline first-class support** — Ollama, LM Studio, vLLM, and Foundry Local are supported natively, no proxy required.
- **Transparent cost** — the UI shows per-million-token cost for every model, factoring in real-world cache hit rates from Kilo's aggregate usage data.

### Privacy & Data

- **Open source and auditable** (MIT) — you can inspect exactly what the extension sends over the network.
- **No mandatory data collection to Kilo Org.** When you use BYOK with your own endpoint or with a local model, your code does not touch any Kilo-operated server.
- **When using Kilo API**, requests go through Kilo's gateway to the underlying provider under that provider's privacy terms; Kilo passes the traffic through without retaining snippets for training (per their docs).
- **Directories under your control** — you decide which folders are indexed and which tools/MCP servers the agent can connect to. Permission prompts appear before file writes and command execution (unless you explicitly enable `--auto`).
- **100% offline operation** is genuinely possible: run Ollama locally, point Kilo Code at `http://localhost:11434`, and the tool works with zero outbound network traffic.

### Open Source & Self-Hosting

- **License:** MIT (full text in the repo). You are free to use, modify, redistribute, and use commercially, with attribution.
- **Source:** `https://github.com/Kilo-Org/kilocode` — active development, community PRs, weekly releases.
- **Self-hosting:** The VS Code extension, JetBrains plugin, and CLI can all be built from source (`pnpm install && pnpm build && pnpm vsix`). The Cloud Agent web UI and KiloClaw server can be self-hosted as well. There is no mandatory cloud dependency for the core coding experience.
- **Funding:** Founded in early 2025 by a team including ex-GitLab co-founder/CEO Sid Sijbrandij; announced an **$8M seed round** in December 2025 (Breakers, Cota Capital, General Catalyst, Quiet Capital, Tokyo Black), which funds ongoing development while the core remains MIT-licensed.
- **Known caveats:** As a fast-moving community project, users report occasional bugs (UI freezes, indexer crashes, checkpoint reliability, multi-instance conflicts), and UX polish is a notch below Cursor/Copilot.

---

## Bottom Line: Recommendations by User Profile

### 🧑‍💻 Hobbyist / Student / Tinkerer
**Go with Kilo Code (free + BYOK + free models)**, or Copilot Free if you just want the easiest possible Tab completion.

- **Kilo Code** costs you nothing to install, works out of the box against free-tier models (MiniMax, GLM-Flash, Qwen Code), and supports Ollama if you want to experiment with local models on a home GPU. It's the only option where you can genuinely build real projects without ever pulling out a credit card.
- **Copilot Free** is the easiest "set it and forget it" option if you're learning and want zero setup, though its model choice and quota are limited.
- **Cursor Free** is generous enough to evaluate, but the Free tier's slow-queue and request cap will frustrate anyone coding regularly.

### 🚀 Startup Developer / Solo Builder / Power User
**Pick Cursor Pro ($20/mo) if you want the most polished "AI pair programmer" UX, or Kilo Code + BYOK (OpenRouter/your own keys) if you want maximum flexibility and control.**

- **Cursor Pro** is the productivity sweet spot for most full-time developers in 2026. Composer's multi-file coherence is best-in-class, Tab is slick, and $20 is cheap if it saves you an hour a week. Step up to **Pro+ ($60)** or **Ultra ($200)** if you're routinely doing big refactors and burning through credits. Be mindful of the credit-based billing — Opus-4 work can burn through $20 faster than you'd expect.
- **Kilo Code + BYOK** is the pick for tinkerers, polyglots, and anyone who wants to arbitrage model prices (cheap fast models for scaffolding, expensive models only for the hard bits). It also gives you MCP, custom skills, multi-agent Orchestrator, and CLI automation that Cursor doesn't yet match. Many power users run **both**: Cursor as the daily editor, Kilo Code as an extension inside it for MCP/agent jobs and cheap model access.
- **Copilot Pro ($10/mo)** is a strong "value" tier — its Tab completions and NES are the most polished, and at $10 it's a cheap add-on alongside any other tool. If you live mostly in VS Code and don't need Cursor-level Composer, Copilot Pro alone can be plenty.

### 🏢 Enterprise Team / Regulated Industry
**GitHub Copilot Enterprise is the safest "check the compliance boxes" choice, but Kilo Code self-hosted is the pick for air-gapped or cost-sensitive teams. Cursor Business is a solid middle ground for startups that prioritize velocity.**

- **Copilot Enterprise (~$60/user/mo effective with GEC)** is the only option here that ships with legal-grade **IP indemnity**, **SAML SSO**, **audit logs**, **content exclusion**, **Azure Private Link**, **FedRAMP / data residency**, and a Microsoft-backed enterprise SLA. If you're in finance, healthcare, or government and your CISO signs the checks, this is the default answer. Be aware of the April 2026 policy change and explicitly disable training use for individual accounts if you extend Pro/Pro+ to employees; Business/Enterprise are already opted out by default.
- **Cursor Business ($40/user/mo)** is excellent for engineering-led startups and mid-sized teams who value velocity over compliance checkboxes. Privacy Mode is team-enforced, SSO/admin/billing are solid, and Composer is a genuine productivity multiplier. You will not get Microsoft-grade IP indemnity, and you can't self-host.
- **Kilo Code (self-hosted / BYOK)** is the only realistic option for **air-gapped / offline / highly sensitive environments**. Point it at an internal vLLM/Ollama deployment, run the Cloud Agent on your own infrastructure, use MCP to connect to internal tools, and zero code ever leaves your network. Total cost is model inference cost only — no seat tax. It requires more engineering effort to operate and the UX isn't as polished, but for defense, financial infra, or teams that care about lock-in, it's the clear answer. It also works as a **complement** to Copilot Enterprise for teams that want MCP, custom agents, or local-model experimentation on top of Microsoft's compliance baseline.

### TL;DR Decision Tree

- "I just want Tab to finish my lines with minimal setup" → **Copilot Pro**
- "I want the best AI-native editor and multi-file edits today" → **Cursor Pro**
- "I want to pick any model, run offline, self-host, pay only for tokens, and hack on the tool itself" → **Kilo Code**
- "My legal/security team needs to sign off on everything" → **Copilot Enterprise**
- "I want all of the above" → Run **Copilot Pro for the Tab polish + Kilo Code for agent/MCP/BYOK flexibility** inside VS Code; add Cursor Pro for the weeks you're doing heavy greenfield work.

---

*Sources: GitHub official billing documentation (June 2026 update), Microsoft DevBlogs Visual Studio/Copilot release notes, Cursor pricing/settings documentation and 2025–2026 plan revisions, Kilo Code GitHub repository (Kilo-Org/kilocode) and official docs, Tencent Cloud / Aliyun Token Plan integration guides for Kilo Code, and multiple independent 2026 reviews and usage reports. Pricing and feature availability evolve quickly — verify with each vendor's official pricing page before making a purchase.*
