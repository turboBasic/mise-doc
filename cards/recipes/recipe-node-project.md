---
id: recipe-node-project
title: "Recipe: Node.js Project Setup"
category: recipes
tags: [recipe, node, npm, javascript]
source: https://github.com/jdx/mise/blob/main/docs/mise-cookbook/nodejs.md
related: [backend-core, backend-npm, env-variables, task-toml-tasks]
---

## Summary

Complete mise configuration for a Node.js project with npm/pnpm/yarn tool management and task automation.

## Examples

### Basic Node.js project

```toml
# mise.toml
[tools]
node = "20"

[env]
_.path = ["./node_modules/.bin"]

[tasks.dev]
run = "npm run dev"
description = "Start dev server"

[tasks.build]
run = "npm run build"

[tasks.test]
run = "npm test"

[tasks.lint]
run = "npx eslint ."
```

### With pnpm

```toml
# mise.toml
[tools]
node = "20"
"npm:pnpm" = "9"

[env]
_.path = ["./node_modules/.bin"]

[tasks.install]
run = "pnpm install"

[tasks.dev]
run = "pnpm dev"
depends = ["install"]
```

### With global npm tools

```toml
# mise.toml
[tools]
node = "20"
"npm:prettier" = "3"
"npm:eslint" = "9"
"npm:typescript" = "5"
```

### Monorepo with workspaces

```toml
# mise.toml (root)
[tools]
node = "20"
"npm:turbo" = "latest"

[env]
_.path = ["./node_modules/.bin"]

[tasks.build]
run = "turbo build"

[tasks.test]
run = "turbo test"

[tasks.lint]
run = "turbo lint"
```

## Details

Key patterns for Node.js with mise:
- Use `node` from core backend for the runtime
- Add `./node_modules/.bin` to PATH via `_.path` for local binaries
- Use `npm:` backend for globally-needed CLI tools (prettier, eslint, etc.)
- Define tasks in mise.toml to wrap npm scripts for consistency

## See Also

- backend-core
- backend-npm
- env-variables
- task-toml-tasks
