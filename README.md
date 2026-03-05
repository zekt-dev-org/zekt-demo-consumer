# zekt-demo-consumer

**Ship It Demo — Consumer Repository**

Part of the [Zekt](https://zekt.io) "Ship It 🚀" live deploy demo (Spec 030).

## Purpose

This repository acts as the **Consumer** in the Zekt demo pipeline. When the Provider generates an HTML page and dispatches it through Zekt, this workflow:

1. **Receives** the Zekt event containing the generated HTML page (base64-encoded)
2. **Writes** the page to `docs/demo-{correlationId}/index.html`
3. **Commits and pushes**, triggering a GitHub Pages rebuild
4. **Verifies** the page is live and reports the URL back to the Zekt backend

The visitor on the demo page then sees the live URL and can open/share it.

## Architecture

```
zekt-demo-provider → zekt-action → Zekt Mesh → [THIS REPO] → GitHub Pages → Live URL
```

## Workflows

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `zekt-ship-it-consumer.yml` | `repository_dispatch` (Zekt event) | Deploy a demo page |
| `cleanup-old-demos.yml` | Scheduled (daily 3 AM UTC) + manual | Delete demo pages older than 24 hours |

## GitHub Pages

Pages are served from the `docs/` directory on the `main` branch.

- **Root:** `https://zekt-dev-org.github.io/zekt-demo-consumer/`
- **Demo pages:** `https://zekt-dev-org.github.io/zekt-demo-consumer/demo-{correlationId}/`

## Setup

### 1. Enable GitHub Pages

1. Go to **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, Folder: `/docs`
4. Click **Save**

### 2. Repository Variable

Set `ZEKT_DEMO_API` under **Settings → Secrets and variables → Actions → Variables**:

| Name | Value |
|------|-------|
| `ZEKT_DEMO_API` | `https://<your-zekt-backend-url>` |

### 3. Requirements

- Repository must be **public** (free GitHub Pages + free Actions)
- The `docs/` directory must exist with at least `index.html`

## Cleanup

Demo pages are automatically deleted after 24 hours by the cleanup workflow. You can also trigger cleanup manually from the **Actions** tab.

## Related

- **Provider repo:** [zekt-dev-org/zekt-demo-provider](https://github.com/zekt-dev-org/zekt-demo-provider)
- **Main platform:** [zekt-dev-org/zektMainWeb](https://github.com/zekt-dev-org/zektMainWeb)
- **Spec plan:** `specs/030-chess-ai-demo/` in `zektMainWeb`
