---
id: concept-shims
title: "Shims"
category: concepts
tags: [shim, activation, concept]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/shims.md
related: [concept-activation, cmd-activate, cmd-reshim]
---

## Summary

Shims are small executables (symlinks to the mise binary) placed in `~/.local/share/mise/shims/` that intercept commands and load the appropriate tool version based on the current directory's config.

## Details

When you install a tool (e.g., `node`), mise creates shims for every binary that tool provides. Each shim is a symlink to mise itself. When invoked, mise detects it was called as a shim, reads the relevant config, installs the tool if needed, and executes the real binary.

Default shim directory: `~/.local/share/mise/shims` (Windows: `%LOCALAPPDATA%\mise\shims`)

### Shims vs PATH activation

| Feature                         | PATH activation | Shims |
| ------------------------------- | --------------- | ----- |
| Shell aliases                   | Yes             | No    |
| Hooks                           | Yes             | No    |
| `chpwd` hook                    | Yes             | No    |
| IDE compatibility               | Limited         | Yes   |
| CI/CD compatibility             | Limited         | Yes   |
| Per-invocation overhead         | None            | Small |
| Per-prompt overhead             | Small           | None  |

### When to use shims

- IDEs that need tools on PATH without a shell hook
- CI/CD pipelines
- Non-interactive scripts
- `cron` jobs

## Examples

```sh
# Enable shims via activate
eval "$(mise activate --shims)"

# Or manually add to PATH
export PATH="$HOME/.local/share/mise/shims:$PATH"

# Regenerate shims after manual changes
mise reshim

# Check what a shim resolves to
mise which node
```

## Caveats / Common Mistakes

- Shims don't support shell aliases defined in `mise.toml`
- If both `mise activate` and shims are on PATH, activation takes precedence
- After installing tools outside mise (e.g., `npm install -g`), run `mise reshim` to create shims for new binaries

## See Also

- concept-activation
- cmd-activate
- cmd-reshim
- cmd-which
