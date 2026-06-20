---
id: ts-github-rate-limit
title: "GitHub API rate limiting"
category: troubleshooting
tags: [troubleshooting, github, token, installation]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/github-tokens.md
related: [backend-github, backend-aqua, cmd-install]
---

## Summary

Many mise backends (github, aqua) use the GitHub API. Unauthenticated requests are limited to 60/hour. If you see 403/429 errors during tool installation, configure a GitHub token.

## Symptom

Tool installation fails with HTTP 403 or 429 errors. Error messages mention rate limiting or "API rate limit exceeded".

## Cause

GitHub's unauthenticated API rate limit is 60 requests per hour per IP. Installing multiple tools or running in CI can easily exceed this.

## Fix / Workaround

Configure a GitHub token (any method works):

```sh
# Method 1: mise token command
mise token github <your-token>

# Method 2: environment variable
export GITHUB_TOKEN=ghp_...

# Method 3: mise-specific env var
export MISE_GITHUB_TOKEN=ghp_...

# Method 4: GitHub CLI (if gh is installed)
# mise will automatically use gh's token
```

### Creating a token

A classic personal access token with no scopes (public repos only) is sufficient. Go to: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token.

No scopes/permissions needed for public repository access.

## Examples

```sh
# Store token permanently
mise token github ghp_xxxxxxxxxxxx

# Or in shell profile
echo 'export GITHUB_TOKEN=ghp_xxxxxxxxxxxx' >> ~/.zshrc

# Verify it works
MISE_DEBUG=1 mise install node@20
```

## See Also

- backend-github
- backend-aqua
- cmd-install
