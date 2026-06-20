---
id: ts-activate-not-working
title: "mise activate doesn't work in profile files"
category: troubleshooting
tags: [troubleshooting, activation, shell]
source: https://github.com/jdx/mise/blob/main/docs/troubleshooting.md
related: [concept-activation, cmd-activate, concept-shims, installation-activation]
---

## Summary

`mise activate` only works in interactive shell rc files (`.bashrc`, `.zshrc`). It does not work in profile files (`.profile`, `.bash_profile`, `.zprofile`) because the prompt hook never fires in non-interactive shells.

## Symptom

Tools installed via mise are not available in scripts, IDEs, or shells started from login profile files. The prompt hook is never triggered.

## Cause

`mise activate` installs a prompt hook that updates PATH on every prompt display. Profile files (`.profile`, `.bash_profile`, `.zprofile`) run in non-interactive contexts where no prompt is displayed, so the hook never fires.

## Fix / Workaround

1. Move the `eval "$(mise activate ...)"` line to the rc file (`.bashrc`, `.zshrc`), not the profile file.

2. For non-interactive contexts, use shims instead:
```sh
export PATH="$HOME/.local/share/mise/shims:$PATH"
```

3. Or use `mise exec` directly:
```sh
mise exec -- node script.js
```

4. Or use `mise env` to export vars (only global tools):
```sh
eval "$(mise env)"
```

## Examples

```sh
# WRONG — won't work in .bash_profile
echo 'eval "$(mise activate bash)"' >> ~/.bash_profile

# CORRECT — use in .bashrc
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# For non-interactive (scripts, cron, IDE):
export PATH="$HOME/.local/share/mise/shims:$PATH"
```

## See Also

- concept-activation
- concept-shims
- installation-activation
