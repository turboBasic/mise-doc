---
id: installation-curl
title: "Installing mise"
category: installation
tags: [installation, setup]
source: https://github.com/jdx/mise/blob/main/docs/installing-mise.md
related: [installation-activation, cmd-self-update, cmd-doctor]
---

## Summary

mise can be installed via curl, package managers (brew, apt, dnf, snap, scoop, winget, chocolatey), or built from source. The curl installer is the quickest cross-platform method.

## Syntax / Usage

```sh
curl https://mise.run | sh
```

## Details

### Installation methods

| Method       | Command                              | Platform         |
| ------------ | ------------------------------------ | ---------------- |
| curl         | `curl https://mise.run \| sh`        | Linux/macOS      |
| Homebrew     | `brew install mise`                  | macOS/Linux      |
| apt          | `sudo apt install mise` (via extrepo)| Debian/Ubuntu    |
| dnf          | `sudo dnf install mise` (via copr)   | Fedora/RHEL      |
| Snap         | `sudo snap install mise --classic`   | Linux            |
| Scoop        | `scoop install mise`                 | Windows          |
| winget       | `winget install jdx.mise`            | Windows          |
| Chocolatey   | `choco install mise`                 | Windows          |
| Cargo        | `cargo install mise`                 | Any (from source)|

### Default installation directory

The curl installer places the binary at `~/.local/bin/mise`. This directory doesn't need to be on PATH — mise adds its own directory when activated.

### Verifying installation

```sh
~/.local/bin/mise --version
mise doctor
```

### Updating

```sh
mise self-update
```

## Examples

```sh
# Install on macOS/Linux
curl https://mise.run | sh

# Verify
~/.local/bin/mise --version

# Activate in zsh
echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc

# Check everything is working
mise doctor
```

## See Also

- installation-activation
- cmd-self-update
- cmd-doctor
