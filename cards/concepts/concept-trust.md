---
id: concept-trust
title: "Config Trust Model"
category: concepts
tags: [security, trust, config, concept]
source: https://github.com/jdx/mise/blob/main/docs/getting-started.md
related: [cmd-trust, cmd-untrust, config-mise-toml]
---

## Summary

Mise requires explicit trust before executing code from config files. This prevents malicious `mise.toml` files from running arbitrary commands when you clone a repository.

## Details

Config files can execute arbitrary code via:
- `[env]` directives (especially `_.source` which executes scripts)
- Hooks (enter, leave, cd, etc.)
- Tasks

When mise encounters an untrusted config file containing any of these, it displays:

```
mise ~/my-project/mise.toml is not trusted. Trust it? [y/n]
```

Trust is recorded per-file and persists across sessions. Plain `[tools]` declarations don't require trust — only executable content.

### Disabling trust prompts

```sh
# Trust all paths unconditionally
mise settings trusted_config_paths=["/"]

# Or via environment variable
export MISE_TRUSTED_CONFIG_PATHS=/
```

### How trust is tracked

Trust state is stored in `~/.local/share/mise/trusted-configs/`. Each trusted file gets an entry recording its path and a hash.

## Examples

```sh
# Trust after cloning a project
cd ~/projects/new-repo
mise trust

# Check trust status
mise untrust --show

# Trust a specific file
mise trust ./mise.toml
```

## Caveats / Common Mistakes

- `mise use` auto-trusts the file it creates — trust prompts only appear for files from others
- If a trusted file is modified (e.g., after `git pull`), it may need re-trusting
- Trust is per-machine — team members each need to trust independently

## See Also

- cmd-trust
- cmd-untrust
- config-mise-toml
