---
id: cmd-run
title: "mise run"
category: commands
tags: [command, task, execution]
source: https://github.com/jdx/mise/blob/main/docs/cli/run.md
related: [cmd-exec, task-toml-tasks, task-file-tasks, task-running]
---

## Summary

Run a task defined in `mise.toml` or as a file task. Tasks execute with the full mise context (tools + env vars) loaded, and dependencies run in parallel by default.

## Syntax / Usage

```sh
mise run [FLAGS] [TASK] [ARGS]…
mise r [TASK] [ARGS]…
```

Alias: `mise r`

## Details

`mise run` executes tasks defined in your project's `mise.toml` or as standalone scripts in task directories. Before running, mise automatically installs all tools declared in `mise.toml`. Task dependencies are resolved and run in parallel unless sequential execution is specified.

If the task name doesn't conflict with a built-in mise command, you can also run it as `mise <task-name>` directly (e.g., `mise build` instead of `mise run build`).

Environment variables passed to tasks:
- `MISE_ORIGINAL_CWD` — directory where the task was invoked
- `MISE_CONFIG_ROOT` — directory containing the `mise.toml` that defines the task
- `MISE_PROJECT_ROOT` — root of the project defining the task
- `MISE_TASK_NAME` — name of the running task

## Examples

```sh
# Run a task
mise run build

# Short alias
mise r test

# Pass arguments to a task
mise run test -- --verbose

# Run task if name doesn't conflict with mise commands
mise build

# Run multiple tasks
mise run lint test build

# Run with increased parallelism
mise run build --jobs 8
```

## Caveats / Common Mistakes

- Tasks that share the same name as a mise subcommand must be invoked with `mise run <name>`, not `mise <name>`.
- All tools in `mise.toml` are installed before the task runs — this ensures reproducibility but may add startup time on first run.
- Task dependencies run in parallel by default with no configuration required.

## See Also

- task-toml-tasks
- task-file-tasks
- task-running
- cmd-exec
