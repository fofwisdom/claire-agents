# claire-agents

> Automated mirror generated from [fofwisdom/claire-bible](https://github.com/fofwisdom/claire-bible) (`agent_clients/`). Do not edit files in this repository manually; submit changes to `fofwisdom/claire-bible`.

[한국어 안내](#antigravity) · [English](#antigravity) · [Antigravity GUI 가이드 (KO)](./docs/antigravity.ko.md) · [Antigravity GUI Guide (EN)](./docs/antigravity.en.md) · [Claude 가이드 (KO)](./docs/claude.ko.md) · [Claude Guide (EN)](./docs/claude.en.md)

Official Claire Bible agent plugins for OAuth 2.1 MCP connection, direct URL/document ingest, isolated extraction, and knowledge graph queries.

## Architecture: Standalone `claire` Server + `claire` Plugin

- Register your remote Claire Bible `/mcp` endpoint once as a standalone `claire` OAuth 2.1 MCP server so Antigravity exposes `claire/<tool>` and Claude exposes `mcp__claire__<tool>` without double prefixes.
- Install the **`claire`** plugin for your client to add direct URL/file ingest, isolated extraction (`claire-extractor` / `extractor`), and knowledge graph query skills. Upgrading the plugin never overwrites your global server URL.

## Supported Agents

- **`claire`** (`antigravity`, `claire-antigravity-v0.2.0`): Antigravity plugin for direct URL/document ingest, isolated source extraction, and knowledge graph queries over the global claire MCP server.
- **`claire`** (`claude`, `claire-claude-v0.2.0`): Claude plugin providing /claire:ingest, /claire:extract, /claire:query skills and isolated source extraction for your claire MCP server.

<a id="antigravity"></a>
## Antigravity (GUI-First Setup)

- **Step-by-step guides**: [한국어 가이드](./docs/antigravity.ko.md) · [English Guide](./docs/antigravity.en.md)
- **GitHub Releases**: [Download `claire-antigravity-v0.2.0.zip`](https://github.com/fofwisdom/claire-agents/releases/latest)

1. In Claire Bible **Account Settings** (`/settings/account`), switch extraction mode to `client`.
2. In **MCP Settings** (`/settings/mcp`), copy the **Antigravity quick setup prompt** and paste it into Antigravity `+ New Conversation` (installs `~/.gemini/config/plugins/claire` and merges `mcpServers.claire` into `~/.gemini/config/mcp_config.json`).
3. Open **`Customizations -> Installed -> MCP Servers -> claire`** and finish OAuth 2.1 login (`readonly`, `write`).
4. On first run when `Allow using this MCP tool?` (`claire/<tool>`) prompts, pick **`3 Yes, and always allow`** -> **`Submit`**.

<a id="claude"></a>
## Claude (GUI Marketplace Setup)

- **Step-by-step guides**: [한국어 안내](./docs/claude.ko.md) · [English Guide](./docs/claude.en.md)

1. Enter **`fofwisdom/claire-agents`** in Claude **Marketplace management**, sync, and install **`claire`**.
2. Add your Claire Bible `/mcp` connector named **`claire`** (`https://<your-claire-server>/mcp`) and sign in via OAuth 2.1 (`readonly`, `write`).
3. Use `/claire:ingest`, `/claire:extract`, and `/claire:query`.

<details>
<summary>CLI &amp; Advanced Verification (Terminal / SHA256SUMS)</summary>

- Claude CLI: `/plugin marketplace add fofwisdom/claire-agents` -> install `claire` -> `claude mcp add --transport http claire https://<your-claire-server>/mcp`.
- Antigravity CLI / checksum check: download `claire-antigravity-v0.2.0.tar.gz` and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest), run `sha256sum -c SHA256SUMS --ignore-missing`, extract into `~/.gemini/config/plugins/`, and add `mcpServers.claire` in `~/.gemini/config/mcp_config.json`.

</details>

## Server Authentication Model

This repository (`v0.2.0`) distributes static client skills, subagents, rules, and templates only. Zero server tokens or secrets are included. Every Claire Bible instance shares these bundles and authenticates dynamically over OAuth 2.1 (`PKCE S256` or `/device`).
