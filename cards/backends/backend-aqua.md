---
id: backend-aqua
title: "Aqua Backend"
category: backends
tags: [backend, tool, aqua]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/backends/aqua.md
related: [concept-backends, backend-github, cmd-use]
---

## Summary

The aqua backend installs tools from the aqua-registry. It is the preferred backend for CLI tools — it provides SLSA verification, per-version download logic, and better UX than raw GitHub releases.

## Syntax / Usage

```sh
mise use aqua:owner/repo@version
```

## Details

The aqua registry (github.com/aquaproj/aqua-registry) maintains metadata about thousands of CLI tools: where to download them, how to extract them, and which binary to expose. Mise reads this registry directly — you don't need aqua itself installed.

Advantages over the `github:` backend:
- SLSA provenance verification (supply chain security)
- Per-version logic (different archive formats across versions)
- Consistent metadata across tools
- Largest registry of CLI tools

### Checking if a tool is in aqua

Search at https://github.com/aquaproj/aqua-registry or:
```sh
mise registry | grep <tool-name>
```

## Examples

```sh
# Install GitHub CLI via aqua
mise use aqua:cli/cli

# Install terraform
mise use aqua:hashicorp/terraform@1.7

# Install with exact version
mise use aqua:BurntSushi/ripgrep@14.1.0

# In mise.toml
[tools]
"aqua:cli/cli" = "2.40"
"aqua:hashicorp/terraform" = "1.7"
```

## Caveats / Common Mistakes

- Not all tools are in the aqua registry — fall back to `github:` for tools that aren't listed.
- The aqua registry is updated independently of mise — new tool releases may take a day or two to appear.

## See Also

- concept-backends
- backend-github
- cmd-use
