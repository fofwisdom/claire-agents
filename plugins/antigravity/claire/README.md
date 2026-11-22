# Claire Bible Plugin for Antigravity (`claire` v0.4.3)

Provides isolated client extraction (`claire-extractor`), unified ingest & pending queue extraction (`claire-ingest`), and knowledge graph query (`claire-query`) skills wired to your global `claire` OAuth 2.1 MCP server (`claire-antigravity-v0.4.3.zip`). Zero tokens or server secrets are stored in this plugin.

## 1. Global `claire` MCP Server Setup & Sign In (OAuth 2.1)

This plugin works together with a standalone **`claire`** MCP entry in `~/.gemini/config/mcp_config.json`, so plugin upgrades never overwrite your server URL:

1. Use Claire **`/account/mcp`** **Antigravity quick setup prompt** (installs `claire-antigravity-v0.4.3.zip` into `~/.gemini/config/plugins/claire`), OR download `mcp_config.json` from **`/account/mcp`** and place/merge `"claire": {"url": "https://<your-claire-server>/mcp"}` inside `~/.gemini/config/mcp_config.json`.
2. In Antigravity, open **`Customizations -> Installed -> MCP Servers -> claire`** and complete the browser OAuth 2.1 login approving both `readonly` and `write` scopes.

## 2. Starter Workflows & Skills

This plugin exposes two user-facing skills:

- **Unified Ingest & Queue Extraction (`claire-ingest`)**:
  - **URL / File / Note Ingest**: Paste a URL, a local file (PDF, DOCX, ODT, MD, TXT, RST, ADOC, HTML), or a text note to save and extract it into your personal KB in one step.
  - **Pending Queue Drain**: Run `claire-ingest` with no URL, file, or note (or ask to process waiting/queued items) to drain `pending` items staged from the Web UI, Telegram, or feeds through the isolated `claire-extractor` (`agents/claire-extractor.md`) subagent.
  - Examples:
    - URL ingest: `"Save and extract https://example.com/article into Claire Bible"`
    - File or note ingest: `"Stage and extract ./notes/spec.pdf into Claire Bible"`
    - Pending queue drain: `"Process my pending Claire Bible items"`
- **Knowledge Graph Query & Contribution (`claire-query`)**:
  - Discover accessible KBs (`kb_list`), search entities and relations (`entity_search`, `graph_neighbors`), inspect documents, or contribute to shared KBs (`contrib_submit`) only after explicit confirmation.
  - Example: `"Search my Claire Bible knowledge graph for Fedify"`

## 3. First-Run Permission Prompts (`Allow using this MCP tool?`)

When Antigravity invokes `claire/<tool_name>` for the first time, it shows four choices:

1. **`1 Yes, allow this time`** — Allow once
2. **`2 Yes, and always allow in this conversation`** — Allow for this chat
3. **`3 Yes, and always allow`** — Always allow
4. **`4 No (tell the agent what to do instead)`** — Decline

- **Recommended choice**: Select **`3 Yes, and always allow`** -> **`Submit`** for extraction and query tools (`extract_stage`, `extract_spec`, `extract_queue`, `extract_claim`, `extract_read`, `entity_search`, `graph_neighbors`, `extract_submit`, `kb_list`) so multi-step extraction does not pause on every call. Keep **`1 Yes, allow this time`** for `claire/contrib_submit` if you want per-submit confirmation before sharing documents. Manage saved allowances anytime in **`Settings -> Permission Grants`**.

## 4. Full Step-by-Step Setup Guides

- **한국어 상세 가이드**: [`docs/antigravity.ko.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/antigravity.ko.md)
- **English Setup Guide**: [`docs/antigravity.en.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/antigravity.en.md)
