# Claude Installation & Setup Guide (English)

[한국어 가이드](./claude.ko.md) · [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest)

## Before You Start

1. In your Claire Bible **Account Settings** (`/settings/account`), switch **Extraction mode** to **`Agent Client (client)`**.
2. Keep **MCP Settings** (`/settings/mcp`) open so you can copy your `/mcp` endpoint URL.

## Install & Connect via GUI

1. Open **Marketplace management** in Claude.
2. Enter **`fofwisdom/claire-agents`** (or `https://github.com/fofwisdom/claire-agents.git`), sync the marketplace, and install **`claire-claude`**.
3. In Claude Connectors / MCP settings, add your Claire Bible `/mcp` URL (`https://<your-claire-server>/mcp`).
4. Complete the browser OAuth 2.1 flow and approve both `readonly` and `write` scopes.

## Try Your First Extraction

- `List pending Claire Bible sources`
- `Extract and save pending Claire Bible sources`

<details>
<summary>CLI / Terminal setup</summary>

```text
/plugin marketplace add fofwisdom/claire-agents
```

Then install `claire-claude` and register the MCP server:

```bash
claude mcp add --transport http claire https://<your-claire-server>/mcp
```

</details>
