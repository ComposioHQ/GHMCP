# Composio → Official MCP Registry Listing

This repo gets **Composio listed on the [official MCP Registry](https://modelcontextprotocol.io/registry)** — the catalog MCP clients (Claude, GitHub Copilot, Cursor, etc.) read to discover and install servers. It also **auto-syndicates to the [GitHub MCP Registry](https://github.com/mcp)**.

Composio already runs MCP. This repo is just the **packaging + listing** that makes it discoverable.

---

## ✅ Status: ready to publish

- The repo already lives under the **`ComposioHQ`** GitHub org, so it can publish under the official `io.github.composiohq/composio` name.
- The listing points at Composio's **single canonical, zero-config MCP endpoint**: `https://connect.composio.dev/mcp`.
- Auth is **OAuth** — verified live against the endpoint (it returns the standard MCP OAuth challenge and a valid discovery document). Users just click "install" and log in; **no API keys, no IDs to paste**.

The publish pipeline has been tested end-to-end. The only thing left is tagging a release (Step 3).

---

## How it works (the model)

`server.json` declares a **remote** MCP server:

```json
"remotes": [
  { "type": "streamable-http", "url": "https://connect.composio.dev/mcp" }
]
```

There are no `headers` or `variables` on purpose. The endpoint is OAuth-protected, so the connecting client auto-discovers Composio's auth server and runs the login flow itself:

```
client → POST https://connect.composio.dev/mcp
       ← 401 WWW-Authenticate: Bearer ... resource_metadata=".../.well-known/oauth-protected-resource"
client → reads metadata → authorization_servers: ["https://login.composio.dev"]
       → runs OAuth login → retries with Bearer token → connected ✅
```

This is the best-case registry experience: one URL, click-to-install, OAuth login, done.

---

## 📋 Step-by-step to publish the official listing

### Step 1 — Confirm the listing (product owner)
Open `server.json` and confirm:
- `name`: `io.github.composiohq/composio` (must match the **ComposioHQ** org — this is what OIDC authorizes)
- `url`: `https://connect.composio.dev/mcp`
- `title` / `description` read the way marketing wants.

### Step 2 — (already done) Repo is in the Composio org
This repo is at `github.com/ComposioHQ/GHMCP`. That org membership is what proves ownership of the `io.github.composiohq` namespace — no personal logins, secrets, or tokens needed. The included workflow uses **GitHub OIDC**.

### Step 3 — Publish (anyone with push access)
```bash
git tag v1.0.0
git push origin v1.0.0
```
Pushing a `v*` tag triggers `.github/workflows/publish-mcp.yml`, which authenticates via OIDC (proving ComposioHQ-org ownership) and publishes. No manual login.

### Step 4 — Verify
1. Confirm the official server is live:
   ```bash
   curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.composiohq/composio"
   ```
2. Give it a beat, then confirm it appears in the **GitHub MCP Registry**: https://github.com/mcp
3. Install it in a real MCP client (Claude / Copilot / Cursor) and run **one real tool call** — the OAuth login → connect an app → execute. *Trust the tool call, not just the listing.*

### Step 5 (optional) — Apply for featured placement (BD)
Marquee registry slots (Stripe/Notion-tier) are hand-curated by GitHub. Once the official listing is live, email **partnerships@github.com** referencing `io.github.composiohq/composio` to request inclusion/featuring. Arriving with a live, installable server beats pitching an idea.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `server.json` | The listing: name, description, and the OAuth-protected Composio MCP endpoint clients connect to. |
| `.github/workflows/publish-mcp.yml` | Auto-publishes when a `v*` tag is pushed. Uses GitHub OIDC — no personal login, no secrets. |
| `README.md` | This document. |

---

## Honest caveats
- **Registry is in preview** — schema and URLs may shift before GA.
- **Featured placement is GitHub's call** — not guaranteed. Steps 1–4 give a real, installable listing regardless.
- **The real risk is the in-client OAuth / first-tool-call experience** (Step 4) — a broken first login in the registry is worse than a late listing. Validate it before announcing.
