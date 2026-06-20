# AI Instructions — mise-doc

## Project Overview

This repository is a Knowledge Base for AI harnesses about **mise** (jdx/mise), the
polyglot development environment tool. The primary deliverable is a collection of
Knowledge Cards covering every aspect of mise: commands, concepts, configuration,
backends, tasks, environments, hooks, installation, troubleshooting, and recipes.

The Knowledge Cards are designed for RAG/vector-search and direct context injection
into AI assistants (Claude Code, Copilot, Cursor, etc.).

---

## Tech Stack

- **Cards:** YAML front-matter + Markdown body (one `.md` file per card)
- **Consumption:** Designed for both RAG/vector-search and direct context injection
- **Tooling:** AI agents generating cards from source materials listed below

---

## Source Materials

All card content must be derived exclusively from these authoritative sources:

| Source           | URL                                              | Notes                                   |
| ---------------- | ------------------------------------------------ | --------------------------------------- |
| Source code      | https://github.com/jdx/mise                     | Ground truth for all behavior           |
| Documentation    | https://github.com/jdx/mise/tree/main/docs      | Primary reference                       |
| llms.txt         | https://github.com/jdx/mise/blob/main/llms.txt  | AI-friendly comprehensive docs          |
| CLI reference    | https://github.com/jdx/mise/tree/main/docs/cli  | Per-command documentation               |
| Issues           | https://github.com/jdx/mise/issues              | Bug patterns, edge cases, workarounds   |
| Discussions      | https://github.com/jdx/mise/discussions          | Community patterns, Q&A                 |

Do not invent behavior. If a claim cannot be traced to one of the above sources, omit it.

---

## Project Structure

```text
mise-doc/
├── CLAUDE.md                    <- entry point, references this file
├── docs/
│   └── ai-instructions.md       <- this file — single source of truth for all AI tools
├── cards/                       <- all Knowledge Cards live here
│   ├── commands/                <- CLI commands (use, exec, run, install, etc.)
│   ├── concepts/                <- architecture and concepts (activation, shims, trust, etc.)
│   ├── configuration/           <- config files, settings, environments
│   ├── backends/                <- tool backends (aqua, github, npm, cargo, etc.)
│   ├── tasks/                   <- task runner (toml tasks, file tasks, running, etc.)
│   ├── environments/            <- env vars, secrets, direnv
│   ├── hooks/                   <- lifecycle hooks
│   ├── installation/            <- install, bootstrap, update, uninstall flows
│   ├── troubleshooting/         <- patterns from issues and debugging
│   └── recipes/                 <- cookbook patterns (python, node, docker, etc.)
└── .claude/
    └── skills/
        └── mise/
            └── SKILL.md         <- Claude Code skill for knowledge lookup
```

---

## Knowledge Card Schema

Every card is a single `.md` file with this exact YAML front-matter:

```yaml
---
id: <kebab-case-unique-identifier>
title: <human-readable title>
category: <commands | concepts | configuration | backends | tasks | environments | hooks | installation | troubleshooting | recipes>
tags: [<tag1>, <tag2>, ...]
source: <URL of the primary source this card is derived from>
related: [<id-of-related-card>, ...]
---
```

Followed by a Markdown body with this structure:

```markdown
## Summary

One or two sentences: what this is and why it matters.

## Syntax / Usage

(omit for concept-only cards)
Code block showing the canonical form.

## Details

Prose explanation. Cover behavior, defaults, constraints, interactions.
Keep to what is true and sourced — no speculation.

## Examples

Concrete, copy-pasteable mise invocations or config snippets. At least one example per card.

## Caveats / Common Mistakes

(omit if none documented in sources)
Known footguns, version-specific behavior, order-sensitivity, etc.

## See Also

(omit if no related cards yet)
Bulleted list of related card ids.
```

---

## Card Generation Rules

1. **One concept per card.** If a topic requires 300+ words of body text, split it.
2. **Narrow scope.** Each command, each backend, each setting gets its own card.
3. **Source every claim.** The `source:` field in front-matter must point to where the
   information was found.
4. **No duplication.** Before creating a card, check if an existing card already covers
   the concept. Prefer adding a `See Also` link.
5. **Tags must be consistent.** Use existing tags before inventing new ones. Core tag
   vocabulary: `command`, `tool`, `backend`, `task`, `env`, `config`, `hook`,
   `activation`, `shim`, `installation`, `troubleshooting`, `recipe`, `ci`,
   `security`, `performance`, `plugin`.
6. **IDs are permanent.** Once a card is committed, its `id` must not change.
7. **File naming:** `<id>.md` — exactly matches the front-matter `id` field.

---

## AI Behaviour Guidelines

- Read the source materials before generating cards. Do not rely on training-data
  knowledge of mise — the project evolves rapidly.
- Prefer concrete shell examples in Examples over prose descriptions of usage.
- The `related` field should link cards that a user would naturally navigate between.
- When unsure whether something belongs in `concepts/` vs `configuration/`, ask: is it
  about what mise does (concept) or how you configure it (configuration)?
