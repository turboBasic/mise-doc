---
id: ts-slow-prompt
title: "Slow shell prompts with mise activate"
category: troubleshooting
tags: [troubleshooting, performance, activation, shell]
source: https://github.com/jdx/mise/blob/main/docs/troubleshooting.md
related: [concept-activation, concept-shims, cmd-activate]
---

## Summary

The prompt hook from `mise activate` runs on every prompt. If prompts feel sluggish, profile with `MISE_TIMINGS` to identify the bottleneck.

## Symptom

Shell prompt takes noticeably long to appear after each command. Responsiveness degrades, especially in projects with complex `mise.toml`.

## Cause

Common causes of slow prompts:
- Expensive `_.source` scripts in `[env]` (re-run on every prompt)
- Large numbers of tools or plugins
- Network-dependent operations in env directives
- Slow filesystem (network mounts, spinning disks)

## Fix / Workaround

### Profile the hook

```sh
# Deactivate first so the hook doesn't interfere
mise deactivate

# Show timing per major step (color-coded: red = slow)
MISE_TIMINGS=1 mise hook-env -s zsh 2>&1 >/dev/null

# Detailed per-step breakdown with cumulative time
MISE_TIMINGS=2 mise hook-env -s zsh 2>&1 >/dev/null
```

### Solutions

1. **Move expensive `_.source` to `_.file`** — file reads are fast, script execution is slow.
2. **Reduce tool count** — each tool adds overhead to version resolution.
3. **Switch to shims** — moves cost from every prompt to every tool invocation:
   ```sh
   eval "$(mise activate --shims)"
   ```
4. **Use `mise exec` or `mise run`** for one-off commands instead of activation.

## Examples

```sh
# Profile to find the bottleneck
MISE_TIMINGS=2 mise hook-env -s zsh 2>&1 >/dev/null

# Switch to shims if prompt overhead is unacceptable
# Replace your activate line with:
eval "$(mise activate --shims)"
```

## See Also

- concept-activation
- concept-shims
- ts-mise-not-working
