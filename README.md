# Realot ⇄ AI bridge (relay)

The relay is the small free Cloudflare service that links any AI app that supports MCP connectors to an open Realot editor.

## You (the engine's builder): set it up ONCE for everybody
1. Put the whole engine, including this `relay` folder, in your GitHub repo.
2. Open your engine → any project → press **AI** → **Create relay on Cloudflare** → sign in → **Deploy**.
3. Copy the address Cloudflare shows (ends in `.workers.dev`).
4. Open `project/editor/bridge/bridge-config.js`, paste it as `DEFAULT_RELAY_URL`, and redeploy your site.
5. (Recommended) In Cloudflare → your worker → Settings → Variables, set `ALLOWED_ORIGINS` to your site, e.g. `https://yourname.github.io`, so only your engine can use the relay.

## Your users: nothing to install or deploy
They open a project → press **AI** → **Connect** → **Copy link** → in your AI app: *Settings → Connectors (or MCP servers) → Add custom connector* → paste → Add. Each user gets their own private link; nobody can see anybody else's project.

## Safety
* Nothing connects until the user presses **Connect**; the toolbar dot shows the state.
* The link contains a private key — anyone with it can edit that user's open project. **Make a new private link** replaces it.
* **Read-only** lets the AI look without changing anything. Every change can be undone with Ctrl+Z.
* Limits per user: 90 tool calls per minute and 256 KiB per request. Asset transfers use sequential chunks and are limited to 12 MiB per file.

## Cost
The relay runs on Cloudflare's **free plan**: it uses a SQLite-backed Durable Object (required on free) with WebSocket hibernation, so an open-but-idle editor costs almost nothing. Free limits are shared across all your users. If the engine grows, the paid Workers plan is about $5/month.

## What your AI can do

* **Scenes and scripts:** inspect and edit entities, components, scripts, play mode, console, and screenshots.
* **Assets:** list, rename, and delete sprites, animation frames, and audio; import image/audio files through a chunked upload.
* **Animation:** open the animation editor, create/update/delete clips, import image sequences, GIFs, sprite sheets, and image ZIPs, then reorder or delete frames.
* **Tilemaps and tilesets:** import a custom 4×4 tileset sheet or individual tile image, edit tileset roles and dimensions, and paint/erase map cells.
* **Navigation and physics:** paint walkable/blocked navigation cells and areas, edit navigation bounds/costs, rename navigation/physics layers, and set Collider2D layer masks.
* **Undo, redo, and save:** project changes use the editor's history where supported.

## For developers
`mcp-core.js` (MCP protocol, shared) · `worker.js` (Cloudflare Worker + Durable Object) ·
`relay-node.mjs` (same relay for local testing: `npm install && npm run local`, address `http://localhost:8787`).
