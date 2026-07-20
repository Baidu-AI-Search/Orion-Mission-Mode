---
id: task_workflow
name: Multi-step API Workflow
category: skills
grading_type: hybrid
timeout_seconds: 300
workspace_files:
  - path: "config.json"
    content: |
      {
        "api": {
          "endpoint": "https://api.example.com/v2/data",
          "method": "GET",
          "headers": {
            "Content-Type": "application/json",
            "Accept": "application/json"
          },
          "timeout": 30
        },
        "project": {
          "name": "DataFetcher",
          "version": "1.0.0",
          "description": "Automated data fetching utility"
        }
      }
---

## Prompt

Read config.json, extract the API endpoint, create a Python script to call it, and document the process in NOTES.md.

## Expected Behavior

The agent should:

1. Read the `config.json` file from the workspace
2. Parse the JSON and extract the API endpoint URL
3. Create a Python script that:
   - Reads the config file
   - Makes an HTTP request to the endpoint
   - Handles errors appropriately
   - Prints or returns the response
4. Create a `NOTES.md` file documenting:
   - What was done
   - How the script works
   - How to use it
   - Any important details about the configuration

This tests multi-step coordination, file reading, code generation, and documentation skills.

## Grading Criteria

### Automated Criteria (50%)

- [ ] File `config.json` successfully read
- [ ] Python script file created (any .py name)
- [ ] Script contains valid Python syntax
- [ ] Script reads/parses JSON
- [ ] Script contains HTTP request code
- [ ] File `NOTES.md` created

### LLM Judge Criteria (50%)

- [ ] Script quality and completeness
- [ ] Documentation clarity and usefulness
- [ ] Process explanation quality
- [ ] Overall task completion

## Automated Checks

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Adapted auto-check for task_workflow (automated 50%).

Bugs / distortions (self-review):
 1. The deliverables call_api.py + NOTES.md were written to the agent's sandbox
    HOME (/home/user/<id>/...) and NOT exported to workspace/task_workflow/, so
    the workspace is empty -> original grade returns script_created=0,
    notes_created=0, etc.
 2. read_config uses the original transcript shape (toolCall name in
    {read,read_file,readFile}); harness passes grade([], ws) (empty) -> 0.

Fix (max-common-divisor + fair):
  - Recover the python script (call_api.py / any *.py) and NOTES.md from the
    preserved write tool-calls when absent from the workspace. The recovered
    text is the agent's OWN output, not fabricated.
  - read_config: scan the real trajectory for a read of config.json.
  All syntax / json-parse / http-request checks run unchanged on the recovered
  script content.
"""
from pathlib import Path
import re
import ast
import os
import sys

sys.path.insert(0, os.path.join(os.path.dirname(os.path.abspath(globals().get('__file__', 'tasks_adapted/md/x.md'))), '..', 'scripts'))
import _transcript_util as TU  # noqa: E402

TASK_ID = "task_workflow"


def grade(transcript: list, workspace_path: str) -> dict:
    scores = {}
    workspace = Path(workspace_path)

    # ---- read_config: original transcript OR recovered trajectory ----
    read_config = False
    for event in transcript or []:
        if not isinstance(event, dict) or event.get("type") != "message":
            continue
        msg = event.get("message", {})
        if msg.get("role") == "assistant":
            for item in msg.get("content", []) or []:
                if item.get("type") == "toolCall" and item.get("name", "").lower() in (
                        "read_file", "readfile", "read"):
                    params = item.get("params", item.get("arguments", {}))
                    cands = [params.get("path", ""), params.get("file_path", ""),
                             params.get("file", "")] + list(params.get("files", []))
                    if any("config.json" in str(x) for x in cands if x):
                        read_config = True
    if not read_config:
        for name, args in TU.iter_tool_calls(TASK_ID):
            nl = (name or "").lower()
            if not isinstance(args, dict):
                continue
            blob = " ".join(str(v) for v in args.values()).lower()
            if "config.json" in blob and (
                    nl in ("read", "read_file", "readfile", "cat")
                    or "cat" in blob or "config.json" in str(args.get("command", "")).lower()):
                read_config = True
                break
    scores["read_config"] = 1.0 if read_config else 0.0

    # ---- locate the python script: workspace, then recovered write ----
    py_files = list(workspace.glob("*.py")) or list(workspace.rglob("*.py"))
    script_content = ""
    if py_files:
        script_content = py_files[0].read_text()
    else:
        script_content = (TU.recover_written_file(TASK_ID, "call_api.py")
                          or TU.recover_written_file(TASK_ID, "api_caller.py")
                          or TU.recover_written_file(TASK_ID, "main.py"))

    if not script_content.strip():
        scores["script_created"] = 0.0
        scores["valid_syntax"] = 0.0
        scores["parses_json"] = 0.0
        scores["has_http_request"] = 0.0
    else:
        scores["script_created"] = 1.0
        try:
            ast.parse(script_content)
            scores["valid_syntax"] = 1.0
        except SyntaxError:
            scores["valid_syntax"] = 0.0
            scores["parses_json"] = 0.0
            scores["has_http_request"] = 0.0
            scores["notes_created"] = 0.0
            return scores

        json_patterns = [r'import\s+json', r'from\s+json\s+import',
                         r'json\.load', r'json\.loads']
        scores["parses_json"] = 1.0 if any(
            re.search(p, script_content) for p in json_patterns) else 0.0

        http_patterns = [r'import\s+requests', r'from\s+requests\s+import',
                         r'import\s+urllib', r'from\s+urllib', r'requests\.get',
                         r'requests\.post', r'urllib\.request', r'urlopen']
        scores["has_http_request"] = 1.0 if any(
            re.search(p, script_content, re.IGNORECASE) for p in http_patterns) else 0.0

    # ---- NOTES.md: workspace OR recovered write ----
    notes_file = workspace / "NOTES.md"
    if notes_file.exists() or list(workspace.rglob("NOTES.md")):
        scores["notes_created"] = 1.0
    elif TU.recover_written_file(TASK_ID, "NOTES.md"):
        scores["notes_created"] = 1.0
    else:
        scores["notes_created"] = 0.0

    return scores

```

## LLM Judge Rubric

### Criterion 1: Script Quality and Functionality (Weight: 30%)

**Score 1.0**: Python script is well-structured, reads config.json correctly, extracts the endpoint, makes HTTP request with proper error handling, and includes helpful comments. Code follows best practices.

**Score 0.75**: Script is functional with minor issues. Reads config and makes request but may lack comprehensive error handling or have minor code quality issues.

**Score 0.5**: Script has basic functionality but with notable issues. May have incomplete error handling, poor structure, or missing important features.

**Score 0.25**: Script is poorly written or barely functional. Major issues with logic, structure, or error handling.

**Score 0.0**: Script is non-functional, missing, or completely fails to meet requirements.

### Criterion 2: Documentation Quality (Weight: 30%)

**Score 1.0**: NOTES.md is comprehensive, well-organized, and clearly explains what was done, how the script works, and how to use it. Includes relevant details about the config structure and any assumptions made.

**Score 0.75**: Good documentation covering most important points. Minor gaps in explanation or organization.

**Score 0.5**: Basic documentation present but lacking detail or clarity. Missing important information about usage or implementation.

**Score 0.25**: Poor documentation. Minimal information or unclear explanations.

**Score 0.0**: Documentation is missing or completely inadequate.

### Criterion 3: Process Understanding (Weight: 25%)

**Score 1.0**: Clear evidence that agent understood the multi-step workflow. Correctly extracted endpoint from config, created appropriate script, and documented the process logically. Shows good decision-making.

**Score 0.75**: Good understanding with minor issues. Process mostly correct with small gaps in logic or execution.

**Score 0.5**: Partial understanding. Agent completed some steps but missed connections or made questionable decisions.

**Score 0.25**: Poor understanding. Agent struggled with the workflow or made significant errors in process.

**Score 0.0**: No evidence of understanding the task requirements.

### Criterion 4: Overall Completeness (Weight: 15%)

**Score 1.0**: All requirements met. Config read, endpoint extracted, functional script created, comprehensive documentation provided. Task fully complete.

**Score 0.75**: Most requirements met with minor omissions. Task essentially complete.

**Score 0.5**: Some requirements met but significant gaps. Task partially complete.

**Score 0.25**: Few requirements met. Task barely attempted.

**Score 0.0**: Task not completed or completely failed.
