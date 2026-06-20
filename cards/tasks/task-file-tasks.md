---
id: task-file-tasks
title: "File Tasks"
category: tasks
tags: [task, script, file]
source: https://github.com/jdx/mise/blob/main/docs/tasks/file-tasks.md
related: [task-toml-tasks, task-running, cmd-run]
---

## Summary

File tasks are standalone executable scripts in a task directory (default: `mise-tasks/` or `.mise/tasks/`). They offer full syntax highlighting, linting, and can be written in any language.

## Syntax / Usage

Create an executable file in the task directory:

```sh
# mise-tasks/build
#!/usr/bin/env bash
#MISE description="Build the project"
#MISE depends=["fmt", "lint"]
#MISE sources=["src/**/*.rs"]
#MISE outputs=["target/release/myapp"]

set -euo pipefail
cargo build --release
```

## Details

### Task directory locations

mise looks for file tasks in (configurable via `task_config.dir`):
- `mise-tasks/`
- `.mise/tasks/`

### Metadata comments

File tasks use `#MISE` comments for metadata:

```sh
#MISE description="what this task does"
#MISE depends=["other-task"]
#MISE sources=["src/**"]
#MISE outputs=["dist/**"]
#MISE dir="{{config_root}}"
#MISE env={NODE_ENV = "production"}
#MISE hide=true
#MISE raw=true
```

### Naming and namespacing

- Filename becomes the task name: `mise-tasks/build` → `mise run build`
- Subdirectories create namespaces: `mise-tasks/db/migrate` → `mise run db:migrate`
- Files must be executable (`chmod +x`)

### Any language

File tasks can be written in any language with a shebang:

```python
#!/usr/bin/env python3
#MISE description="Process data"
import json
# ...
```

## Examples

```sh
# mise-tasks/test
#!/usr/bin/env bash
#MISE description="Run test suite"
#MISE depends=["build"]
set -euo pipefail
cargo test "$@"
```

```sh
# mise-tasks/db/migrate
#!/usr/bin/env bash
#MISE description="Run database migrations"
set -euo pipefail
diesel migration run
```

## Caveats / Common Mistakes

- Files must be executable (`chmod +x`) — mise won't run non-executable files.
- The `#MISE` comments must be at the top of the file (after shebang).
- Subdirectory separators become colons in task names: `db/migrate` → `db:migrate`.

## See Also

- task-toml-tasks
- task-running
- cmd-run
