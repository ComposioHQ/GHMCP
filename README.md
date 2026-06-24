# Composio — GitHub MCP Registry Listing

This repo publishes **Composio's MCP server** to the [MCP Community Registry](https://modelcontextprotocol.io/registry), which **auto-syndicates into the [GitHub MCP Registry](https://github.com/mcp)**. Composio already serves MCP — this repo is just the *packaging* that gets it listed where GitHub Copilot users can find and install it.

---

## What's in here

| File | What it is |
|------|------------|
| `server.json` | The listing itself — name, description, and the Composio MCP endpoint Copilot connects to. |
| `.github/workflows/publish-mcp.yml` | Auto-publishes when a `v*` tag is pushed. Uses **GitHub OIDC** — no personal login, no OAuth approval. |

---

## ⚠️ Before anything: fill in the real endpoint

`server.json` has a placeholder:

```
"url": "https://TODO-REPLACE-WITH-CANONICAL-COMPOSIO-MCP-ENDPOINT"
```

Replace it with Composio's **one canonical, public, HTTPS, streamable-HTTP MCP endpoint** (the curated default toolset — not all 250+ toolkits firehosed). Nothing publishes correctly until this is real.

---

## The plan (who does what)

### Phase 1 — Dry run, under your OWN name (no admin needed) ✅
Proves the whole flow works before the admin touches anything.

1. Install the publisher CLI:
   ```bash
   brew install mcp-publisher   # or grab the binary from the registry releases page
   ```
2. Temporarily change the name in `server.json` to your personal namespace:
   ```
   "name": "io.github.<your-github-username>/composio-test"
   ```
3. Authenticate as yourself + publish:
   ```bash
   mcp-publisher login github
   mcp-publisher publish
   ```
4. Confirm it appears in the community registry, then **install it in GitHub Copilot** and run one real tool call (search → add → connect an app → call a tool). Trust the tool call, not the listing.
5. **Revert the name** back to `io.github.composio/composio`.

> If connecting an app (the auth handoff) feels rough inside Copilot — **fix that before going official.** That login experience is Composio's whole edge.

### Phase 2 — Make it official (the admin's ONE ~2-min action) 🔑
The official `io.github.composio` name can only be published by something that proves Composio-org ownership. The clean way (OIDC) means the *repo itself* is that proof:

- **Move/create this repo under the Composio GitHub org** (`github.com/composio/composio-mcp-registry`).
- That's the admin's whole job. Once it lives in the org, publishing is automatic.

### Phase 3 — Publish (anyone with push access, after Phase 2)
```bash
git tag v1.0.0
git push origin v1.0.0
```
The workflow runs, authenticates via OIDC (because the repo is in the org), and publishes. Done.

### Phase 4 — Verify syndication
- Confirm it's live in the community registry.
- Give it a beat, then confirm it shows in the **GitHub MCP Registry** ([github.com/mcp](https://github.com/mcp)).
- Install the *official* listing in Copilot and run one real tool call.

### Phase 5 — Go for "featured" (the BD lane, optional) 🌟
Marquee registry slots (Stripe/Notion-tier) are hand-curated by GitHub.
- Email **partnerships@github.com**, reference the now-live `io.github.composio/composio` server, ask for inclusion/featuring.
- Self-publishing first = you arrive with a live artifact, not a pitch.

---

## Honest caveats
- **Featured placement is GitHub's call**, not guaranteed. Phases 1–4 give you a real, installable listing regardless.
- **Registry is in preview** — schema/URLs may shift before GA.
- **The real risk is the auth-in-Copilot experience** (Phase 1, step 4). A broken first tool call in the registry is worse than a late listing.

---

## One-liner to send the admin
> "Built the MCP-registry listing for Composio — it's fully tested under my own account. Only thing I need: this repo moved under our GitHub org so it can publish under the official Composio name. ~2 min, then it self-publishes. Repo: composio-mcp-registry."
