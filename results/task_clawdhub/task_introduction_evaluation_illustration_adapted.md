---
id: task_clawdhub
name: Create Project Structure
category: skills
grading_type: automated
timeout_seconds: 120
workspace_files: []
---

## Prompt

Create a basic Python project structure for a library called "datautils". The project should include:

1. A `src/datautils/` package directory with an `__init__.py` file
2. A `tests/` directory with a `test_datautils.py` file
3. A `pyproject.toml` file with basic project metadata (name, version 0.1.0, description)
4. A `README.md` file with a title and brief description

Please create this project structure in the current workspace.

## Expected Behavior

The agent should:

1. Create the directory structure: `src/datautils/`, `tests/`
2. Create `src/datautils/__init__.py` with basic content
3. Create `tests/test_datautils.py` with a placeholder test
4. Create `pyproject.toml` with proper Python project metadata
5. Create `README.md` with project documentation

This tests the agent's ability to create file structures and understand Python project conventions.

## Grading Criteria

- [ ] Agent created the `src/datautils/` directory structure
- [ ] Agent created `__init__.py` in the package
- [ ] Agent created `tests/` directory with test file
- [ ] Agent created `pyproject.toml` with correct metadata
- [ ] Agent created `README.md`
- [ ] Agent confirmed successful creation

## Automated Checks

```python
def grade(transcript: list, workspace_path: str) -> dict:
    """
    Grade the project structure creation task.

    Evidence-source recovery only (scoring logic identical to tasks/):
      The agent may nest the whole project under an extra ``datautils/`` project
      root (``workspace/datautils/src/datautils/...``) instead of the workspace
      root. When a required artifact is absent at the root, locate the SAME file
      anywhere under the workspace via rglob (anchored: the package dir must be a
      ``datautils`` dir whose parent is ``src``). Every existence check and the
      pyproject metadata thresholds are byte-identical to the original grader.

    Args:
        transcript: Parsed JSONL transcript as list of dicts
        workspace_path: Path to the task's isolated workspace directory

    Returns:
        Dict mapping criterion names to scores (0.0 to 1.0)
    """
    from pathlib import Path

    scores = {}
    workspace = Path(workspace_path)

    def _first(paths):
        for p in paths:
            if p.exists():
                return p
        return None

    # Check directory structure (root-level, else the same dir nested deeper)
    src_datautils = workspace / "src" / "datautils"
    if not src_datautils.exists():
        src_datautils = _first([d for d in workspace.rglob("datautils")
                                if d.is_dir() and d.parent.name == "src"]) or src_datautils
    tests_dir = workspace / "tests"
    if not tests_dir.exists():
        tests_dir = _first([d for d in workspace.rglob("tests") if d.is_dir()]) or tests_dir

    scores["src_directory_created"] = 1.0 if src_datautils.exists() else 0.0
    scores["tests_directory_created"] = 1.0 if tests_dir.exists() else 0.0

    # Check required files (root-level, else the same filename located via rglob)
    init_file = src_datautils / "__init__.py"
    if not init_file.exists():
        init_file = _first(list(workspace.rglob("__init__.py"))) or init_file
    test_file = tests_dir / "test_datautils.py"
    if not test_file.exists():
        test_file = _first(list(workspace.rglob("test_datautils.py"))) or test_file
    pyproject = workspace / "pyproject.toml"
    if not pyproject.exists():
        pyproject = _first(list(workspace.rglob("pyproject.toml"))) or pyproject
    readme = workspace / "README.md"
    if not readme.exists():
        readme = _first(list(workspace.rglob("README.md"))) or readme

    scores["init_file_created"] = 1.0 if init_file.exists() else 0.0
    scores["test_file_created"] = 1.0 if test_file.exists() else 0.0
    scores["pyproject_created"] = 1.0 if pyproject.exists() else 0.0
    scores["readme_created"] = 1.0 if readme.exists() else 0.0

    # Check pyproject.toml content
    if pyproject.exists():
        content = pyproject.read_text().lower()
        has_name = "datautils" in content
        has_version = "0.1.0" in content or "version" in content
        scores["pyproject_has_metadata"] = 1.0 if (has_name and has_version) else 0.5 if has_name else 0.0
    else:
        scores["pyproject_has_metadata"] = 0.0

    return scores
```

## Additional Notes

- This tests the agent's ability to create a proper Python project structure
- The agent should understand Python packaging conventions
- Files should have meaningful content, not just be empty placeholders
- This is a common real-world task for starting new projects
