---
id: env-variables
title: "Environment Variables in mise.toml"
category: environments
tags: [env, config, variables]
source: https://github.com/jdx/mise/blob/main/docs/environments/index.md
related: [config-mise-toml, config-environments, env-secrets]
---

## Summary

The `[env]` section in `mise.toml` defines environment variables that are loaded when mise is activated or when using `mise exec`/`mise run`. Supports static values, file loading, script sourcing, templates, and PATH manipulation.

## Syntax / Usage

```toml
[env]
# Simple key-value
NODE_ENV = "development"
API_URL = "http://localhost:3000"

# Load from .env file
_.file = ".env"
# or multiple files
_.file = [".env", ".env.local"]

# Source a script (requires trust)
_.source = "./load-secrets.sh"

# PATH manipulation
_.path = ["./node_modules/.bin", "./bin"]

# Templates (using tera syntax)
DB_URL = "postgres://{{env.USER}}@localhost/{{env.MISE_ENV}}_db"

# Unset a variable
MY_VAR = false
```

## Details

### Special directives (underscore prefix)

| Directive    | Description                                          |
| ------------ | ---------------------------------------------------- |
| `_.file`     | Load env vars from a .env file                       |
| `_.source`   | Execute a script and capture its exported vars       |
| `_.path`     | Prepend directories to PATH                          |

### Templates

Env values support Tera templates with access to:
- `env.VAR_NAME` — other environment variables
- `config_root` — directory containing the mise.toml
- Other mise template functions

### Ordering

Env vars are set in the order they appear in the file. Later entries can reference earlier ones via templates.

## Examples

```toml
[env]
NODE_ENV = "development"
PORT = "3000"
_.file = ".env"
_.path = ["./node_modules/.bin"]
DATABASE_URL = "postgres://localhost/myapp_{{env.NODE_ENV}}"
```

```sh
# Verify env is loaded
mise env
# or
mise exec -- env | grep NODE_ENV
```

## Caveats / Common Mistakes

- `_.source` scripts require the config file to be trusted (security measure).
- `.env` files loaded via `_.file` are read but don't require trust (they can't execute code).
- `_.source` scripts re-run on every prompt when using `mise activate` — keep them fast.
- Use `false` (boolean) as the value to unset a variable.

## See Also

- config-mise-toml
- config-environments
- env-secrets
