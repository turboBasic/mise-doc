---
id: cmd-env
title: "mise env"
category: commands
tags: [command, env, shell]
source: https://github.com/jdx/mise/blob/main/docs/cli/env.md
related: [env-variables, cmd-exec, cmd-activate]
---

## Summary

Outputs environment variables that mise would set. Useful for debugging, non-interactive shells, and integration with other tools.

## Syntax / Usage

```sh
mise env [FLAGS] [TOOL@VERSION]…
mise en [FLAGS] [TOOL@VERSION]…
```

Alias: `mise en`

## Details

Prints the environment variables that would be set by mise, including PATH modifications and vars from `[env]`. By default outputs shell-compatible `export` statements. Useful for piping into `eval` or debugging what mise would do.

## Examples

```sh
# Show what mise would export
mise env

# Evaluate in current shell (non-interactive alternative to activate)
eval "$(mise env)"

# Show env for specific tools
mise env node@20 python@3.12

# JSON output
mise env --json

# Only show PATH changes
mise env | grep PATH
```

## Caveats / Common Mistakes

- `mise env` without `mise activate` only loads global config. It won't respond to directory changes.
- For scripts that need project-specific tools, use `mise exec` instead.

## See Also

- env-variables
- cmd-exec
- cmd-activate
