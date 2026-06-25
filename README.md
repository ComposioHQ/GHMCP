# Composio → Official MCP Registry Listing

This repo gets **Composio listed on the [official MCP Registry](https://modelcontextprotocol.io/registry)** — the catalog MCP clients (Claude, GitHub Copilot, Cursor, etc.) read to discover and install servers. It also **auto-syndicates to the [GitHub MCP Registry](https://github.com/mcp)**.

Composio already runs MCP. This repo is just the **packaging + listing** that makes it discoverable.

---

## ✅ Status: proven and live

The full publish pipeline has already been **tested end-to-end against the real Composio endpoint** and is live on the registry right now under a personal test name:

- **Live proof:** https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.pboddupalli1/composio-test
- It shows up as **"Composio"**, status `active`, pointing at the real `backend.composio.dev` MCP endpoint.

The **only** thing standing between this and the official `io.github.composio/composio` listing is one admin action (below). Everything else is done.

---

## 🔑 What we need from leadership (one action, ~2 minutes)

The official name `io.github.composio/composio` can only be published by something that proves it belongs to the **Composio GitHub org**. The clean, no-passwords way is to let the repo itself be that proof (GitHub OIDC).

> **Action:** Move (or re-create) this repo under the **`github.com/composio`** org, as `composio/composio-mcp-registry`.

That's it. Once the repo lives in the org, publishing is automatic (Step 3).

---

## 📋 Step-by-step

### Step 1 — Decide the endpoint (product owner)
Open `server.json`. It currently points at Composio's standard hosted MCP URL:

```
https://backend.composio.dev/v3/mcp/{server_id}?user_id={user_id}     (header: x-api-key)
```

This is **multi-tenant**: each user supplies their own `server_id`, `user_id`, and `x-api-key`. If we'd rather expose a **curated zero-config default toolset** under one fixed URL, replace the templated URL with that endpoint. Otherwise, leave as-is — it's functional.

### Step 2 — Move the repo into the Composio org (admin)
Transfer this repo to `github.com/composio`, or create `composio/composio-mcp-registry` and push this code to it. No other setup, secrets, or tokens required — the included workflow uses GitHub OIDC.

### Step 3 — Publish the official listing (anyone with push access)
From the repo, once it's in the org:

```bash
git tag v1.0.0
git push origin v1.0.0
```

Pushing a `v*` tag triggers `.github/workflows/publish-mcp.yml`, which authenticates via OIDC (proving Composio-org ownership) and publishes. No manual login.

### Step 4 — Verify
1. Confirm the official server is live:
   ```bash
   curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.composio/composio"
   ```
2. Give it a beat, then confirm it appears in the **GitHub MCP Registry**: https://github.com/mcp
3. Install it in a real MCP client (Claude/Copilot/Cursor) and run **one real tool call** — search a toolkit → connect an app → execute. *Trust the tool call, not just the listing.*

### Step 5 (optional) — Apply for featured placement (BD)
Marquee registry slots (Stripe/Notion-tier) are hand-curated by GitHub. Once the official listing is live, email **partnerships@github.com** referencing `io.github.composio/composio` to request inclusion/featuring. Arriving with a live, installable server beats pitching an idea.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `server.json` | The listing: name, description, and the Composio MCP endpoint clients connect to. |
| `.github/workflows/publish-mcp.yml` | Auto-publishes when a `v*` tag is pushed. Uses GitHub OIDC — no personal login, no secrets. |
| `README.md` | This document. |

---

## Honest caveats
- **Registry is in preview** — schema and URLs may shift before GA.
- **Featured placement is GitHub's call** — not guaranteed. Steps 1–4 give a real, installable listing regardless.
- **The real risk is the in-client auth/first-tool-call experience** (Step 4) — a broken first tool call in the registry is worse than a late listing. Validate it before announcing.
- The `composio-test` proof listing can be deleted or left up once the official one is live.
