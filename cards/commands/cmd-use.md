---
id: cmd-use
title: "mise use"
category: commands
tags: [command, tool, installation, config]
source: https://github.com/jdx/mise/blob/main/docs/cli/use.md
related: [cmd-install, cmd-exec, cmd-ls, concept-backends]
---

## Summary

Installs a tool and adds the version to `mise.toml`. The primary way to declare which tools a project needs.

## Syntax / Usage

```sh
mise use [FLAGS] [TOOL@VERSION]…
```

Alias: `mise u`

## Details

`mise use` installs the specified tool version (if not already installed) and writes the version to `mise.toml`. By default it writes to the `mise.toml` in the current directory. If multiple config files exist (e.g., both `mise.toml` and `mise.local.toml`), the lowest precedence file (`mise.toml`) is used.

Target file selection order:
1. `--global` → `~/.config/mise/config.toml`
2. `--path <PATH>` → specified file
3. `--env <ENV>` → `mise.<env>.toml`
4. `MISE_DEFAULT_CONFIG_FILENAME` → that filename
5. Otherwise → `mise.toml` in current directory

Tool options can be set with bracket syntax: `mise use ubi:BurntSushi/ripgrep[exe=rg]`

## Examples

```sh
# Interactive selector (no arguments)
mise use

# Set node 20 in local mise.toml (writes fuzzy version)
mise use node@20

# Set node 20 globally with exact pinned version
mise use -g --pin node@20

# Install from a specific backend
mise use github:BurntSushi/ripgrep

# Write to environment-specific config
mise use --env staging node@20

# Write to local config (not committed)
mise use --env local node@20

# Remove a tool from config
mise use --remove node

# Dry run — show what would change
mise use --dry-run node@22
```

## Caveats / Common Mistakes

- By default, fuzzy versions are written (e.g., `20` not `20.0.0`). Use `--pin` or set `MISE_PIN=1` to write exact versions.
- Consider using `mise.lock` instead of `--pin` for reproducibility — it locks versions without polluting `mise.toml`.
- `mise use` automatically trusts the file it creates, so you won't see trust prompts for files you generate yourself.

## See Also

- cmd-install
- cmd-exec
- cmd-unuse
- concept-backends
