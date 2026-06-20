---
id: cmd-mcp
title: "mise mcp"
category: commands
tags: [command, mcp, ai, experimental]
source: https://github.com/jdx/mise/blob/main/docs/mcp.md
related: [cmd-run, cmd-ls, config-settings]
---

## Summary

Starts a Model Context Protocol (MCP) server that exposes mise functionality to AI assistants. Allows AI tools to query tools, tasks, env vars, config, and execute tasks.

## Syntax / Usage

```sh
MISE_EXPERIMENTAL=1 mise mcp
```

## Details

The MCP server communicates over stdin/stdout using JSON-RPC 2.0. It provides read-only resources and a task execution tool.

### Resources

| Resource         | Description                               |
| ---------------- | ----------------------------------------- |
| `mise://tools`   | Installed tools and versions              |
| `mise://tasks`   | Available tasks with configs              |
| `mise://env`     | Environment variables from mise config    |
| `mise://config`  | Active config files and project root      |

### Tools

| Tool            | Description                    |
| --------------- | ------------------------------ |
| `run_task`      | Execute a mise task with args  |
| `install_tool`  | Install a tool (not yet impl)  |

### Integration with Claude Code

Add to `~/.claude/.mcp.json`:

```json
{
  "mcpServers": {
    "mise": {
      "command": "mise",
      "args": ["mcp"],
      "env": {
        "MISE_EXPERIMENTAL": "1"
      }
    }
  }
}
```

### Integration with Claude Desktop

Add to claude_desktop_config.json:

```json
{
  "mcpServers": {
    "mise": {
      "command": "mise",
      "args": ["mcp"],
      "env": {
        "MISE_EXPERIMENTAL": "1"
      }
    }
  }
}
```

## Examples

```sh
# Start MCP server manually (for testing)
MISE_EXPERIMENTAL=1 mise mcp

# Test with a JSON-RPC initialize message
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}' | mise mcp
```

## Caveats / Common Mistakes

- Requires `MISE_EXPERIMENTAL=1` — the feature is experimental.
- The `install_tool` tool is not yet implemented.
- The server reads config from the working directory where it's launched.

## See Also

- cmd-run
- cmd-ls
- config-settings
