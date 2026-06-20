---
id: cmd-doctor
title: "mise doctor"
category: commands
tags: [command, troubleshooting, diagnostics]
source: https://github.com/jdx/mise/blob/main/docs/cli/doctor.md
related: [ts-mise-not-working, ts-wrong-version, cmd-self-update]
---

## Summary

Displays diagnostic information about your mise installation and detects potential issues. Include its output in bug reports.

## Syntax / Usage

```sh
mise doctor
mise dr
```

Alias: `mise dr`

## Details

`mise doctor` checks your mise installation for common problems: whether mise is activated, shell integration is correct, tools are installed, PATH is configured properly, and config files are trusted. It outputs warnings for anything that looks wrong.

## Examples

```sh
# Run diagnostics
mise doctor

# Check PATH configuration specifically
mise doctor path
```

## See Also

- ts-mise-not-working
- cmd-self-update
- installation-curl
