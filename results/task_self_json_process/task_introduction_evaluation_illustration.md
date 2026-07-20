---
id: task_self_json_process
name: JSON Task Schedule Planning
category: data_processing
grading_type: hybrid
timeout_seconds: 180
grading_weights:
  automated: 0.7
  llm_judge: 0.3
workspace_files:
  - source: json/E3_tasks_schedule.json
    dest: E3_tasks_schedule.json
---

## Prompt

我有一个任务列表文件 `E3_tasks_schedule.json`，请读取该文件内容，根据任务优先级、截止日期、预估耗时和日历事件，帮我合理安排任务的执行计划，输出为 `E3_plan.md` 文件。

要求：
1. 按日期排列执行计划，考虑已有日历事件的时间占用
2. 高优先级任务优先安排
3. 考虑用户偏好（专注时间段、会议缓冲、每日最大任务数）
4. 输出为清晰的 Markdown 格式，包含每日计划表

---

## Expected Behavior

The agent should:

1. Read and parse the JSON file containing tasks, calendar events, and user preferences
2. Sort tasks by priority (P0 > P1 > P2 > P3) and deadline
3. Schedule tasks around existing calendar events, respecting time constraints
4. Consider user preferences: morning focus time, 15-min meeting buffer, max 5 tasks/day
5. Output a well-structured markdown plan file `E3_plan.md`

Key scheduling logic:
- 6 tasks total, estimated 17 hours across ~7 working days (3/16 - 3/25)
- P0 task "完成项目报告" (4h, due 3/18) must be scheduled before 3/18
- P1 tasks "回复客户邮件" (1h, due 3/16) and "代码评审" (2h, due 3/17) are urgent
- Calendar events on 3/16 (周例会 10-11, 客户沟通 14-15:30), 3/17 (集中开发 9-12), 3/18 (项目汇报 15-16)
- User prefers morning focus time

---

## Grading Criteria

- [ ] Output file `E3_plan.md` is created
- [ ] Plan covers all 6 tasks from the input
- [ ] Tasks are ordered by priority (P0/P1 before P2/P3)
- [ ] Calendar events are acknowledged/avoided in scheduling
- [ ] Deadlines are respected (no task scheduled after its deadline)
- [ ] User preferences considered (focus time, buffer, max tasks)
- [ ] Plan is organized by date
- [ ] Markdown formatting is clear and readable

---

## Automated Checks

```python
def grade(transcript: list, workspace_path: str) -> dict:
    """
    Grade the JSON task schedule planning task.

    Args:
        transcript: Parsed JSONL transcript as list of dicts
        workspace_path: Path to the task's isolated workspace directory

    Returns:
        Dict mapping criterion names to scores (0.0 to 1.0)
    """
    from pathlib import Path
    import re

    scores = {}
    workspace = Path(workspace_path)

    # Check output file exists
    plan_path = workspace / "E3_plan.md"
    if not plan_path.exists():
        # Try case-insensitive
        candidates = list(workspace.glob("*plan*"))
        candidates += list(workspace.glob("*Plan*"))
        if candidates:
            plan_path = candidates[0]
        else:
            return {
                "file_created": 0.0,
                "all_tasks_covered": 0.0,
                "priority_ordering": 0.0,
                "calendar_awareness": 0.0,
                "deadline_compliance": 0.0,
                "date_organized": 0.0,
            }

    scores["file_created"] = 1.0
    content = plan_path.read_text(encoding="utf-8", errors="replace")
    content_lower = content.lower()

    # Check all 6 tasks are mentioned
    task_names = [
        "完成项目报告", "回复客户邮件", "代码评审",
        "更新文档", "团队分享准备", "学习新技术",
    ]
    found_tasks = sum(1 for t in task_names if t in content)
    scores["all_tasks_covered"] = found_tasks / len(task_names)

    # Check priority ordering: P0/P1 tasks should appear before P2/P3
    high_priority = ["完成项目报告", "回复客户邮件", "代码评审"]
    low_priority = ["更新文档", "团队分享准备", "学习新技术"]
    hp_positions = [content.find(t) for t in high_priority if t in content]
    lp_positions = [content.find(t) for t in low_priority if t in content]
    if hp_positions and lp_positions:
        # Average position of high-pri should be before low-pri
        avg_hp = sum(hp_positions) / len(hp_positions)
        avg_lp = sum(lp_positions) / len(lp_positions)
        scores["priority_ordering"] = 1.0 if avg_hp < avg_lp else 0.5
    elif hp_positions:
        scores["priority_ordering"] = 0.75
    else:
        scores["priority_ordering"] = 0.0

    # Check calendar awareness (mentions meetings/events)
    calendar_keywords = ["周例会", "客户沟通", "集中开发", "项目汇报", "会议", "calendar", "日历"]
    calendar_found = sum(1 for k in calendar_keywords if k in content)
    scores["calendar_awareness"] = min(1.0, calendar_found / 2.0)

    # Check deadline compliance: dates should be present and logical
    date_pattern = re.compile(r"3[/-]1[6-9]|3[/-]2[0-5]|03[/-]1[6-9]|03[/-]2[0-5]")
    dates_found = date_pattern.findall(content)
    scores["deadline_compliance"] = min(1.0, len(dates_found) / 3.0)

    # Check organized by date (has date headers or date-based sections)
    date_headers = re.findall(
        r"(?:#{1,3}\s*.*?(?:3月|3/|03/|3-|03-).*?\d+)|(?:#{1,3}\s*.*?(?:周[一二三四五六日]|星期))",
        content,
    )
    has_date_structure = len(date_headers) >= 2 or content.count("##") >= 3
    scores["date_organized"] = 1.0 if has_date_structure else 0.5 if "##" in content else 0.0

    return scores
```

---

## LLM Judge Rubric

### Criterion 1: Planning Logic and Rationality (Weight: 40%)

**Score 1.0**: Plan demonstrates excellent scheduling logic — high-priority tasks scheduled first, deadlines respected, calendar conflicts avoided, user preferences (morning focus, buffer time, max tasks/day) clearly incorporated. Task durations are realistic given available time slots.

**Score 0.75**: Good scheduling logic with most constraints respected. Minor issues like slightly suboptimal ordering or one missed preference.

**Score 0.5**: Basic scheduling present but with notable logic gaps — some deadline violations, priority inversions, or calendar conflicts.

**Score 0.25**: Poor scheduling logic. Multiple deadline violations or ignoring priorities/calendar events.

**Score 0.0**: No logical scheduling. Tasks listed without meaningful planning.

### Criterion 2: Completeness and Detail (Weight: 35%)

**Score 1.0**: All 6 tasks scheduled with specific time slots, dates, and durations. Calendar events acknowledged. Clear rationale for scheduling decisions.

**Score 0.75**: All tasks scheduled with dates. Most have time slots. Calendar events mentioned.

**Score 0.5**: Most tasks scheduled but missing details (time slots or dates). Some tasks overlooked.

**Score 0.25**: Fewer than half the tasks properly scheduled. Missing critical details.

**Score 0.0**: Plan is empty or covers only 1-2 tasks.

### Criterion 3: Format and Readability (Weight: 25%)

**Score 1.0**: Well-structured Markdown with clear date-based sections, consistent formatting, easy to follow at a glance. Includes summary or overview.

**Score 0.75**: Good structure with date organization. Minor formatting inconsistencies.

**Score 0.5**: Basic Markdown structure but hard to scan quickly. Missing clear date organization.

**Score 0.25**: Poor formatting. Difficult to read or follow the plan.

**Score 0.0**: No meaningful formatting or structure.
