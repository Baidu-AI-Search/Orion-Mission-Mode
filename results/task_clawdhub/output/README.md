# datautils

A small utility library for common data processing tasks.

## Overview

`datautils` is a lightweight Python library that provides helpful utilities for
everyday data manipulation work. The package currently exposes a simple version
identifier and a basic test scaffold, intended as a starting point that you can
extend with your own helper functions and modules.

## Project Layout

```
datautils/
├── pyproject.toml          # Project metadata and build configuration
├── README.md               # This file
├── src/
│   └── datautils/
│       └── __init__.py     # Package source code
└── tests/
    └── test_datautils.py   # Unit tests
```

## Installation

Install the package in editable mode for local development:

```bash
pip install -e .
```

## Running Tests

Tests use `pytest`. With the source path configured in `pyproject.toml`, you can
run them directly:

```bash
pip install pytest
pytest
```

## Development

- Source code lives under `src/datautils/`.
- Add new modules inside `src/datautils/` and re-export public names from
  `__init__.py` as needed.
- Add corresponding tests under `tests/` following the `test_*.py` naming
  convention.

## License

MIT
