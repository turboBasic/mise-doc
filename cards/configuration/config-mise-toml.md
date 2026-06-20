---
id: config-mise-toml
title: "mise.toml Configuration File"
category: configuration
tags: [config, toml, tool, env]
source: https://github.com/jdx/mise/blob/main/docs/configuration.md
related: [concept-config-hierarchy, config-settings, config-environments]
---

## Summary

`mise.toml` is the primary configuration file for mise. It declares tools, environment variables, tasks, hooks, and settings for a project.

## Syntax / Usage

```toml
[tools]
node = "20"
python = "3.12"
"npm:prettier" = "latest"

[env]
NODE_ENV = "development"

[tasks.build]
run = "cargo build"
description = "Build the project"

[hooks.enter]
shell = "echo 'Entered project'"
```

## Details

### Sections

| Section        | Purpose                                        |
| -------------- | ---------------------------------------------- |
| `[tools]`      | Declare tool versions                          |
| `[env]`        | Set environment variables                      |
| `[tasks]`      | Define runnable tasks                          |
| `[hooks]`      | Lifecycle hooks (enter, leave, cd, etc.)       |
| `[settings]`   | Override mise settings for this project         |
| `[plugins]`    | Legacy plugin configuration                    |
| `[shell_alias]`| Define shell aliases                           |

### Tool version formats

```toml
[tools]
node = "20"           # fuzzy — resolves to latest 20.x.x
node = "20.11.0"      # exact pin
node = "latest"       # always latest
node = ["20", "18"]   # multiple versions installed
node = { version = "20", options = { ... } }  # with options
```

### Env var features

```toml
[env]
# Simple value
API_URL = "http://localhost:3000"

# From a file
_.file = ".env"

# From a script (requires trust)
_.source = "./load-secrets.sh"

# Templated
DB_URL = "postgres://{{env.USER}}@localhost/mydb"

# Path manipulation
_.path = ["./node_modules/.bin", "./bin"]
```

## Examples

```toml
# Minimal project config
[tools]
node = "20"
python = "3.12"

[env]
NODE_ENV = "development"

[tasks.dev]
run = "npm run dev"
description = "Start development server"

[tasks.test]
run = "pytest"
depends = ["build"]
```

## See Also

- concept-config-hierarchy
- config-settings
- config-environments
- task-toml-tasks
