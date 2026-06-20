---
id: cmd-trust
title: "mise trust"
category: commands
tags: [command, security, config]
source: https://github.com/jdx/mise/blob/main/docs/cli/trust.md
related: [concept-trust, cmd-untrust, config-mise-toml]
---

## Summary

Marks a config file as trusted so mise will load its env directives, hooks, and tasks. Required security measure for files you didn't create yourself.

## Syntax / Usage

```sh
mise trust [PATH]
```

## Details

Config files can execute arbitrary code via `[env]` directives, hooks, and tasks. When mise encounters an untrusted config file, it prompts before executing any of these. Running `mise trust` marks the file as safe.

`mise use` automatically trusts the file it creates, so you only see prompts for files written by others (e.g., after `git pull`).

To trust all config files unconditionally: `mise settings trusted_config_paths=["/"]`

## Examples

```sh
# Trust the config in the current directory
mise trust

# Trust a specific file
mise trust ~/projects/myapp/mise.toml

# Trust all paths (disable trust prompts entirely)
mise settings trusted_config_paths=["/"]
```

## Caveats / Common Mistakes

- Trust is per-file — you must trust each config file individually unless you set `trusted_config_paths`.
- After `git pull` brings new `mise.toml` changes, you may need to re-trust.
- The trust prompt only appears for files with env directives, hooks, or tasks — plain tool declarations don't require trust.

## See Also

- concept-trust
- cmd-untrust
- config-mise-toml
