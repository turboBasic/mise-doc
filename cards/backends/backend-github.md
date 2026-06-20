---
id: backend-github
title: "GitHub Backend"
category: backends
tags: [backend, tool, github]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/backends/github.md
related: [concept-backends, backend-aqua, cmd-use]
---

## Summary

The GitHub backend installs tools directly from GitHub Releases. Use it when the tool isn't in the aqua registry but ships pre-built binaries on GitHub.

## Syntax / Usage

```sh
mise use github:owner/repo@version
```

## Details

Mise downloads release assets from GitHub, auto-detecting the correct binary for your platform and architecture. It looks for common patterns in asset names (linux/darwin/windows, amd64/arm64, tar.gz/zip).

### Tool options

```sh
# Specify which binary to expose
mise use "github:owner/repo[exe=binary-name]"

# Specify asset pattern
mise use "github:owner/repo[matching=*linux*amd64*]"
```

### Rate limiting

GitHub API has rate limits for unauthenticated requests (60/hour). If you see 403 errors, configure a GitHub token:

```sh
mise token github <token>
# or
export GITHUB_TOKEN=ghp_...
# or
export MISE_GITHUB_TOKEN=ghp_...
```

## Examples

```sh
# Install ripgrep from GitHub releases
mise use github:BurntSushi/ripgrep

# Install with specific binary name
mise use "github:astral-sh/ruff[exe=ruff]"

# In mise.toml
[tools]
"github:BurntSushi/ripgrep" = "latest"
"github:astral-sh/ruff" = "0.3"
```

## Caveats / Common Mistakes

- Prefer `aqua:` when the tool is in the aqua registry — it has better version metadata and verification.
- Unauthenticated GitHub API requests are rate-limited. Configure `GITHUB_TOKEN` to avoid 403 errors.
- Some tools don't follow standard naming conventions for their release assets — use `[matching=...]` to specify the pattern.

## See Also

- concept-backends
- backend-aqua
- ts-github-rate-limit
