# Realot Bridge

The small free Cloudflare service that links any AI app that supports MCP connectors to an open
Realot Studio editor. It only forwards messages: the project itself stays in the user's browser.

## Run your own relay (no time limit)

1. Click the button, sign in to Cloudflare (free plan is enough) and follow the steps.

   [![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/Realot-Studio/realot-bridge)

2. When it finishes, copy your relay address. It looks like `https://realot-bridge.<your-name>.workers.dev`.
3. In Realot Studio open a project, press **AI**, paste the address into the relay box, and press **Connect**.
4. Copy the link the panel shows into your AI app (*Settings -> Connectors / MCP servers -> Add custom connector*).

Prefer the command line? Clone this repo and run:

```
npm install
npx wrangler deploy
```

## Files

| File | What it does |
| --- | --- |
| `worker.js` | The Cloudflare Worker and its Durable Object (one room per user) |
| `mcp-core.js` | The MCP protocol and the instructions the AI receives when it connects |
| `tools.json` | The list of tools the AI can call |
| `relay-node.mjs` | The same relay for local testing: `npm install && npm run local` (http://localhost:8787) |
| `wrangler.toml` | Cloudflare settings (Durable Object binding and migration) |
| `mcp-bridge/` | Optional local bridge for Claude Desktop, see `mcp-bridge/README.md` |

## Settings

* `ALLOWED_ORIGINS` in `wrangler.toml`: leave empty to let any Realot editor connect, or set it to
  your own site (for example `https://yourname.github.io`) to lock the relay to that site.

## Safety

* Nothing connects until the user presses **Connect**.
* The link contains a private key. Anyone with it can edit that user's open project; **Make a new private link** replaces it.
* **Read-only** lets the AI look without changing anything. Every change can be undone with Ctrl+Z.
* Limits per user: 90 tool calls per minute and 256 KiB per request; asset files up to 20 MiB.

## Cost

Runs on Cloudflare's free plan using a SQLite-backed Durable Object with WebSocket hibernation, so an
open but idle editor costs almost nothing.

## License

MIT, see `LICENSE`. This covers only this repository, not the Realot Studio engine.
