---
id: cmd-self-update
title: "mise self-update"
category: commands
tags: [command, installation, update]
source: https://github.com/jdx/mise/blob/main/docs/cli/self-update.md
related: [installation-curl, cmd-doctor, cmd-version]
---

## Summary

Updates mise itself to the latest version. The simplest way to keep mise current.

## Syntax / Usage

```sh
mise self-update
```

## Details

Downloads and replaces the mise binary with the latest release. Works regardless of how mise was originally installed (curl, cargo, etc.). If installed via a package manager (brew, apt), prefer using that package manager to update instead.

## Examples

```sh
# Update to latest
mise self-update

# Check current version
mise --version
```

## See Also

- installation-curl
- cmd-doctor
