---
id: task-toml-tasks
title: "TOML Tasks"
category: tasks
tags: [task, config, toml]
source: https://github.com/jdx/mise/blob/main/docs/tasks/toml-tasks.md
related: [task-file-tasks, task-running, cmd-run, config-mise-toml]
---

## Summary

Tasks defined directly in `mise.toml` using the `[tasks]` section. Simple tasks can be a single string; complex tasks use a table with `run`, `depends`, `description`, and other fields.

## Syntax / Usage

```toml
# Simple task (single command)
[tasks]
hello = "echo hello"

# Table form with options
[tasks.build]
run = "cargo build --release"
description = "Build for production"
depends = ["fmt", "lint"]
sources = ["src/**/*.rs"]
outputs = ["target/release/myapp"]

# Multi-line commands
[tasks.deploy]
run = """
cargo build --release
scp target/release/myapp server:/app/
"""
```

## Details

### Task fields

| Field         | Type            | Description                                       |
| ------------- | --------------- | ------------------------------------------------- |
| `run`         | string/array    | Command(s) to execute                             |
| `description` | string          | Shown in `mise tasks ls`                          |
| `depends`     | array           | Tasks that must run first                         |
| `sources`     | array of globs  | Input files (for last-modified checking)          |
| `outputs`     | array of globs  | Output files (for last-modified checking)         |
| `dir`         | string          | Working directory for the task                    |
| `env`         | table           | Extra env vars for this task                      |
| `tools`       | table           | Tools required specifically for this task         |
| `hide`        | bool            | Hide from `mise tasks ls`                         |
| `raw`         | bool            | Connect stdin/stdout directly                     |
| `shell`       | string          | Shell to use (default: `sh -c`)                   |

### Dependencies and parallelism

Dependencies run in parallel by default. If task B depends on [A, C], both A and C run concurrently before B starts.

### Last-modified checking

When both `sources` and `outputs` are specified, mise skips the task if all outputs are newer than all sources (like Make).

## Examples

```toml
[tasks.fmt]
run = "cargo fmt"

[tasks.lint]
run = "cargo clippy"
depends = ["fmt"]

[tasks.test]
run = "cargo test"
depends = ["build"]
sources = ["src/**/*.rs", "tests/**/*.rs"]
outputs = ["target/debug/deps/*"]

[tasks.build]
run = "cargo build"
sources = ["src/**/*.rs", "Cargo.toml"]
outputs = ["target/debug/myapp"]
```

## See Also

- task-file-tasks
- task-running
- cmd-run
