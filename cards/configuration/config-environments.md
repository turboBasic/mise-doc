---
id: config-environments
title: "Configuration Environments"
category: configuration
tags: [config, env, environments]
source: https://github.com/jdx/mise/blob/main/docs/configuration/environments.md
related: [config-mise-toml, concept-config-hierarchy, env-variables]
---

## Summary

Configuration environments allow different tool versions, env vars, and tasks per deployment context (development, staging, production). Activated via `MISE_ENV` or `mise --env`.

## Syntax / Usage

```sh
# Activate an environment
export MISE_ENV=staging
# or
mise --env staging exec -- node server.js
# or
mise use --env staging node@18
```

## Details

When `MISE_ENV` is set to e.g., `staging`, mise additionally loads `mise.staging.toml` (or `.mise.staging.toml`) alongside the base `mise.toml`. The environment-specific file's values override the base config.

File naming: `mise.<env>.toml` or `.mise.<env>.toml`

### Platform environments

mise can auto-detect platform-specific environments (no `MISE_ENV` required):
- `mise.macos.toml` — loaded on macOS
- `mise.linux.toml` — loaded on Linux
- `mise.windows.toml` — loaded on Windows
- `mise.macos-arm64.toml` — loaded on macOS ARM64

Enable with `auto_env` setting.

### Local overrides

`mise.<env>.local.toml` files exist for per-machine overrides within an environment. These should be gitignored.

## Examples

```toml
# mise.toml (base)
[tools]
node = "20"

[env]
DATABASE_URL = "postgres://localhost/dev"

# mise.staging.toml (overrides for staging)
[env]
DATABASE_URL = "postgres://staging-host/app"
NODE_ENV = "staging"
```

```sh
# Use staging environment
MISE_ENV=staging mise exec -- node server.js

# Create environment-specific config
mise use --env staging node@18
```

## See Also

- config-mise-toml
- concept-config-hierarchy
- env-variables
