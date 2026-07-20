task 适配说明

# PinchBench 自动检查适配说明
本文档记录了 PinchBench 148 个任务的适配内容。适配共涉及 44 个任务。

---

## 适配分类总览
|类型|数量|说明|
|-|-|-|
|A · 证据来源恢复|13|评分逻辑不变，修复证据获取路径|
|B · 格式/结构容差|22|评分标准不变，兼容同值不同写法|
|C · 环境阻塞部分分|7|后端不可达时封顶 0.5 分|
|知情例外|2|经确认的评分点变更|

---

## A 类 · 证据来源恢复（13 个）
评分逻辑不变，仅修复三类环境失真导致的证据丢失：

* 空 transcript → 从 trajectory 复原工具调用
* 产物写到沙箱 HOME 未导出 → 从 write/read 工具调用复原
* 行号 gutter `N|` → 逐行剥离后再匹配

|任务|适配内容|
|-|-|
|task_calendar|从 ICS `DTSTAMP`/`CREATED` 取基准日代替 `datetime.now()`|
|task_git_rescue_recovery|剥离行号 gutter；git 路径回落|
|task_market_research|从 trajectory 复原工具调用|
|task_sanity|兼容 OpenAI 形 trajectory 格式|
|task_second_brain|`rglob("MEMORY.md")` 递归兜底|
|task_image_identification|`rglob` 定位 `image_categories.json`|
|task_memory|从 write 调用复原 `answer.txt`|
|task_workflow|从 write 调用复原脚本与 NOTES.md|
|task_test_generation|复原测试文件与被测模块后跑 pytest|
|task_csv_gdp_regions|从 agent stdout 重建报告文本|
|task_meeting_council_neighborhood|拼接脚本 + stdout 重建分析文本|
|task_cicd_pipeline_debug|重放 str-replace 编辑重建 ci.yml|
|task_log_syslog_auth_failures|rglob + trajectory 复原报告文件|

---

## B 类 · 格式/结构容差（22 个）
评分标准（阈值、词表、档位）不变，兼容同一正确值的不同书写形式。

### 数值格式（4 个）
|任务|适配内容|
|-|-|
|task_log_hdfs_slow_ops|`"91178"` → 正则 `91[,.]?178`，接受千分位|
|task_log_hdfs_storage|同上|
|task_log_nginx_slow_requests|同上|
|task_csv_gdp_ranking|百分比接受多位小数 `22.25%`|

### 词边界（3 个）
|任务|适配内容|
|-|-|
|task_meeting_blog_post|`um\b` → `\bum\b`，避免误伤 "momentum"|
|task_cron_organizer|`"mon"` → `\bmon\b`，避免命中 "month"|
|task_executive_lookup|允许中间名，如 "Jessica P. Ross"|

### Markdown 表格/标点（6 个）
|任务|适配内容|
|-|-|
|task_meeting_council_votes|接受表格形式的投票数|
|task_meeting_tech_decisions|接受中间有标点的短语|
|task_meeting_searchable_index|接受表格单元格形式的索引|
|task_meeting_advisory_acronyms|跳过数字序号列取缩写列|
|task_meeting_tech_action_items|剥离 gutter 恢复行首锚点|
|task_csv_cities_filter|增加等价措辞|

### 结构匹配优化（5 个）
|任务|适配内容|
|-|-|
|task_csv_stations_filter|收窄正则避免跨段误配|
|task_csv_pension_risk|接受精度差异与全称州名|
|task_playwright_e2e|数实际断言条数（而非惯用法种类）|
|task_email_triage|扫描所有匹配窗口取最优|
|task_log_apache_top_errors|接受 md 授权的第二种分组方式|

### 容差修复（4 个）
|任务|得分|适配内容|
|-|-|-|
|task_meeting_gov_next_steps|0.940|识别等价措辞；`aaro_annual_report` 保留 0 分|
|task_selector_fix|0.080|构造去注释视图；最终仍 0.08|
|task_multi_file_refactoring|0.267|加满分快捷判断（未触发，仍 0.267）|
|task_polymarket_briefing|—|接受等价新闻段落与赔率标签|

---

## C 类 · 环境阻塞部分分（7 个）
后端不可达时，对受阻步骤封顶给 0.5 分（成功仍给更高分）。

|任务|适配内容|
|-|-|
|task_gh_issue_triage|从 trajectory 读命令判分；服务器不可达时封顶 0.5|
|task_gws_email_triage|后端超时步骤封顶 0.5|
|task_gws_task_management|同上|
|task_gws_cross_service|同上 + 认 workspace 或 HOME 写入|
|task_k8s_debugging|从 response 文本读修复意图判分|
|task_iterative_code_refine|回落到 agent 的 review.txt 自证|
|task_files|下载丢失目录结构时给 0.5|

---

## 知情例外（2 个）
### task_image_gen · 1.000
运行时无 `generate_image` 工具。适配后接受 PIL 程序化渲染 + 实际产出文件作为替代方案（任务 md 明文授权 "a sound alternative if none exists"）。

### task_clawdhub · 1.000
模型多套一层 `datautils/` 目录。适配后用 `rglob` 定位文件。

---