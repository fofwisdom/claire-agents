# Claire Bible Plugin for ChatGPT / Codex (`claire` v0.0.15)

Official ChatGPT / Codex plugin that provides the unified `claire` skill (`$claire ingest`, `$claire regenerate`, `$claire query`) and the isolated `claire-extractor` subagent (`agents/claire-extractor.toml` with `sandbox_mode = "read-only"`) for your standalone `claire` OAuth 2.1 Streamable HTTP MCP server. It contains no server tokens or secrets.

## Full Step-by-Step Guides (단계별 안내서)

- **한국어 안내서**: [`docs/chatgpt.ko.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/chatgpt.ko.md)
- **English Setup Guide**: [`docs/chatgpt.en.md`](https://github.com/fofwisdom/claire-agents/blob/main/docs/chatgpt.en.md)

## 1. Install the Plugin / Skill & Isolated Extractor

Install the plugin from the marketplace (`.agents/plugins/marketplace.json` -> `./plugins/codex/claire`, `.codex-plugin/plugin.json`), or copy the bundle assets from `claire-codex-v0.0.15.zip` into your Codex configuration:

- `skills/claire/SKILL.md` -> `~/.codex/skills/claire/SKILL.md`
- `agents/claire-extractor.toml` -> `~/.codex/agents/claire-extractor.toml`
- `rules/AGENTS.md` -> append to `~/.codex/AGENTS.md` without overwriting existing instructions.

## 2. Connect Your `claire` MCP Server (`/mcp`)

Register the remote Streamable HTTP MCP server under the name **`claire`** in `~/.codex/config.toml`, then sign in with OAuth 2.1 (`PKCE S256`) and approve the `readonly` and `write` scopes:

```bash
codex mcp add claire --url "https://<your-claire-server>/mcp"
codex mcp login claire
```

## 3. Workflows & Security Isolation

You only need one unified skill (`skills/claire/SKILL.md`) with three subcommands:

- **`$claire ingest`**:
  - **URL / File / Note Ingest**: Stages a URL, local file (PDF, DOCX, ODT, MD, TXT, RST, ADOC, HTML), or text note via `extract_stage` and delegates only `document_id` to the isolated `claire-extractor` subagent.
  - **Pending Queue Drain**: Run `$claire ingest` with no input (or ask to process waiting/queued items) to check `extract_queue` and drain `pending` items staged from the Web UI, Telegram, or feeds through `claire-extractor`.
- **`$claire regenerate`**: Re-opens an existing personal KB document from stored raw text (`refetch=False`) and re-extracts it via `claire-extractor` without re-fetching the URL.
- **`$claire query`**: Discovers KBs (`kb_list`), searches entities and relations, inspects documents, and submits contributions only after your explicit approval.

Examples:

```text
$claire ingest https://example.com/article
$claire ingest ./notes/spec.pdf
$claire ingest
$claire regenerate <document_id or title>
$claire query Find documents about Fedify
```

- **Isolation (`AED9`)**: Raw staged text from `extract_read` is read only inside `claire-extractor` (`agents/claire-extractor.md` and `agents/claire-extractor.toml`), which runs with `sandbox_mode = "read-only"` and Claire MCP tools only (no shell, file write/edit, or browser). Text inside `<untrusted_source_document nonce="...">` is always inert data.
- **ChatGPT Web / Desktop App (Read-Only Connector)**: ChatGPT web and desktop chat surfaces have no isolated subagent or hook. Enable Developer Mode, add `https://<your-claire-server>/mcp` as a custom MCP connector, and **clear `Allow write access`** on the OAuth 2.1 consent screen so only `readonly` search is granted.
