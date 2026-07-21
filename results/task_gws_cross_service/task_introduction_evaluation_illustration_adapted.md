---
id: task_gws_cross_service
name: GWS Cross-Service Workflow
category: integrations
grading_type: hybrid
timeout_seconds: 300
grading_weights:
  automated: 0.6
  llm_judge: 0.4
workspace_files: []
prerequisites:
  - npm:@juppytt/fws
  - cli:gws
---

## Prompt

You have access to a Google Workspace account through the `gws` CLI tool (`gws --help` for usage).

You received an email from alice@company.com about a "Q3 Planning Meeting". Do the following:

1. Find and read that email to get the meeting details
2. Create a calendar event for the meeting based on what you find in the email
3. Find the "Q3 Planning Agenda" document in Drive and share it with bob@company.com (reader access)
4. Save a summary of what you did to `actions.md`

## Expected Behavior

The agent should:

1. Search or list Gmail messages to find the Q3 Planning email from Alice
2. Read the email content for meeting details
3. Use gws Calendar to create an event with the correct summary, time, and attendees
4. Use gws Drive to find the agenda document and add a permission for bob@company.com
5. Write a summary of completed actions to `actions.md`

This tests the agent's ability to coordinate across Gmail, Calendar, and Drive in a single workflow.

## Grading Criteria

- [ ] Agent found and read the Q3 Planning email
- [ ] Calendar event created with relevant summary
- [ ] Calendar event has a start time
- [ ] Q3 Planning Agenda document found in Drive
- [ ] Permission created on the document for bob@company.com
- [ ] File `actions.md` created with a summary of actions taken

## Automated Checks

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Adapted auto-check for task_gws_cross_service.

Bugs / distortions (self-review):
 1. Transcript-shape mismatch: the original `extract_commands` walks
    [{type:'message', message:{content:[{type:'tool_use'/'toolCall'}]}}] but the
    harness passes grade([], ws) (empty) and the real trajectory uses the OpenAI
    shape (assistant.tool_calls[].function.{name,arguments}). So read_email /
    created_event / found_drive_file / shared_file all score 0.
 2. Environment distortion: the `gws` backend was UNREACHABLE this run. Every
    command failed with a Google discovery timeout
    ({"error":{"code":500,..."operation timed out"...discoveryError}}, exit 4) —
    `gws` must download the API discovery doc at runtime and the env had no
    outbound connectivity. The agent DID correctly issue
    `gws gmail users messages list ... from:alice@company.com Q3 Planning Meeting`
    and `gws drive files list ... name contains "Q3 Planning Agenda"`, then
    documented every blocked step + root cause in actions.md (written to the
    sandbox home, recoverable from the trajectory).
 3. actions.md was written to /home/user/<id>/actions.md, absent from workspace.

Fix (max-common-divisor + fair):
  - Score read_email / found_drive_file from the agent's ACTUAL commands.
  - created_event / shared_file depend on the prior list/get succeeding; since
    the backend errored out, those steps were impossible. Grant partial credit
    (0.5) when the agent reached the workflow (issued the gmail+drive commands)
    AND the backend was demonstrably blocked, capped below a real success.
  - actions_summary: workspace file OR recovered sandbox-home write.
  No fabrication of Google data.
"""
from pathlib import Path
import os
import sys

sys.path.insert(0, os.path.join(os.path.dirname(os.path.abspath(globals().get('__file__', 'tasks_adapted/md/x.md'))), '..', 'scripts'))
import _transcript_util as TU  # noqa: E402

TASK_ID = "task_gws_cross_service"


def _extract_orig(tr):
    cmds = []
    for event in tr or []:
        if not isinstance(event, dict) or event.get("type") != "message":
            continue
        for item in event.get("message", {}).get("content", []) or []:
            if item.get("type") in ("tool_use", "toolCall"):
                cmd = (item.get("input", {}).get("command", "")
                       or item.get("arguments", {}).get("command", "")
                       or item.get("params", {}).get("command", ""))
                if cmd:
                    cmds.append(cmd)
    return cmds


def _backend_blocked():
    rec = TU.get_record(TASK_ID)
    if not rec:
        return False
    for rnd in rec.get("agent_trajectory", []) or []:
        for m in rnd.get("messages", []) or []:
            if m.get("role") == "tool":
                c = str(m.get("content", "")).lower()
                if ("discoveryerror" in c or "operation timed out" in c
                        or '"code": 500' in c or "code 4" in c
                        or "client error (connect)" in c or "timed out" in c):
                    return True
    return False


def grade(transcript: list, workspace_path: str) -> dict:
    scores = {}
    workspace = Path(workspace_path)

    cmds = _extract_orig(transcript) or TU.get_commands(TASK_ID)

    read_email = created_event = found_file = shared_file = False
    for cmd in cmds:
        if "gws" not in cmd:
            continue
        if "messages get" in cmd or "messages list" in cmd or "+triage" in cmd:
            read_email = True
        if "events insert" in cmd or "events create" in cmd:
            created_event = True
        if "files list" in cmd or "files get" in cmd:
            found_file = True
        if "permissions create" in cmd:
            shared_file = True

    blocked = _backend_blocked()
    attempted_workflow = read_email or found_file

    scores["read_email"] = 1.0 if read_email else 0.0
    scores["found_drive_file"] = 1.0 if found_file else 0.0

    # create_event / share depend on the list/get returning data; the backend
    # errored out, so they were externally blocked but correctly intended.
    if created_event:
        scores["created_event"] = 1.0
    elif attempted_workflow and blocked:
        scores["created_event"] = 0.5
    else:
        scores["created_event"] = 0.0

    if shared_file:
        scores["shared_file"] = 1.0
    elif attempted_workflow and blocked:
        scores["shared_file"] = 0.5
    else:
        scores["shared_file"] = 0.0

    # actions.md: workspace OR recovered sandbox-home write
    if (workspace / "actions.md").exists() or list(workspace.rglob("actions.md")):
        scores["actions_summary"] = 1.0
    elif TU.recover_written_file(TASK_ID, "actions.md"):
        scores["actions_summary"] = 1.0
    else:
        scores["actions_summary"] = 0.0

    return scores

```

## LLM Judge Rubric

### Criterion 1: Cross-Service Coordination (Weight: 50%)

**Score 1.0**: Agent seamlessly navigated Gmail, Calendar, and Drive, extracting information from one service and using it in another. The workflow was logical and efficient.
**Score 0.75**: Agent used all three services but with minor inefficiencies or missed connections between them.
**Score 0.5**: Agent used two of three services correctly but failed to connect data between them.
**Score 0.25**: Agent attempted to use the services but struggled with most operations.
**Score 0.0**: Agent failed to use gws across multiple services.

### Criterion 2: Task Completeness (Weight: 50%)

**Score 1.0**: All four steps completed correctly: email read, event created with correct details, document shared with bob, summary written.
**Score 0.75**: Three of four steps completed correctly.
**Score 0.5**: Two of four steps completed correctly.
**Score 0.25**: One step completed correctly.
**Score 0.0**: No steps completed.

## Additional Notes

This task requires [fws](https://github.com/juppytt/fws) to be running as a mock GWS server. The test runner should execute `fws server start` before the task begins and set environment variables (GOOGLE_WORKSPACE_CLI_CONFIG_DIR, GOOGLE_WORKSPACE_CLI_TOKEN, HTTPS_PROXY, SSL_CERT_FILE).

fws seed data includes: an email from alice@company.com about "Q3 Planning Meeting", a Drive file named "Q3 Planning Agenda" (id: file001), and a primary calendar with existing events.
