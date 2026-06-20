---
id: concept-activation
title: "Activation Methods"
category: concepts
tags: [activation, shell, shim, concept]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/shims.md
related: [cmd-activate, concept-shims, cmd-exec]
---

## Summary

Mise offers three ways to load its context (tools + env vars) into your shell: PATH activation, shims, or explicit `mise exec`/`mise run`. The choice affects performance, compatibility, and feature support.

## Details

### PATH activation (`mise activate`)

A prompt hook runs on every prompt display, updating PATH and env vars based on the current directory's config. Recommended for interactive shells.

Pros:
- Full feature support (shell aliases, hooks, env vars)
- Immediate response to directory changes
- No shim overhead per command invocation

Cons:
- Small overhead on every prompt (typically < 5ms)
- Only works in interactive shells
- Doesn't work in profile files (`.profile`, `.bash_profile`)

### Shims (`mise activate --shims`)

Small executables in `~/.local/share/mise/shims/` that intercept commands and load the right context. Better for non-interactive use.

Pros:
- Works in IDEs, scripts, CI
- No prompt hook overhead
- Simple PATH addition

Cons:
- Doesn't support shell aliases
- Small overhead per command invocation (must resolve versions each time)
- Some features unavailable (see shims vs PATH docs)

### Neither (explicit `mise exec` / `mise run`)

Use `mise exec` or `mise run` directly without any shell integration.

Pros:
- No shell configuration needed
- Works everywhere
- Most explicit

Cons:
- Verbose (must prefix every command)
- Not suitable for interactive development workflows

## Examples

```sh
# PATH activation (interactive shell)
eval "$(mise activate zsh)"

# Shims (add to PATH in profile)
export PATH="$HOME/.local/share/mise/shims:$PATH"

# Explicit (no activation)
mise exec node@20 -- node server.js
```

## See Also

- cmd-activate
- concept-shims
- cmd-exec
