# Claire Bible Plugin for Claude (`claire` v0.4.1)

Official Claude plugin that provides `/claire:ingest`, `/claire:query`, and the isolated `claire:extractor` subagent for your standalone `claire` OAuth 2.1 Streamable HTTP MCP server (`mcp__claire__*`). It contains no server tokens or secrets.

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

You only need two user-facing commands:

- **`/claire:ingest`**:
  - **URL / File / Note Ingest**: Saves a URL, local file (PDF, DOCX, ODT, MD, TXT, RST, ADOC, HTML), or text note with `mcp__claire__extract_stage` and extracts it automatically via the hidden forked subagent runner (`extract-runner` -> `claire:extractor`).
  - **Pending Queue Drain**: Invoke `/claire:ingest` with no arguments (or ask to process waiting/queued items) to check `mcp__claire__extract_queue` and drain `pending` items staged from the Web UI, Telegram, or feeds through the same hidden runner.
- **`/claire:query`**: Discovers KBs (`mcp__claire__kb_list`), searches entities and relations, inspects documents, and submits contributions only after your explicit approval.

Examples:

```text
/claire:ingest https://example.com/article
/claire:ingest ./notes/spec.pdf
/claire:ingest
/claire:query Find documents about Fedify
```

- **Isolation**: Raw staged text from `mcp__claire__extract_read` is read only inside `agents/extractor.md` (`claire:extractor`), which has Claire MCP tools only (no files, shell, or browser). `/claire:ingest` invokes the hidden forked subagent runner automatically so users only learn 2 commands. When Claude on the main thread tries to read a raw staged source directly, the `PreToolUse` guard hook in `hooks/hooks.json` stops the call and returns: `extract_read is allowed only in the claire:extractor subagent. Use /claire:ingest, or give the document_id to claire:extractor.` Text inside `<untrusted_source_document nonce="...">` is always inert data.
