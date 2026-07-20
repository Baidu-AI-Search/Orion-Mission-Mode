---
id: task_memory
name: Memory Retrieval from Context
category: memory
grading_type: automated
timeout_seconds: 120
workspace_files:
  - path: "notes.md"
    content: |
      # Project Phoenix - Development Notes

      ## Timeline

      - **Alpha Release**: March 15, 2024
      - **Beta Release**: June 1, 2024
      - **Production Launch**: September 30, 2024

      ## Team

      - Lead Developer: Sarah Chen
      - Backend: Marcus Rodriguez, Aisha Patel
      - Frontend: James Kim, Elena Volkov
      - QA: David Thompson

      ## Key Features

      1. Real-time collaboration
      2. Advanced analytics dashboard
      3. Mobile app integration
      4. API v2 with GraphQL support

      ## Current Status

      We're currently in the alpha testing phase with 50 internal users. Feedback has been positive, particularly regarding the new UI. The beta release is scheduled for June 1, 2024, and we're on track to meet this deadline.

      ## Technical Stack

      - Frontend: React 18, TypeScript
      - Backend: Node.js, Express, PostgreSQL
      - Infrastructure: AWS (ECS, RDS, S3)
      - CI/CD: GitHub Actions

      ## Blockers

      - Need to finalize API documentation before beta
      - Mobile app needs performance optimization
      - Waiting on security audit completion
---

## Prompt

Read the project notes in notes.md and answer: What is the deadline for the beta release? Save your answer to answer.txt.

## Expected Behavior

The agent should:

1. Read the file `notes.md` from the workspace
2. Parse the content to find information about the beta release deadline
3. Extract the correct date: June 1, 2024
4. Create a file named `answer.txt` with the answer
5. Format the answer clearly

This tests the agent's ability to retrieve specific information from context files.

## Grading Criteria

- [ ] File `answer.txt` created
- [ ] Answer contains the date "June 1, 2024" or equivalent
- [ ] Answer is clear and directly addresses the question
- [ ] Agent read the notes.md file (visible in transcript)
- [ ] No hallucinated or incorrect information

## Automated Checks

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Adapted auto-check for task_memory.

Bugs / distortions (self-review):
 1. answer.txt was written to the agent's sandbox HOME
    (/home/user/<id>/answer.txt) and NOT exported to workspace/task_memory/, so
    the workspace is empty -> original grade returns all-zero (0.0).
 2. read_notes uses the original transcript shape (toolCall + name in
    {read,read_file,readFile}); the harness passes grade([], ws) (empty), so
    read_notes scores 0 even though the agent read notes.md.

Fix (max-common-divisor + fair):
  - Recover answer.txt content from the preserved write tool-call (and final
    answer text as a secondary source). The recovered content is the agent's
    OWN output, not fabricated.
  - read_notes: scan the real trajectory for a read of notes.md (or final-text
    evidence that notes were consulted).
  All correctness checks (correct_date / clear_answer / no_hallucination) run
  unchanged against the recovered content.
"""
from pathlib import Path
import re
import os
import sys

sys.path.insert(0, os.path.join(os.path.dirname(os.path.abspath(globals().get('__file__', 'tasks_adapted/md/x.md'))), '..', 'scripts'))
import _transcript_util as TU  # noqa: E402

TASK_ID = "task_memory"


def grade(transcript: list, workspace_path: str) -> dict:
    scores = {}
    workspace = Path(workspace_path)

    # ---- locate answer.txt: workspace, then recovered sandbox-home write ----
    answer_file = workspace / "answer.txt"
    content = ""
    if answer_file.exists():
        content = answer_file.read_text()
    else:
        hits = list(workspace.rglob("answer.txt"))
        if hits:
            content = hits[0].read_text()
        else:
            content = TU.recover_written_file(TASK_ID, "answer.txt")
            if not content:
                # last resort: the final answer text itself contains the answer
                content = TU.get_final_text(TASK_ID) or ""

    if not content.strip():
        return {
            "file_created": 0.0,
            "correct_date": 0.0,
            "clear_answer": 0.0,
            "read_notes": 0.0,
            "no_hallucination": 0.0,
        }

    scores["file_created"] = 1.0
    cl = content.lower()

    # correct date (June 1, 2024)
    date_patterns = [
        r'june\s+1,?\s+2024', r'june\s+1st,?\s+2024', r'6/1/2024',
        r'6-1-2024', r'2024-06-01', r'01\s+june\s+2024', r'1\s+june\s+2024',
    ]
    if any(re.search(p, cl, re.IGNORECASE) for p in date_patterns):
        scores["correct_date"] = 1.0
    elif (re.search(r'\d{1,2}[/-]\d{1,2}[/-]\d{2,4}', cl)
          or re.search(r'(january|february|march|april|may|june|july|august|'
                       r'september|october|november|december)', cl, re.IGNORECASE)):
        scores["correct_date"] = 0.3
    else:
        scores["correct_date"] = 0.0

    # clear answer
    if len(cl.strip()) > 10 and ('beta' in cl or 'release' in cl or 'deadline' in cl):
        scores["clear_answer"] = 1.0
    elif len(cl.strip()) > 5:
        scores["clear_answer"] = 0.5
    else:
        scores["clear_answer"] = 0.0

    # read_notes: original transcript OR recovered trajectory
    read_notes = False
    for event in transcript or []:
        if not isinstance(event, dict) or event.get("type") != "message":
            continue
        msg = event.get("message", {})
        if msg.get("role") == "assistant":
            for item in msg.get("content", []) or []:
                if item.get("type") == "toolCall" and item.get("name", "") in (
                        "read", "read_file", "readFile"):
                    args = item.get("arguments", item.get("params", {}))
                    cands = [args.get("path", ""), args.get("file_path", ""),
                             args.get("file", "")] + list(args.get("files", []))
                    if any("notes.md" in str(x) for x in cands if x):
                        read_notes = True
    if not read_notes:
        for name, args in TU.iter_tool_calls(TASK_ID):
            nl = (name or "").lower()
            if nl in ("read", "read_file", "readfile", "cat") and isinstance(args, dict):
                blob = " ".join(str(v) for v in args.values()).lower()
                if "notes.md" in blob:
                    read_notes = True
                    break
            if nl in ("bash", "shell") and isinstance(args, dict):
                if "notes.md" in str(args.get("command", "")).lower():
                    read_notes = True
                    break
    scores["read_notes"] = 1.0 if read_notes else 0.0

    # no hallucination of wrong dates
    wrong = [r'march\s+15', r'september\s+30', r'3/15', r'9/30']
    scores["no_hallucination"] = 0.0 if any(
        re.search(p, cl, re.IGNORECASE) for p in wrong) else 1.0

    return scores

```
