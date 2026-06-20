---
id: cmd-ls
title: "mise ls"
category: commands
tags: [command, tool, inspection]
source: https://github.com/jdx/mise/blob/main/docs/cli/ls.md
related: [cmd-use, cmd-install, cmd-ls-remote, cmd-outdated]
---

## Summary

List installed tool versions and their status (active, requested version, source config file).

## Syntax / Usage

```sh
mise ls [FLAGS] [TOOL]…
```

## Details

Shows all installed tool versions. When a tool is specified, shows only versions of that tool. The output includes the requested version, installed version, whether it's active, and which config file requested it. Useful for diagnosing "wrong version" issues.

## Examples

```sh
# List all installed tools
mise ls

# List only node versions
mise ls node

# Show in JSON format
mise ls --json

# Show only currently active tools
mise ls --current
```

## See Also

- cmd-ls-remote
- cmd-outdated
- cmd-where
- cmd-which
