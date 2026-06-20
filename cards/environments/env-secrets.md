---
id: env-secrets
title: "Secrets Management"
category: environments
tags: [env, secrets, security, sops, age]
source: https://github.com/jdx/mise/blob/main/docs/environments/secrets/index.md
related: [env-variables, config-mise-toml, concept-trust]
---

## Summary

Mise can decrypt secrets from encrypted files using SOPS or age, injecting them as environment variables without storing plaintext in the repository.

## Syntax / Usage

```toml
[env]
# SOPS-encrypted file
_.file = { path = "secrets.enc.env", sops = true }

# age-encrypted file
_.file = { path = "secrets.age", age = true }
```

## Details

### SOPS integration

SOPS (Secrets OPerationS) supports multiple encryption backends (age, AWS KMS, GCP KMS, etc.). Mise shells out to `sops` to decrypt:

```toml
[env]
_.file = { path = "secrets.enc.json", sops = true }
```

Requires `sops` CLI to be installed and encryption keys configured.

### age integration

age is a simpler encryption tool. Mise can decrypt `.age` files directly:

```toml
[env]
_.file = { path = ".env.age", age = true }
```

The age identity (private key) is read from `~/.config/age/keys.txt` or `SOPS_AGE_KEY_FILE`.

### Workflow

1. Encrypt secrets: `sops -e .env > secrets.enc.env`
2. Commit encrypted file to git
3. Reference in mise.toml: `_.file = { path = "secrets.enc.env", sops = true }`
4. Team members with keys get decrypted values automatically

## Examples

```toml
# mise.toml
[env]
# Public env vars
NODE_ENV = "production"

# Secrets (decrypted at runtime)
_.file = { path = "secrets.enc.env", sops = true }
```

## Caveats / Common Mistakes

- Encrypted files require the decryption key to be available on the machine.
- `sops` or `age` CLI must be installed (mise can manage them: `mise use sops`).
- The decrypted values are only in memory — never written to disk in plaintext.

## See Also

- env-variables
- config-mise-toml
- concept-trust
