---
id: backend-npm
title: "npm Backend"
category: backends
tags: [backend, tool, npm, node]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/backends/npm.md
related: [concept-backends, backend-pipx, cmd-use]
---

## Summary

The npm backend installs CLI tools from the npm registry. Requires Node.js to be available on PATH (mise can provide it).

## Syntax / Usage

```sh
mise use npm:package-name@version
```

## Details

Installs npm packages globally using `npm install -g`. The installed tool binds to whichever Node.js version was on PATH at install time. Mise manages shims for the installed binaries.

Common use cases: prettier, eslint, typescript, claude-code, etc.

## Examples

```sh
# Install prettier
mise use npm:prettier@3

# Install Claude Code
mise use npm:@anthropic-ai/claude-code

# In mise.toml
[tools]
"npm:prettier" = "3"
"npm:@anthropic-ai/claude-code" = "latest"
```

## Caveats / Common Mistakes

- Requires Node.js on PATH — if node is also managed by mise, declare it before npm tools in config.
- The installed tool silently binds to the node version active at install time. Changing node versions may require reinstalling npm tools.
- Prefer `aqua:` or `github:` backends for tools that ship standalone binaries — npm adds a Node.js runtime dependency.

## See Also

- concept-backends
- backend-pipx
- backend-core
