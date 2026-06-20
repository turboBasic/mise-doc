---
id: concept-config-hierarchy
title: "Configuration Hierarchy"
category: concepts
tags: [config, concept, precedence]
source: https://github.com/jdx/mise/blob/main/docs/configuration.md
related: [config-mise-toml, config-environments, config-settings]
---

## Summary

Mise uses a hierarchical configuration system that walks up the directory tree, merging config files from most-specific (closest to current directory) to least-specific (global). More specific files override less specific ones.

## Details

### Resolution process

1. Walks up the directory tree from the current directory to root (or `MISE_CEILING_PATHS`)
2. Collects all config files found along the way
3. Merges them with closer files overriding broader ones
4. Applies environment-specific configs (if `MISE_ENV` is set)

### File precedence (within a directory, top wins)

1. `mise.local.toml` — local overrides, not committed
2. `mise.toml`
3. `mise/config.toml`
4. `.mise/config.toml`
5. `.config/mise.toml`
6. `.config/mise/config.toml`
7. `.config/mise/conf.d/*.toml` — alphabetical fragments

### Merge behavior by section

- **Tools** (`[tools]`): Additive with overrides. A tool declared in a child overrides the same tool from parent.
- **Env vars** (`[env]`): Additive with overrides. Same variable in child wins.
- **Tasks** (`[tasks]`): Completely replaced per-task. If a child defines `test`, it fully replaces the parent's `test`.
- **Settings** (`[settings]`): Additive with overrides.

### Directory tree example

```
~/.config/mise/config.toml     → node@18, python@3.11
~/work/mise.toml               → python@3.12
~/work/myproj/mise.toml        → node@20, go@1.22
```

In `~/work/myproj/`: node@20, python@3.12, go@1.22

## Examples

```sh
# See which config files are active and their order
mise config ls

# See the resolved tools
mise ls --current
```

## Caveats / Common Mistakes

- `mise.local.toml` has the highest precedence in its directory — use it for machine-specific overrides that shouldn't be committed.
- Run `mise cfg` to see the actual file load order on your system. This is easier than memorizing the rules.

## See Also

- config-mise-toml
- config-environments
- config-settings
