---
id: cmd-settings
title: "mise settings"
category: commands
tags: [command, config, settings]
source: https://github.com/jdx/mise/blob/main/docs/cli/settings.md
related: [config-settings, cmd-use, config-mise-toml]
---

## Summary

View and modify mise settings. Settings control mise's own behavior (not project tools or env vars).

## Syntax / Usage

```sh
mise settings [SUBCOMMAND]
mise settings ls
mise settings get <KEY>
mise settings set <KEY> <VALUE>
mise settings unset <KEY>
mise settings add <KEY> <VALUE>
```

## Details

Settings are stored in `~/.config/mise/config.toml` under the `[settings]` section or can be set via environment variables prefixed with `MISE_`. Settings control behaviors like experimental features, trust, parallelism, and more.

## Examples

```sh
# List all settings
mise settings ls

# Get a specific setting
mise settings get experimental

# Enable experimental features
mise settings set experimental true

# Set trusted paths
mise settings set trusted_config_paths '["/home/user/work"]'

# Unset a setting (revert to default)
mise settings unset jobs
```

## See Also

- config-settings
- config-mise-toml
