---
id: installation-activation
title: "Shell Activation Setup"
category: installation
tags: [installation, shell, activation, setup]
source: https://github.com/jdx/mise/blob/main/docs/getting-started.md
related: [installation-curl, cmd-activate, concept-activation, concept-shims]
---

## Summary

After installing mise, you need to activate it in your shell for tools and env vars to load automatically. Add the activation line to your shell's rc file.

## Syntax / Usage

```sh
# Bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# Zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# Fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish

# PowerShell
# Add to $PROFILE:
(&mise activate pwsh) | Out-String | Invoke-Expression
```

## Details

### Shell compatibility

| Feature                         | Bash | Zsh | Fish | Nushell | Elvish | Xonsh | PowerShell |
| ------------------------------- | ---- | --- | ---- | ------- | ------ | ----- | ---------- |
| `mise activate`                 | Yes  | Yes | Yes  | Yes     | Yes    | Yes   | Yes        |
| `mise shell`                    | Yes  | Yes | Yes  | Yes     | Yes    | Yes   | Yes        |
| Shell aliases                   | Yes  | Yes | Yes  | No      | No     | Yes   | No         |
| `chpwd` hook                    | Yes  | Yes | Yes  | Yes     | Yes    | Yes   | Yes        |

### Homebrew + Fish

If you installed mise via Homebrew on Fish, activation happens automatically. Disable with:
```sh
set -Ux MISE_FISH_AUTO_ACTIVATE 0
```

### After activation

Restart your shell, then verify:
```sh
mise doctor
```

## Caveats / Common Mistakes

- Only use in rc files (`.bashrc`, `.zshrc`), NOT profile files (`.profile`, `.bash_profile`, `.zprofile`). Profile files run in non-interactive contexts where the prompt hook won't fire.
- If mise isn't on PATH yet after a curl install, use the full path: `eval "$(~/.local/bin/mise activate zsh)"`.
- For non-interactive shells (CI, scripts, IDEs), use shims instead.

## See Also

- installation-curl
- cmd-activate
- concept-activation
- concept-shims
