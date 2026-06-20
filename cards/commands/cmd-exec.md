---
id: cmd-exec
title: "mise exec"
category: commands
tags: [command, tool, execution]
source: https://github.com/jdx/mise/blob/main/docs/cli/exec.md
related: [cmd-use, cmd-run, concept-activation]
---

## Summary

Execute a command with mise tool versions and environment variables loaded. The quickest way to run a tool at a specific version without activating mise in your shell.

## Syntax / Usage

```sh
mise exec [TOOL@VERSION]… -- <COMMAND>…
mise x [TOOL@VERSION]… -- <COMMAND>…
```

Alias: `mise x`

## Details

`mise exec` sets up the full mise environment (tools on PATH, env vars loaded) then executes the given command. It installs tools on-the-fly if they aren't already installed. This works without `mise activate` — useful for scripts, CI, one-off commands, and shebangs.

If no tool version is specified, it uses versions from the active `mise.toml` config files. Everything after `--` is passed to the command.

## Examples

```sh
# Run python at a specific version
mise exec python@3.12 -- python --version

# Short alias
mise x node@20 -- node -v

# Use versions from mise.toml (no tool spec needed)
mise exec -- npm test

# Multiple tools
mise exec node@20 python@3.12 -- node script.js

# In a shebang
#!/usr/bin/env -S mise x node@20 --
console.log("Hello from node!")
```

## Caveats / Common Mistakes

- The `--` separator is required between mise arguments and the command to execute.
- Tools are installed automatically if missing, which may cause a delay on first run.
- For interactive shells, prefer `mise activate` over wrapping everything in `mise exec`.

## See Also

- cmd-use
- cmd-run
- concept-activation
- concept-shims
