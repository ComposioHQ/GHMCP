# Composio — MCP Server

[Composio](https://composio.dev) connects AI agents to **1000+ apps** — Gmail, Slack, GitHub, Linear, and more — with managed authentication and tool-calling.

This is Composio's official remote **MCP server**, listed in the [official MCP Registry](https://modelcontextprotocol.io/registry) and the [GitHub MCP Registry](https://github.com/mcp/ComposioHQ/composio).

## The endpoint

Every setup below points one MCP client at this single remote URL:

```
https://connect.composio.dev/mcp
```

- **Remote + streamable HTTP** — nothing to install or run locally.
- **Zero-config auth** — the first connection opens an OAuth login (`login.composio.dev`); no API keys or IDs to paste.

## Add it manually

No install button required — register the endpoint by hand in any MCP client.

### VS Code

From the terminal:

```bash
code --add-mcp "{\"name\":\"composio\",\"type\":\"http\",\"url\":\"https://connect.composio.dev/mcp\"}"
```

Or edit `mcp.json` (Command Palette → **MCP: Open User Configuration**, or `.vscode/mcp.json` for a single workspace):

```json
{
  "servers": {
    "composio": {
      "type": "http",
      "url": "https://connect.composio.dev/mcp"
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http composio https://connect.composio.dev/mcp
```

### Cursor

Add to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (per project):

```json
{
  "mcpServers": {
    "composio": {
      "url": "https://connect.composio.dev/mcp"
    }
  }
}
```

### Any other MCP client

Register a **remote / streamable-HTTP** server pointing at `https://connect.composio.dev/mcp`, then complete the OAuth login in the browser on first use.

## Verify the connection

Once added, your client lists Composio's tools:

- **VS Code** — run **MCP: List Servers**, or open `mcp.json` and click the inline **Start**.
- **Claude Code** — run `claude mcp list` and look for `composio`.

---

## For maintainers — how this listing is published

<details>
<summary>Publishing details (<code>server.json</code> + GitHub Actions)</summary>

### What gets published

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

The registry card — including the description and the auto-generated **Install in VS Code** button — is derived from these fields. Edit `server.json`, bump `version`, and re-publish.

### How publishing works

Publishing is automated with GitHub Actions. Creating a release (a `v*` tag) triggers [`.github/workflows/publish-mcp.yml`](.github/workflows/publish-mcp.yml), which:

1. Installs the `mcp-publisher` CLI
2. Authenticates to the registry with **GitHub OIDC** — no tokens or secrets
3. Runs `mcp-publisher publish`

Because the repository lives under the `ComposioHQ` org, OIDC proves ownership of the `io.github.ComposioHQ` namespace automatically.

```bash
# publish a new version — the tag must be higher than the currently published version
git tag v1.1.0
git push origin v1.1.0
```

Verify the result:

➡️ <https://registry.modelcontextprotocol.io/v0/servers?search=io.github.ComposioHQ/composio>

### Prior art

This follows the same path other vendors use to self-publish their official servers. For example, MongoDB publishes [`io.github.mongodb-js/mongodb-mcp-server`](https://github.com/mcp/mongodb-js/mongodb-mcp-server) via GitHub OIDC under its own org — no partnership required.

### Repository layout

| File | Purpose |
|------|---------|
| `server.json` | The registry listing (name, title, remote OAuth endpoint). |
| `.github/workflows/publish-mcp.yml` | Auto-publishes to the MCP Registry on release. GitHub OIDC, no secrets. |

</details>
