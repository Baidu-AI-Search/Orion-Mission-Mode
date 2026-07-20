---
id: task_files
name: File Structure Creation
category: skills
grading_type: automated
timeout_seconds: 120
workspace_files: []
---

## Prompt

Create a project structure with: src/ directory, src/main.py with hello world, README.md with project title, and .gitignore ignoring **pycache**.

## Expected Behavior

The agent should:

1. Create a directory named `src/`
2. Create a file `src/main.py` with a hello world program
3. Create a file `README.md` with a project title
4. Create a file `.gitignore` that includes `__pycache__`
5. Ensure all files have appropriate content

This tests the agent's ability to perform basic file operations and create a standard project structure.

## Grading Criteria

- [ ] Directory `src/` created
- [ ] File `src/main.py` created
- [ ] `src/main.py` contains valid Python hello world code
- [ ] File `README.md` created
- [ ] `README.md` contains a project title/heading
- [ ] File `.gitignore` created
- [ ] `.gitignore` contains `__pycache__` entry

## Automated Checks

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Adapted auto-check for task_files.

Bugs:
 1. (download distortion, user-confirmed tolerate) wget flattened src/main.py
    -> main.py and stripped .gitignore -> gitignore. Original grader looks
    only at workspace/src/main.py and workspace/.gitignore, scoring 0 for a
    correct result that merely lost its directory/dot during download.
 2. (人工拆解) hello-world regex only matched a BARE top-level
    print("Hello..."). The real file wraps it in `def main(): print(...)`,
    which is still a valid hello-world, but scored 0.5 (syntax-only).

Fix:
 - Resolve each expected file by trying the canonical path first, then the
   distorted/flattened fallbacks (main.py, gitignore, etc.), and a recursive
   search for the basename.
 - Detect hello-world by any print(...) containing 'hello' anywhere in the
   source (covers wrapped/`main()` style), keeping AST validity check.
"""
from pathlib import Path
import re
import ast


def _find(workspace, *candidates, basename=None):
    """Return first existing path among candidates; else recursive basename search."""
    for c in candidates:
        p = workspace / c
        if p.exists():
            return p
    if basename:
        hits = list(workspace.rglob(basename))
        if hits:
            return hits[0]
    return None


def grade(transcript: list, workspace_path: str) -> dict:
    workspace = Path(workspace_path)
    scores = {}

    # src/ directory (tolerate flattening: if files landed flat, treat as ok-ish)
    src_dir = workspace / "src"
    main_py = _find(workspace, "src/main.py", "main.py", basename="main.py")
    if src_dir.exists() and src_dir.is_dir():
        scores["src_directory"] = 1.0
    elif main_py is not None:
        # directory lost in download but main.py exists -> partial credit
        scores["src_directory"] = 0.5
    else:
        scores["src_directory"] = 0.0

    if main_py is not None:
        scores["main_py_created"] = 1.0
        content = main_py.read_text()
        try:
            ast.parse(content)
            valid_syntax = True
        except SyntaxError:
            valid_syntax = False
        # FIX: any print(... hello ...) anywhere (wrapped or bare)
        has_hello = bool(re.search(
            r'print\s*\(\s*[^)]*["\']\s*hello', content, re.IGNORECASE))
        if not has_hello:
            # also accept a hello string anywhere combined with a print call
            has_hello = bool(re.search(r'print\s*\(', content)) and \
                'hello' in content.lower()
        if valid_syntax and has_hello:
            scores["main_py_valid"] = 1.0
        elif valid_syntax:
            scores["main_py_valid"] = 0.5
        else:
            scores["main_py_valid"] = 0.0
    else:
        scores["main_py_created"] = 0.0
        scores["main_py_valid"] = 0.0

    readme = _find(workspace, "README.md", basename="README.md")
    if readme is not None:
        scores["readme_created"] = 1.0
        content = readme.read_text()
        has_title = (
            re.search(r'^#\s+\w+', content, re.MULTILINE) or
            re.search(r'^[A-Z][\w\s]{3,}$', content, re.MULTILINE) or
            len(content.strip()) > 5
        )
        scores["readme_has_title"] = 1.0 if has_title else 0.5
    else:
        scores["readme_created"] = 0.0
        scores["readme_has_title"] = 0.0

    # FIX: tolerate distorted ".gitignore" -> "gitignore"
    gitignore = _find(workspace, ".gitignore", "gitignore", basename=".gitignore")
    if gitignore is None:
        gitignore = _find(workspace, basename="gitignore")
    if gitignore is not None:
        scores["gitignore_created"] = 1.0
        content = gitignore.read_text()
        scores["gitignore_has_pycache"] = 1.0 if '__pycache__' in content else 0.0
    else:
        scores["gitignore_created"] = 0.0
        scores["gitignore_has_pycache"] = 0.0

    return scores

```
