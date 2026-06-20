---
id: concept-lock
title: "Lockfiles (mise.lock)"
category: concepts
tags: [concept, config, reproducibility, security]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/mise-lock.md
related: [cmd-use, cmd-install, config-settings]
---

## Summary

`mise.lock` pins exact tool versions and download URLs for reproducible installs. It's the preferred alternative to `--pin` in `mise.toml` — keeps fuzzy versions in config while locking exact versions separately.

## Details

When lockfile support is enabled, `mise install` and `mise use` write resolved versions and pre-resolved download URLs to `mise.lock`. Subsequent installs use the lockfile instead of resolving versions dynamically.

### Enabling lockfiles

```toml
# mise.toml
[settings]
lockfile = true
```

Or: `mise settings set lockfile true`

### What mise.lock contains

- Exact resolved versions (e.g., `node 20` → `node 20.11.1`)
- Pre-resolved download URLs for the current platform
- Checksums where available

### Workflow

1. `mise.toml` declares fuzzy versions: `node = "20"`
2. `mise.lock` records: `node 20.11.1 https://nodejs.org/dist/v20.11.1/...`
3. Commit both files to git
4. Teammates get exact same versions without network resolution

### Updating the lockfile

```sh
mise install --force  # re-resolves and updates mise.lock
```

## Examples

```toml
# mise.toml — human-friendly fuzzy versions
[tools]
node = "20"
python = "3.12"

[settings]
lockfile = true
```

```sh
# mise.lock is auto-generated
mise install  # creates/updates mise.lock

# With --locked, fails if lockfile is incomplete
mise install --locked
```

## Caveats / Common Mistakes

- `mise.lock` is platform-specific (URLs differ per OS/arch). Team members on different platforms may see lock conflicts.
- `--locked` flag in CI ensures no network resolution occurs — fails if the lockfile is incomplete.
- Lockfiles are an alternative to `--pin`, not a replacement for `mise.toml`.

## See Also

- cmd-use
- cmd-install
- config-settings
