---
id: ts-mise-not-working
title: "General debugging when mise isn't working"
category: troubleshooting
tags: [troubleshooting, debugging, diagnostics]
source: https://github.com/jdx/mise/blob/main/docs/troubleshooting.md
related: [cmd-doctor, ts-wrong-version, ts-activate-not-working]
---

## Summary

General debugging steps when mise isn't behaving as expected: enable debug output, check diagnostics, verify activation, and try alternative execution methods.

## Symptom

mise isn't loading tools, env vars aren't set, tasks fail, or behavior is unexpected.

## Fix / Workaround

### Step 1: Enable debug output

```sh
MISE_DEBUG=1 mise <command>
# or for even more detail:
MISE_TRACE=1 mise <command>

# Write logs to a file
MISE_LOG_FILE_LEVEL=debug MISE_LOG_FILE=/tmp/mise.log mise <command>
```

### Step 2: Run diagnostics

```sh
mise doctor
```

Include this output in any bug report.

### Step 3: Check activation

If the hook isn't firing, try manually:
```sh
eval "$(mise hook-env)"
mise env  # just outputs env vars
```

### Step 4: Try raw mode

If tool installation hangs or fails:
```sh
mise install --raw node@20
```

This connects stdin/stdout directly to the terminal (some installers need interaction).

### Step 5: Nuclear options

```sh
mise cache clean    # wipe internal cache
mise self-update    # update to latest mise
mise implode        # remove everything except config (last resort)
```

## Examples

```sh
# Quick debugging flow
mise doctor
MISE_DEBUG=1 mise ls
mise config ls

# Check timing if prompts are slow
MISE_TIMINGS=1 mise hook-env -s zsh 2>&1 >/dev/null
```

## See Also

- cmd-doctor
- ts-wrong-version
- ts-activate-not-working
- ts-slow-prompt
