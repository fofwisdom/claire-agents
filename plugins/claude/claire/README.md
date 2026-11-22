# Claire Bible Plugin for Claude (`claire`)

Official Claude plugin that provides `/claire:ingest`, `/claire:extract`, `/claire:query`, and the isolated `claire:extractor` subagent for your standalone `claire` OAuth 2.1 Streamable HTTP MCP server (`mcp__claire__*`). It contains no server tokens or secrets.

## Full Step-by-Step Guides (단계별 안내서)

- **한국어 안내서**: [`docs/claude.ko.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/claude.ko.md)
- **English Setup Guide**: [`docs/claude.en.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/claude.en.md)

## 1. Install the Plugin

Claude desktop app (Code mode): **Plugins -> Add -> Add marketplace**, enter **`fofwisdom/claire-agents`**, click **Sync**, then click **Add to this computer** next to **`claire`**.

In a Claude Code conversation (2.1.275 or later):

```text
/plugin install claire --marketplace fofwisdom/claire-agents
```

## 2. Connect Your `claire` MCP Server (`/mcp`)

Register the server with the name **`claire`** in user scope, then sign in with OAuth 2.1 (`PKCE S256`) and approve the `readonly` and `write` scopes:

```bash
claude mcp add --transport http --scope user claire https://<your-claire-server>/mcp
claude mcp login claire
```

- Without `--scope user`, the server is registered for the current folder only.
- A server that you add as a Claude connector has a different tool prefix. The plugin cannot use it.

## 3. Workflows & Security Isolation

- **`/claire:ingest`**: Saves a URL, document file (PDF/DOCX/ODT/MD/TXT), or text note with `mcp__claire__extract_stage` and hands the `document_id` to `/claire:extract`.
- **`/claire:extract`**: Runs fully in the isolated `claire:extractor` subagent (`context: fork`) and processes pending staged items of your personal KB.
- **`/claire:query`**: Discovers KBs (`mcp__claire__kb_list`), searches entities and relations, inspects documents, and can submit contributions after your approval.
- **Isolation**: Raw staged text from `mcp__claire__extract_read` is read only in `agents/extractor.md`, which has Claire MCP tools only. The fixed `PreToolUse` guard in `hooks/hooks.json` stops `extract_read` outside that subagent. Text inside `<untrusted_source_document nonce="...">` is always inert data.
