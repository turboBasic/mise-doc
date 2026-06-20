---
id: recipe-ci
title: "Recipe: CI/CD Integration"
category: recipes
tags: [recipe, ci, github-actions, docker]
source: https://github.com/jdx/mise/blob/main/docs/continuous-integration.md
related: [concept-shims, cmd-exec, installation-curl]
---

## Summary

Patterns for using mise in CI/CD pipelines: GitHub Actions with `jdx/mise-action`, Docker containers, and generic CI setups.

## Examples

### GitHub Actions (recommended)

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: jdx/mise-action@v2
      - run: mise run test
```

### GitHub Actions with specific tools

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: jdx/mise-action@v2
    with:
      install: true    # install tools from mise.toml
  - run: node --version
  - run: python --version
  - run: mise run build
```

### Docker

```dockerfile
FROM ubuntu:24.04

# Install mise
RUN curl https://mise.run | sh
ENV PATH="/root/.local/bin:$PATH"

# Install tools
COPY mise.toml .
RUN mise install

# Use mise exec for commands
RUN mise exec -- node --version
```

### Generic CI (no action available)

```sh
# Install mise
curl https://mise.run | sh
export PATH="$HOME/.local/bin:$PATH"

# Activate via shims (best for CI)
export PATH="$HOME/.local/share/mise/shims:$PATH"

# Install tools from config
mise install

# Run tasks
mise run test
```

### Caching in CI

```yaml
# GitHub Actions with caching
steps:
  - uses: actions/checkout@v4
  - uses: jdx/mise-action@v2
    with:
      cache: true  # caches installed tools
  - run: mise run test
```

## Details

Key CI patterns:
- Use `jdx/mise-action@v2` for GitHub Actions (handles install + caching)
- Use shims in CI (not `mise activate`) since there's no interactive shell
- `mise exec` works without activation
- Cache `~/.local/share/mise/installs/` for faster CI runs

## Caveats / Common Mistakes

- Don't use `mise activate` in CI — there's no interactive prompt. Use shims or `mise exec`.
- Configure `GITHUB_TOKEN` in CI to avoid rate limiting during tool installation.
- Pin tool versions in `mise.toml` or use `mise.lock` for reproducible CI builds.

## See Also

- concept-shims
- cmd-exec
- installation-curl
- ts-github-rate-limit
