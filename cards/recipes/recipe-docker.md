---
id: recipe-docker
title: "Recipe: Docker Integration"
category: recipes
tags: [recipe, docker, container]
source: https://github.com/jdx/mise/blob/main/docs/mise-cookbook/docker.md
related: [recipe-ci, installation-curl, cmd-exec]
---

## Summary

Patterns for using mise inside Docker containers for reproducible development and production builds.

## Examples

### Development Dockerfile

```dockerfile
FROM ubuntu:24.04

# Install mise
RUN curl https://mise.run | sh
ENV PATH="/root/.local/bin:/root/.local/share/mise/shims:$PATH"

# Copy config and install tools
COPY mise.toml mise.lock ./
RUN mise install

# Copy application code
COPY . .

# Run with mise context
CMD ["mise", "run", "start"]
```

### Multi-stage build (production)

```dockerfile
# Build stage — with mise for tools
FROM ubuntu:24.04 AS builder

RUN curl https://mise.run | sh
ENV PATH="/root/.local/bin:/root/.local/share/mise/shims:$PATH"

COPY mise.toml mise.lock ./
RUN mise install

COPY . .
RUN mise run build

# Production stage — no mise needed
FROM ubuntu:24.04
COPY --from=builder /app/dist /app
CMD ["/app/server"]
```

### devcontainer with mise

```json
// .devcontainer/devcontainer.json
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "postCreateCommand": "curl https://mise.run | sh && ~/.local/bin/mise install",
  "remoteEnv": {
    "PATH": "${containerEnv:HOME}/.local/bin:${containerEnv:HOME}/.local/share/mise/shims:${containerEnv:PATH}"
  }
}
```

## Details

Key patterns:
- Install mise early in Dockerfile for layer caching
- Copy `mise.toml` and `mise.lock` before application code (better cache utilization)
- Use shims PATH in containers (no interactive shell for activation)
- In multi-stage builds, mise is only needed in the build stage

## Caveats / Common Mistakes

- Don't use `mise activate` in Docker — there's no interactive shell. Use shims or `mise exec`.
- Put `COPY mise.toml mise.lock ./` before `COPY . .` for better Docker layer caching.
- If tools need compilation (cargo, python), the build layer will be large — use multi-stage builds.

## See Also

- recipe-ci
- installation-curl
- concept-shims
