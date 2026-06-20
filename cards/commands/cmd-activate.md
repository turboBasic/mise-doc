---
id: cmd-activate
title: "mise activate"
category: commands
tags: [command, activation, shell]
source: https://github.com/jdx/mise/blob/main/docs/cli/activate.md
related: [concept-activation, concept-shims, cmd-deactivate, cmd-shell]
---

## Summary

Outputs shell code that activates mise in the current shell session. When evaluated, it hooks into the prompt to keep PATH and environment variables updated as you change directories.

## Syntax / Usage

```sh
eval "$(mise activate <SHELL>)"
```

Supported shells: `bash`, `zsh`, `fish`, `nushell`, `elvish`, `xonsh`, `pwsh`

## Details

`mise activate` installs a prompt hook that runs on every prompt display. The hook calls `mise hook-env` to detect config file changes and update PATH/env vars accordingly. This is the recommended approach for interactive shells.

Add the activation line to your shell's rc file (`.bashrc`, `.zshrc`, `.config/fish/config.fish`, etc.).

For non-interactive use (CI, IDE, scripts), consider shims or `mise exec` instead.

## Examples

```sh
# Bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# Zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# Fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish

# PowerShell
(&mise activate pwsh) | Out-String | Invoke-Expression

# Activate with shims mode instead of PATH manipulation
eval "$(mise activate --shims)"
```

## Caveats / Common Mistakes

- Only use in rc files (`.bashrc`, `.zshrc`), NOT in profile files (`.profile`, `.bash_profile`, `.zprofile`). Profile files run in non-interactive contexts where the prompt hook won't fire.
- `mise activate --shims` does not support all features of `mise activate`. See the shims docs for differences.
- For Homebrew + Fish, mise activates automatically — no configuration needed. Disable with `set -Ux MISE_FISH_AUTO_ACTIVATE 0`.

## See Also

- concept-activation
- concept-shims
- cmd-deactivate
- cmd-hook-env
