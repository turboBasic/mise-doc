---
id: backend-cargo
title: "Cargo Backend"
category: backends
tags: [backend, tool, cargo, rust]
source: https://github.com/jdx/mise/blob/main/docs/dev-tools/backends/cargo.md
related: [concept-backends, backend-github, cmd-use]
---

## Summary

The cargo backend installs Rust tools by compiling them from source via `cargo install`. Requires Rust/Cargo on PATH.

## Syntax / Usage

```sh
mise use cargo:crate-name@version
```

## Details

Compiles the specified crate from crates.io using `cargo install`. This is slow (compilation takes time) but gives you the latest version of any Rust tool. For tools that ship pre-built binaries on GitHub, prefer the `github:` or `aqua:` backend instead.

## Examples

```sh
# Install a Rust tool
mise use cargo:ripgrep

# In mise.toml
[tools]
"cargo:bat" = "latest"
"cargo:fd-find" = "9"
```

## Caveats / Common Mistakes

- Compilation from source is slow — can take several minutes for large crates.
- Requires Rust toolchain on PATH. If Rust is also mise-managed, declare it before cargo tools.
- Most popular Rust CLI tools have pre-built binaries available via `aqua:` or `github:` — these install in seconds instead of minutes.

## See Also

- concept-backends
- backend-github
- backend-aqua
