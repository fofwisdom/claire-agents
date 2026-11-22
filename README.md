# claire-agents

> Automated mirror generated from [fofwisdom/claire-bible](https://github.com/fofwisdom/claire-bible) (`agent_clients/`). Do not edit files in this repository manually; submit changes to `fofwisdom/claire-bible`.

[한국어 안내](#antigravity) · [English](#antigravity) · [Antigravity GUI 가이드 (KO)](./docs/antigravity.ko.md) · [Antigravity GUI Guide (EN)](./docs/antigravity.en.md) · [Claude 가이드 (KO)](./docs/claude.ko.md) · [Claude Guide (EN)](./docs/claude.en.md)

Official Claire Bible agent plugins for OAuth 2.1 MCP connection, direct URL/document ingest, isolated extraction, and knowledge graph queries.

## Architecture: Standalone `claire` Server + `claire` Plugin

- Register your remote Claire Bible `/mcp` endpoint once as a standalone `claire` OAuth 2.1 MCP server so Antigravity exposes `claire/<tool>` and Claude exposes `mcp__claire__<tool>` without double prefixes.
- Install the **`claire`** plugin for your client to add the two user-facing skills (`claire-ingest` / `claire-query` on Antigravity; `/claire:ingest` / `/claire:query` on Claude) plus isolated extraction (`claire-extractor` / `claire:extractor`). Upgrading the plugin never overwrites your global server URL.

## Supported Agents

- **`claire`** (`antigravity`, `claire-antigravity-v0.4.1`): Antigravity plugin for direct URL/document ingest, isolated source extraction, and knowledge graph queries over the global claire MCP server.
- **`claire`** (`claude`, `claire-claude-v0.4.1`): Claude plugin providing /claire:ingest and /claire:query skills with isolated source extraction for your claire MCP server.

<a id="antigravity"></a>
## Antigravity (GUI-First Setup)

- **Step-by-step guides**: [한국어 가이드](./docs/antigravity.ko.md) · [English Guide](./docs/antigravity.en.md)
- **GitHub Releases**: [Download `claire-antigravity-v0.4.1.zip`](https://github.com/fofwisdom/claire-agents/releases/latest)

1. In Claire Bible **LLM provider settings** (`/account/providers`), switch extraction mode to `client`.
2. In **MCP Settings** (`/account/mcp`), copy the **Antigravity quick setup prompt** and paste it into Antigravity `+ New Conversation` (installs `~/.gemini/config/plugins/claire` and merges `mcpServers.claire` into `~/.gemini/config/mcp_config.json`).
3. Open **`Customizations -> Installed -> MCP Servers -> claire`** and finish OAuth 2.1 login (`readonly`, `write`).
4. On first run when `Allow using this MCP tool?` (`claire/<tool>`) prompts, pick **`3 Yes, and always allow`** -> **`Submit`**.
5. Use the 2 skills (`claire-ingest` for URLs, files, notes, or pending items; `claire-query` for graph search) or pick one of the 3 starter suggested prompts.

<a id="claude"></a>
## Claude (Desktop App Code Mode and Claude Code)

- **Step-by-step guides**: [한국어 안내](./docs/claude.ko.md) · [English Guide](./docs/claude.en.md)

1. In the Claude desktop app (Code mode), open **Plugins -> Add -> Add marketplace**, enter **`fofwisdom/claire-agents`**, click **Sync**, and click **Add to this computer** next to **`claire`**.
2. In Claire Bible **MCP Settings** (`/account/mcp`), copy the **Claude quick setup prompt** and paste it into a new Claude conversation. It registers your `/mcp` endpoint as the user-scope server **`claire`** and opens the OAuth 2.1 sign-in (`readonly`, `write`).
3. Start Claude again and use `/claire:ingest` (stages URLs, files, or notes and drains pending items via the isolated extractor) and `/claire:query` (searches your knowledge graph).

<details>
<summary>CLI &amp; Advanced Verification (Terminal / SHA256SUMS)</summary>

- Claude Code CLI: `claude plugin install claire --marketplace fofwisdom/claire-agents` -> `claude mcp add --transport http --scope user claire https://<your-claire-server>/mcp` -> `claude mcp login claire`.
- Antigravity CLI / checksum check: download `claire-antigravity-v0.4.1.tar.gz` and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest), run `sha256sum -c SHA256SUMS --ignore-missing`, extract into `~/.gemini/config/plugins/`, and add `mcpServers.claire` in `~/.gemini/config/mcp_config.json`.

</details>

## Server Authentication Model

This repository (`v0.4.1`) distributes static client skills, subagents, rules, and templates only. Zero server tokens or secrets are included. Every Claire Bible instance shares these bundles and authenticates dynamically over OAuth 2.1 (`PKCE S256` or `/device`).
