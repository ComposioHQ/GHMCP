# Composio — MCP Server

This repository publishes **Composio's MCP server** to the [official MCP Registry](https://modelcontextprotocol.io/registry), which syndicates to the [GitHub MCP Registry](https://github.com/mcp). Once listed, any MCP-compatible client — Claude, GitHub Copilot, Cursor, and others — can discover and install Composio in a click.

[Composio](https://composio.dev) connects AI agents to **1000+ apps** (Gmail, Slack, GitHub, Linear, and more) with managed authentication and tool-calling.

---

## Using the Composio MCP server

Add the remote server to any MCP-compatible client:

```
https://connect.composio.dev/mcp
```

- **Zero config.** Connecting opens an OAuth login (`login.composio.dev`) — no API keys or IDs to paste.
- **Remote + streamable HTTP.** Nothing to install or run locally.

---

## What gets published

`server.json` defines the listing:

```json
{
  "name": "io.github.ComposioHQ/composio",
  "title": "Composio",
  "remotes": [
    { "type": "streamable-http", "url": "https://connect.composio.dev/mcp" }
  ]
}
```

---

## How publishing works

Publishing is automated with GitHub Actions. Creating a release (a `v*` tag) triggers `.github/workflows/publish-mcp.yml`, which:

1. Installs the `mcp-publisher` CLI
2. Authenticates to the registry with **GitHub OIDC** — no tokens or secrets
3. Runs `mcp-publisher publish`

Because the repository lives under the `ComposioHQ` org, OIDC proves ownership of the `io.github.ComposioHQ` namespace automatically.

```bash
# publish a new version
git tag v1.0.4
git push origin v1.0.4
```

Verify the result:

➡️ https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.ComposioHQ/composio

---

## Prior art

This follows the same path other vendors use to self-publish their official servers. For example, MongoDB publishes [`io.github.mongodb-js/mongodb-mcp-server`](https://github.com/mcp/mongodb-js/mongodb-mcp-server) via GitHub OIDC under its own org — no partnership required.

---

## Repository layout

| File | Purpose |
|------|---------|
| `server.json` | The registry listing (name, title, remote OAuth endpoint). |
| `.github/workflows/publish-mcp.yml` | Auto-publishes to the MCP Registry on release. GitHub OIDC, no secrets. |
