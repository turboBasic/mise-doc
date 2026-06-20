---
name: mise
description: Look up mise documentation — commands, concepts, configuration, backends, tasks, environments, hooks, installation, troubleshooting, recipes
allowed-tools: Read, Bash(grep *), Bash(find *), Bash(wc *)
---

You are a knowledge base lookup skill for **mise** (jdx/mise), the polyglot development environment tool that manages dev tools, environment variables, and tasks.

## Steps

The knowledge base lives at `~/00-projects/personal/turboBasic/mise-doc`.

1. Based on the user's question, identify which category directory is relevant (see topic routing below).

2. List files in the relevant directory to find matching cards:
   `find ~/00-projects/personal/turboBasic/mise-doc/cards/<category>/ -name "*.md" | sort`

3. If the right card isn't obvious from the filename, grep for the relevant keyword:
   `grep -rln "<keyword>" ~/00-projects/personal/turboBasic/mise-doc/cards/`

4. Read the relevant card(s).

5. Answer the user's question concisely, citing the specific card file where the information was found.

## Topic routing

| Topic                                              | Directory                  |
| -------------------------------------------------- | -------------------------- |
| CLI commands (use, exec, run, install, ls, etc.)   | `cards/commands/`          |
| Architecture / activation / shims / trust          | `cards/concepts/`          |
| Config files, settings, env vars config            | `cards/configuration/`     |
| Tool backends (aqua, github, npm, cargo, etc.)     | `cards/backends/`          |
| Task runner (toml tasks, file tasks, running)      | `cards/tasks/`             |
| Environment variables, secrets, direnv             | `cards/environments/`      |
| Lifecycle hooks (enter, leave, cd, preinstall)     | `cards/hooks/`             |
| Install / bootstrap / update / self-update         | `cards/installation/`      |
| Errors, debugging, wrong versions, slow prompts    | `cards/troubleshooting/`   |
| Cookbook patterns (python, node, docker, CI, etc.)  | `cards/recipes/`           |

## Answering strategy

- If the question is about a specific command, check `cards/commands/` first.
- If the question is "how do I..." or a workflow question, check `cards/recipes/` first, then `cards/tasks/`.
- If the question involves an error or something not working, check `cards/troubleshooting/` first.
- For questions about which backend to use or how tools are installed, check `cards/backends/`.
- For questions about mise.toml structure or settings, check `cards/configuration/`.
- When multiple cards are relevant, synthesize an answer from all of them.

## Important context about mise

- mise (pronounced "meez") manages: dev tools, env vars, and tasks
- Config file is `mise.toml` (TOML format)
- Activation: `eval "$(mise activate bash)"` in shell rc file
- Alternative: shims at `~/.local/share/mise/shims/`
- Tools come from backends: core, aqua, github, npm, pipx, cargo, go, etc.
- Tasks can be defined in mise.toml or as standalone script files
- The MCP server (`mise mcp`) exposes tools, tasks, env, and config to AI assistants
