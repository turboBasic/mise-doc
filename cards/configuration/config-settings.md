---
id: config-settings
title: "Settings Reference"
category: configuration
tags: [config, settings]
source: https://github.com/jdx/mise/blob/main/docs/configuration/settings.md
related: [cmd-settings, config-mise-toml, concept-trust]
---

## Summary

Settings control mise's own behavior (parallelism, trust, experimental features, etc.). They can be set in config files, via CLI, or through environment variables.

## Syntax / Usage

```toml
# In ~/.config/mise/config.toml or project mise.toml
[settings]
experimental = true
jobs = 8
trusted_config_paths = ["/home/user/work"]
```

Or via environment variables (prefix `MISE_`):
```sh
export MISE_EXPERIMENTAL=1
export MISE_JOBS=8
```

## Details

### Key settings

| Setting                | Default | Description                                      |
| ---------------------- | ------- | ------------------------------------------------ |
| `experimental`         | `false` | Enable experimental features                     |
| `jobs`                 | `4`     | Parallel installation jobs                       |
| `trusted_config_paths` | `[]`    | Paths to auto-trust                              |
| `verbose`              | `false` | Show extra output                                |
| `pin`                  | `false` | Pin exact versions by default in `mise use`      |
| `lockfile`             | `false` | Enable mise.lock for reproducibility             |
| `paranoid`             | `false` | Extra security checks                            |
| `disable_tools`        | `[]`    | Tools to ignore                                  |
| `raw`                  | `false` | Connect stdin/stdout directly                    |

### Environment variable mapping

Any setting can be set via `MISE_<SETTING_NAME>` in SCREAMING_SNAKE_CASE:
- `experimental` → `MISE_EXPERIMENTAL`
- `trusted_config_paths` → `MISE_TRUSTED_CONFIG_PATHS`
- `jobs` → `MISE_JOBS`

### Debug and trace

```sh
MISE_DEBUG=1    # Show debug output
MISE_TRACE=1    # Show trace output (very verbose)
MISE_LOG_FILE=/path/to/logfile  # Write logs to file
MISE_LOG_FILE_LEVEL=debug       # Log file verbosity
```

## Examples

```sh
# Enable experimental features
mise settings set experimental true

# Set parallel jobs
mise settings set jobs 8

# Trust all work directories
mise settings set trusted_config_paths '["/home/user/work"]'

# List current settings
mise settings ls
```

## See Also

- cmd-settings
- config-mise-toml
- concept-trust
