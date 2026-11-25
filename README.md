# claire-agents

> Automated mirror generated from [fofwisdom/claire-bible](https://github.com/fofwisdom/claire-bible) (`agent_clients/`). Do not edit files in this repository manually; submit changes to `fofwisdom/claire-bible`.

[한국어 안내](#antigravity) · [English](#antigravity) · [Antigravity GUI 가이드 (KO)](./docs/antigravity.ko.md) · [Antigravity GUI Guide (EN)](./docs/antigravity.en.md) · [Claude 가이드 (KO)](./docs/claude.ko.md) · [Claude Guide (EN)](./docs/claude.en.md) · [ChatGPT (Codex) 가이드 (KO)](./docs/chatgpt.ko.md) · [ChatGPT (Codex) Guide (EN)](./docs/chatgpt.en.md)

Official Claire Bible agent plugins for OAuth 2.1 MCP connection, direct URL/document ingest, isolated extraction, and knowledge graph queries.

## Architecture: Standalone `claire` Server + `claire` Plugin

- Register your remote Claire Bible `/mcp` endpoint once as a standalone `claire` OAuth 2.1 MCP server so Antigravity exposes `claire/<tool>`, Claude exposes `mcp__claire__<tool>`, and Codex reads `mcp_servers.claire` without double prefixes.
- Install the **`claire`** plugin for your client to expose the unified `claire` + subcommand workflow (`claire ingest` / `claire regenerate` / `claire query` on Antigravity; `$claire ingest` / `$claire regenerate` / `$claire query` on Codex; `/claire:ingest` / `/claire:regenerate` / `/claire:query` on Claude) plus isolated extraction (`claire-extractor` / `claire:extractor`). Upgrading the plugin never overwrites your global server URL.

## Supported Agents

- **`claire`** (`antigravity`, `claire-antigravity-v0.0.14`): Antigravity plugin providing the unified claire skill (ingest, regenerate, query subcommands), isolated source extraction, and knowledge graph queries over the global claire MCP server.
- **`claire`** (`claude`, `claire-claude-v0.0.14`): Claude plugin providing /claire:ingest, /claire:regenerate, and /claire:query skills with isolated source extraction for your claire MCP server.
- **`claire`** (`codex`, `claire-codex-v0.0.14`): ChatGPT (Codex) plugin providing the unified claire skill (ingest, regenerate, query subcommands) with an isolated claire-extractor subagent for your claire MCP server.

<a id="antigravity"></a>
## Antigravity (GUI-First Setup)

- **Step-by-step guides**: [한국어 가이드](./docs/antigravity.ko.md) · [English Guide](./docs/antigravity.en.md)
- **GitHub Releases**: [Download `claire-antigravity-v0.0.14.zip`](https://github.com/fofwisdom/claire-agents/releases/latest)

1. In Claire Bible **LLM provider settings** (`/account/providers`), switch extraction mode to `client`.
2. In **MCP Settings** (`/account/mcp`), copy the **Antigravity quick setup prompt** and paste it into Antigravity `+ New Conversation` (installs `~/.gemini/config/plugins/claire` and merges `mcpServers.claire` into `~/.gemini/config/mcp_config.json`).
3. Open **`Customizations -> Installed -> MCP Servers -> claire`** and finish OAuth 2.1 login (`readonly`, `write`).
4. On first run when `Allow using this MCP tool?` (`claire/<tool>`) prompts, pick **`3 Yes, and always allow`** -> **`Submit`**.
5. Use the unified `claire` skill (`claire ingest`, `claire regenerate`, `claire query`) or pick one of the 3 starter suggested prompts.

<a id="claude"></a>
## Claude (Desktop App Code Mode and Claude Code)

- **Step-by-step guides**: [한국어 안내](./docs/claude.ko.md) · [English Guide](./docs/claude.en.md)

1. In the Claude desktop app (Code mode), open **Plugins -> Add -> Add marketplace**, enter **`fofwisdom/claire-agents`**, click **Sync**, and click **Add to this computer** next to **`claire`**.
2. In Claire Bible **MCP Settings** (`/account/mcp`), copy the **Claude quick setup prompt** and paste it into a new Claude conversation. It registers your `/mcp` endpoint as the user-scope server **`claire`** and opens the OAuth 2.1 sign-in (`readonly`, `write`).
3. Start Claude again and use `/claire:ingest` (stages URLs, files, or notes and drains pending items via the isolated extractor), `/claire:regenerate` (re-extracts from stored raw text), and `/claire:query` (searches your knowledge graph).

<a id="chatgpt"></a>
## ChatGPT (Codex App, Codex CLI, and ChatGPT Web/Desktop)

- **Step-by-step guides**: [한국어 안내](./docs/chatgpt.ko.md) · [English Guide](./docs/chatgpt.en.md)
- **GitHub Releases**: [Download `claire-codex-v0.0.14.zip`](https://github.com/fofwisdom/claire-agents/releases/latest)

1. **Codex app / CLI (full ingest, isolated extraction, and query)**: In Claire Bible **MCP Settings** (`/account/mcp`), copy the **ChatGPT (Codex) quick setup prompt** and paste it into a new Codex conversation (installs the unified `skills/claire` skill for `$claire ingest` / `$claire regenerate` / `$claire query`, `agents/claire-extractor.toml` with `sandbox_mode = "read-only"`, and `rules/AGENTS.md`, runs `codex mcp add claire --url ...`, and starts `codex mcp login claire`).
2. **ChatGPT web / desktop chat (read-only search)**: Enable **Developer Mode** in ChatGPT connector settings, add `https://<your-claire-server>/mcp` with OAuth, and **clear `Allow write access`** on the Claire Bible consent screen so only `readonly` is granted.

<details>
<summary>CLI &amp; Advanced Verification (Terminal / SHA256SUMS)</summary>

- Claude Code CLI: `claude plugin install claire --marketplace fofwisdom/claire-agents` -> `claude mcp add --transport http --scope user claire https://<your-claire-server>/mcp` -> `claude mcp login claire`.
- Codex CLI: download `claire-codex-v0.0.14.tar.gz` and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest), install `skills/claire`, `agents/claire-extractor.toml`, and `rules/AGENTS.md` under `~/.codex/`, then run `codex mcp add claire --url "https://<your-claire-server>/mcp"` -> `codex mcp login claire`.
- Antigravity CLI / checksum check: download `claire-antigravity-v0.0.14.tar.gz` and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest), run `sha256sum -c SHA256SUMS --ignore-missing`, extract into `~/.gemini/config/plugins/`, and add `mcpServers.claire` in `~/.gemini/config/mcp_config.json`.

</details>

## Server Authentication Model

This repository (`v0.0.14`) distributes static client skills, subagents, rules, and templates only. Zero server tokens or secrets are included. Every Claire Bible instance shares these bundles and authenticates dynamically over OAuth 2.1 (`PKCE S256` or `/device`).
