---
id: cmd-outdated
title: "mise outdated"
category: commands
tags: [command, tool, update]
source: https://github.com/jdx/mise/blob/main/docs/cli/outdated.md
related: [cmd-upgrade, cmd-ls, cmd-ls-remote]
---

## Summary

Shows tools that have newer versions available than what's currently installed or configured.

## Syntax / Usage

```sh
mise outdated [TOOL]…
```

## Details

Compares installed versions against the latest available versions from their respective backends. Useful for identifying tools that need updating.

## Examples

```sh
# Check all tools
mise outdated

# Check specific tool
mise outdated node

# Update outdated tools
mise upgrade
```

## See Also

- cmd-upgrade
- cmd-ls
- cmd-ls-remote
