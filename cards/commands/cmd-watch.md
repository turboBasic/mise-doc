---
id: cmd-watch
title: "mise watch"
category: commands
tags: [command, task, watch, filesystem]
source: https://github.com/jdx/mise/blob/main/docs/cli/watch.md
related: [cmd-run, task-running, task-toml-tasks]
---

## Summary

Watch for file changes and automatically re-run tasks. Uses filesystem events for efficient change detection without polling.

## Syntax / Usage

```sh
mise watch [FLAGS] [TASK] [ARGS]…
mise w [TASK] [ARGS]…
```

Alias: `mise w`

## Details

`mise watch` monitors the filesystem and re-runs the specified task whenever source files change. If the task has `sources` defined, only those paths are watched. Otherwise, the entire project directory is watched.

## Examples

```sh
# Watch and re-run build on changes
mise watch build

# Watch specific glob pattern
mise watch test -g "src/**/*.rs"

# Watch with arguments passed to the task
mise watch test -- --verbose
```

## See Also

- cmd-run
- task-running
- task-toml-tasks
