---
id: hooks-overview
title: "Hooks Overview"
category: hooks
tags: [hook, lifecycle, shell]
source: https://github.com/jdx/mise/blob/main/docs/hooks.md
related: [config-mise-toml, concept-trust, env-variables]
---

## Summary

Hooks are shell commands that run automatically at specific lifecycle points: entering a directory, leaving it, changing directories, before/after tool installation, etc.

## Syntax / Usage

```toml
[hooks.enter]
shell = "echo 'Entered {{config_root}}'"

[hooks.leave]
shell = "echo 'Left project'"

[hooks.cd]
shell = "echo 'Changed to {{cwd}}'"

[hooks.preinstall]
shell = "echo 'About to install tools'"

[hooks.postinstall]
shell = "echo 'Tools installed'"
```

## Details

### Available hooks

| Hook           | Trigger                                     |
| -------------- | ------------------------------------------- |
| `enter`        | First time mise loads this config file       |
| `leave`        | Leaving the directory (config no longer active) |
| `cd`           | Every directory change while config is active |
| `preinstall`   | Before installing tools                      |
| `postinstall`  | After installing tools                       |

### Hook fields

```toml
[hooks.enter]
shell = "command to run"    # Shell command
wait_for = ["other-hook"]   # Wait for another hook to complete
```

### Templates in hooks

Hook commands support Tera templates:
- `{{config_root}}` — directory containing the mise.toml
- `{{cwd}}` — current working directory
- `{{env.VAR}}` — environment variables

### Trust requirement

Hooks execute arbitrary code, so the config file must be trusted (`mise trust`).

## Examples

```toml
# Activate a Python virtualenv on enter
[hooks.enter]
shell = "source .venv/bin/activate 2>/dev/null || true"

# Show project info on enter
[hooks.enter]
shell = "echo 'Welcome to {{config_root}}'"

# Run migrations after tool install
[hooks.postinstall]
shell = "mise run db:migrate"
```

## Caveats / Common Mistakes

- Hooks require the config file to be trusted.
- The `cd` hook fires on every directory change while the config is active — keep it fast.
- The `enter` hook fires once when the config becomes active, not on every `cd`.
- Hooks run in the context of the shell, so they can modify the environment.

## See Also

- config-mise-toml
- concept-trust
- env-variables
