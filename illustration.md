task 适配说明

# PinchBench 自动检查适配说明
本文档记录了 PinchBench 147 个任务的适配内容。适配共涉及 44 个任务（对应`task_introduction_evaluation_illustration.md`文件命名为`task_introduction_evaluation_illustration_adapted.md`）。

---

## 适配原则
1. **不注水** — 如果 agent 确实未完成某项检查，适配后的评分脚本仍然判定为失败。
2. **标准不变** — 数值阈值、关键词列表、评分档位均保持原样。



---

## A 类 · 证据来源恢复
**问题：** Agent 产出了正确的结果，因沙箱环境无法找到结果文件。

**修复：** 从结果来源恢复证据，评分逻辑本身不变。

**示例：**`task_workflow`**：**
Agent 通过 `write` 工具写入了脚本和笔记文件，但这些文件未导出到评分脚本预期的 workspace 目录。适配后的评分脚本从 `write` 工具调用记录中还原文件内容，再按原逻辑评分。

---

## B 类：格式容差
**问题：** Agent 的回答语义正确，但书写格式与评分脚本的刚性匹配模式不同导致评分较低。

**修复：** 调整匹配模式，接受同一正确值的等价写法，不引入新的可接受值。

### 数值格式
**示例：**`task_log_hdfs_slow_ops`

评分脚本要求精确匹配 `"91178"`，Agent 写的是带千分位的 `"91,178"`。适配后两种写法均接受——底层数值完全相同。

### Markdown 结构
**示例：**`task_meeting_council_votes`

评分脚本期望纯文本格式的投票数，Agent 以 Markdown 表格呈现——数据相同，格式不同。适配后从两种格式中均可提取数值。

<!-- ---

## C 类 后端不可达
**问题：** 部分任务需要与在线服务交互（GitHub API、Google Workspace、Kubernetes 集群），但这些服务在沙箱中不可用。Agent 展示了正确的操作意图，但无法实际完成。

**修复：** 对因后端不可达而受阻的步骤，上限给 0.5 分（认可正确意图）；成功完成的步骤仍获满分。

**示例：**`task_gh_issue_triage`
Agent 发出了正确的 `gh` CLI 命令来分类 issue，但 GitHub 服务器不可达。适配后的评分脚本从 trajectory 日志中读取命令验证正确性，对受阻步骤给予 0.5 分而非 0 分。 -->

---
