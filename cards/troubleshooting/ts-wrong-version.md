---
id: ts-wrong-version
title: "Wrong tool version being used"
category: troubleshooting
tags: [troubleshooting, tool, version, path]
source: https://github.com/jdx/mise/blob/main/docs/troubleshooting.md
related: [concept-activation, concept-shims, cmd-ls, cmd-which, cmd-where]
---

## Summary

When the wrong version of a tool is active, it usually means mise isn't first in PATH, or the config file specifies a different version than expected.

## Symptom

Running `node --version` (or any tool) shows a different version than what's in `mise.toml`.

## Cause

Common causes:
1. Another tool manager (nvm, asdf, volta) is earlier in PATH
2. System-installed version shadows mise's version
3. The config file in a parent directory overrides the local one
4. The tool version isn't installed yet

## Fix / Workaround

### Step 1: Verify mise has the version installed and active

```sh
mise ls node
```

Check that the "Requested" column matches your expectation and it doesn't say "missing".

### Step 2: Check which binary is being used

```sh
which -a node
mise which node
mise where node
```

If `which node` returns a path outside mise's install directory, something else is earlier in PATH.

### Step 3: Check config resolution

```sh
mise config ls    # which config files are loaded
mise ls --current # what's currently active
```

### Step 4: Fix PATH ordering

Ensure `mise activate` is the last thing modifying PATH in your rc file, or that no other version manager overrides it.

## Examples

```sh
# Diagnose the problem
mise ls node
which -a node
mise which node

# Force reinstall if corrupted
mise install --force node@20

# Check all active config
mise config ls
```

## See Also

- concept-activation
- concept-shims
- cmd-ls
- cmd-which
- cmd-doctor
