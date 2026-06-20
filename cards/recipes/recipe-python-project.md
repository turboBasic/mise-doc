---
id: recipe-python-project
title: "Recipe: Python Project Setup"
category: recipes
tags: [recipe, python, virtualenv]
source: https://github.com/jdx/mise/blob/main/docs/mise-cookbook/python.md
related: [backend-core, env-variables, task-toml-tasks]
---

## Summary

Complete mise configuration for a Python project with virtual environment, tool management, and task automation.

## Examples

### Basic Python project

```toml
# mise.toml
[tools]
python = "3.12"

[env]
_.source = ".venv/bin/activate"
# or without virtualenv:
# PYTHONPATH = "{{config_root}}/src"
```

### With virtualenv auto-creation

```toml
# mise.toml
[tools]
python = "3.12"
"pipx:poetry" = "latest"

[env]
VIRTUAL_ENV = "{{config_root}}/.venv"
_.path = ["{{config_root}}/.venv/bin"]

[hooks.enter]
shell = "python -m venv .venv 2>/dev/null || true"

[tasks.install]
run = "pip install -e '.[dev]'"
description = "Install project dependencies"

[tasks.test]
run = "pytest"
depends = ["install"]

[tasks.lint]
run = "ruff check ."

[tasks.fmt]
run = "ruff format ."
```

### With uv (modern Python packaging)

```toml
# mise.toml
[tools]
python = "3.12"
"pipx:uv" = "latest"

[tasks.sync]
run = "uv sync"
description = "Sync dependencies"

[tasks.test]
run = "uv run pytest"

[tasks.lint]
run = "uv run ruff check ."
```

## Details

Key patterns for Python with mise:
- Use `python` from the core backend for the interpreter
- Use `pipx:` backend for Python CLI tools (ruff, black, mypy, poetry)
- Virtualenv activation via `_.source` or explicit `_.path` manipulation
- The `hooks.enter` hook can auto-create venvs on directory entry

## See Also

- backend-core
- env-variables
- hooks-overview
- task-toml-tasks
