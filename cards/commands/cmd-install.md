---
id: cmd-install
title: "mise install"
category: commands
tags: [command, tool, installation]
source: https://github.com/jdx/mise/blob/main/docs/cli/install.md
related: [cmd-use, cmd-uninstall, cmd-ls]
---

## Summary

Install tool versions declared in config files or specified on the command line. Unlike `mise use`, this does not modify `mise.toml`.

## Syntax / Usage

```sh
mise install [FLAGS] [TOOL@VERSION]…
mise i [TOOL@VERSION]…
```

Alias: `mise i`

## Details

`mise install` installs tools without writing to config. If no arguments are given, it installs all tools declared in the active `mise.toml` files. If a specific tool@version is given, it installs just that version. This is the command to run after cloning a project with an existing `mise.toml`.

## Examples

```sh
# Install all tools from mise.toml
mise install

# Install a specific tool version
mise install node@20.11.0

# Force reinstall
mise install --force node@20

# Install with verbose output
mise install -v node@20
```

## Caveats / Common Mistakes

- `mise install` without arguments reads from config — if no `mise.toml` exists, nothing happens.
- To both install and record in config, use `mise use` instead.
- If installation fails, try `--raw` to connect stdin/stdout directly (some installers need terminal interaction).

## See Also

- cmd-use
- cmd-uninstall
- cmd-ls
