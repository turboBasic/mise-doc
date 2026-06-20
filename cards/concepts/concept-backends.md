---
id: concept-backends
title: "Backend Architecture"
category: concepts
tags: [backend, tool, concept]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/backend_architecture.md
related: [backend-aqua, backend-github, backend-core, backend-npm, backend-cargo]
---

## Summary

Backends are the package ecosystems that mise pulls tools from. Each backend knows how to discover versions, download, and install tools from its ecosystem. The `core` backend handles languages natively; others delegate to external registries.

## Details

### Backend tiers

| Backend   | Source                  | Requires runtime on PATH | Notes                            |
| --------- | ----------------------- | ------------------------ | -------------------------------- |
| `core`    | Built into mise         | No                       | Languages (node, python, go, etc.) |
| `aqua`    | aqua-registry           | No                       | Preferred for CLI tools          |
| `github`  | GitHub Releases         | No                       | Direct binary downloads          |
| `gitlab`  | GitLab Releases         | No                       | Same as github but for GitLab    |
| `http`    | Any URL                 | No                       | Direct HTTP download             |
| `npm`     | npmjs.com               | Yes (node)               | Node packages                    |
| `pipx`    | PyPI                    | Yes (python)             | Python packages                  |
| `cargo`   | crates.io               | Yes (rust)               | Rust crates (compiles from source)|
| `go`      | Go modules              | Yes (go)                 | Go modules (compiles from source)|
| `gem`     | RubyGems                | Yes (ruby)               | Ruby gems                        |
| `conda`   | anaconda.org            | No                       | Uses rattler, no conda needed    |
| `dotnet`  | NuGet                   | Yes (dotnet)             | .NET tools                       |

### Backend syntax

Tools are specified as `backend:identifier@version`:

```
core:node@20        # built-in node
aqua:cli/cli@2.40   # GitHub CLI via aqua
github:BurntSushi/ripgrep@14
npm:prettier@3
pipx:black@24.1
cargo:ripgrep@14
```

### Registry shortcuts

For tools in the mise registry, you can omit the backend prefix:
```
node@20           # resolves to core:node
```

The registry maps short names to their canonical `backend:identifier` form.

## Examples

```sh
# Use tools from different backends
mise use node@20                    # core backend (registry shortcut)
mise use aqua:cli/cli               # aqua backend
mise use github:BurntSushi/ripgrep  # github releases
mise use npm:prettier               # npm
mise use pipx:black                 # pipx

# Check which backend a tool uses
mise tool node
```

## See Also

- backend-aqua
- backend-github
- backend-core
- backend-npm
- backend-cargo
