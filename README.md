# Composio MCP Server

Connect AI agents to **1000+ apps** — Gmail, Slack, GitHub, Linear, and more — with managed authentication and tool-calling.

Composio is a remote, streamable-HTTP MCP server. Connecting opens an OAuth login (`login.composio.dev`) — no API keys to paste, nothing to run locally.

```
https://connect.composio.dev/mcp
```

## Install

**VS Code**

```bash
code --add-mcp "{\"name\":\"composio\",\"type\":\"http\",\"url\":\"https://connect.composio.dev/mcp\"}"
```

**Any other MCP client** — point it at the remote server `https://connect.composio.dev/mcp`.
