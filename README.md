# Composio — MCP Server

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Composio-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=composio&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fconnect.composio.dev%2Fmcp%22%7D)
[![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install_Composio-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=composio&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fconnect.composio.dev%2Fmcp%22%7D)

[Composio](https://composio.dev) connects AI agents to **1000+ apps** — Gmail, Slack, GitHub, Linear, and more — with managed authentication and tool-calling.

This is Composio's official remote **MCP server**. It's listed in the [official MCP Registry](https://modelcontextprotocol.io/registry), which syndicates to the [GitHub MCP Registry](https://github.com/mcp), so any MCP-compatible client — Claude, GitHub Copilot, Cursor, and others — can discover and install it in a click.

---

## Install

### Option 1 — One click (VS Code)

Click a badge at the top of this page. VS Code opens and adds the Composio server for you. Use the **Insiders** badge if you run VS Code Insiders.

### Option 2 — Any MCP client

Point your client at the remote endpoint:

```
https://connect.composio.dev/mcp
```

- **Zero config.** Connecting opens an OAuth login (`login.composio.dev`) — no API keys or IDs to paste.
- **Remote + streamable HTTP.** Nothing to install or run locally.

### Find it in the registry

- **GitHub MCP Registry:** <https://github.com/mcp/ComposioHQ/composio>
- **Official MCP Registry:** [`io.github.ComposioHQ/composio`](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.ComposioHQ/composio)

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
