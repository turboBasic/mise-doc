---
id: backend-core
title: "Core Backend"
category: backends
tags: [backend, tool, core, language]
source: https://github.com/jdx/mise/blob/main/docs/core-tools.md
related: [concept-backends, backend-aqua, cmd-use]
---

## Summary

The core backend handles programming language runtimes natively within mise. These are built-in — no plugins or external registries needed. Core tools include node, python, go, ruby, java, rust, bun, deno, erlang, elixir, and more.

## Syntax / Usage

```sh
mise use node@20
mise use python@3.12
mise use go@1.22
```

## Details

Core tools are compiled into mise itself and provide first-class support for language runtimes. They typically download pre-built binaries from official sources (e.g., nodejs.org, python.org via python-build).

### Available core tools

| Tool    | Source                    | Notes                      |
| ------- | ------------------------- | -------------------------- |
| node    | nodejs.org                | Includes npm               |
| python  | python-build              | Can compile from source    |
| go      | go.dev                    | Official binaries          |
| ruby    | ruby-build                | Can compile from source    |
| java    | Adoptium, etc.            | Multiple distributions     |
| rust    | rustup                    | Via rustup integration     |
| bun     | bun.sh                    | Direct downloads           |
| deno    | deno.land                 | Direct downloads           |
| erlang  | erlang.org                | Compiles from source       |
| elixir  | elixir-lang.org           | Requires erlang            |
| zig     | ziglang.org               | Direct downloads           |
| swift   | swift.org                 | Direct downloads           |
| dotnet  | dotnet.microsoft.com      | Direct downloads           |

### Registry shortcuts

Core tools are in the mise registry, so you can use them without the `core:` prefix:
```sh
mise use node@20      # equivalent to mise use core:node@20
```

## Examples

```toml
# mise.toml
[tools]
node = "20"
python = "3.12"
go = "1.22"
```

```sh
# Install multiple languages
mise use node@20 python@3.12 go@1.22

# Use latest stable
mise use node@latest
```

## See Also

- concept-backends
- backend-aqua
- backend-github
