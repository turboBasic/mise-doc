---
id: cmd-shell
title: "mise shell"
category: commands
tags: [command, tool, shell, session]
source: https://github.com/jdx/mise/blob/main/docs/cli/shell.md
related: [cmd-use, cmd-activate, cmd-exec]
---

## Summary

Sets a tool version for the current shell session only. Does not modify any config files — the version is active until the shell exits.

## Syntax / Usage

```sh
mise shell <TOOL@VERSION>…
```

## Details

`mise shell` sets a tool version for the current shell session by modifying the environment. Unlike `mise use`, it does not write to `mise.toml`. The change is lost when the shell exits.

Requires `mise activate` to be active in the shell.

## Examples

```sh
# Use node 18 for this session only
mise shell node@18

# Use multiple tools
mise shell node@18 python@3.11

# Revert to config-defined version
mise shell --unset node
```

## Caveats / Common Mistakes

- Requires `mise activate` — does not work with shims-only setup.
- Session-only: the version is not written to any config file and is lost when the shell exits.

## See Also

- cmd-use
- cmd-activate
- cmd-exec
