---
id: task-running
title: "Running Tasks"
category: tasks
tags: [task, execution, watch]
source: https://github.com/jdx/mise/blob/main/docs/tasks/running-tasks.md
related: [cmd-run, task-toml-tasks, task-file-tasks, cmd-watch]
---

## Summary

Tasks are run with `mise run` (or `mise r`). Dependencies execute in parallel by default, last-modified checking avoids unnecessary rebuilds, and `mise watch` can auto-run tasks on file changes.

## Syntax / Usage

```sh
mise run <TASK> [ARGS]…
mise r <TASK> [-- ARGS]…
mise watch [TASK]
```

## Details

### Parallel execution

Task dependencies run in parallel with no configuration. If task `build` depends on `[fmt, lint]`, both `fmt` and `lint` run concurrently, then `build` runs after both complete.

Control parallelism with `--jobs`:
```sh
mise run build --jobs 8
```

### Last-modified checking

When `sources` and `outputs` are specified on a task, mise checks timestamps and skips the task if outputs are newer than all sources. This works like Make — only rebuild what's changed.

### Watch mode

`mise watch` uses file system events to re-run tasks when source files change:

```sh
mise watch build          # re-run build on any change
mise watch test -g "src"  # watch only src/ directory
```

### Passing arguments

Arguments after `--` are passed to the task:
```sh
mise run test -- --verbose --filter=auth
```

### Listing tasks

```sh
mise tasks ls    # show all available tasks
mise tasks info build  # show details of a specific task
```

## Examples

```sh
# Run a single task
mise run build

# Run multiple tasks (all run, dependencies resolved)
mise run lint test build

# Pass arguments to a task
mise run test -- --coverage

# Watch for changes and re-run
mise watch test

# Dry run — show what would happen
mise run build --dry-run
```

## See Also

- cmd-run
- cmd-watch
- task-toml-tasks
- task-file-tasks
