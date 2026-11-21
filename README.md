# claire-agents

> Automated mirror generated from [fofwisdom/claire-bible](https://github.com/fofwisdom/claire-bible) (`agent_clients/`). Do not edit files in this repository manually; submit changes to `fofwisdom/claire-bible`.

Official Claire Bible agent plugins for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries.

## Supported Agents

- **`claire-antigravity`** (`antigravity`, `v0.1.0`): Antigravity plugin bundle for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries.
- **`claire-claude`** (`claude`, `v0.1.0`): Claude plugin for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries against your Claire Bible server.

## Installation

### Claude (Claude Desktop / Claude Code)

1. Open Claude Marketplace settings, enter `fofwisdom/claire-agents` (or `https://github.com/fofwisdom/claire-agents.git`), and sync.
2. Or run `/plugin marketplace add fofwisdom/claire-agents` in Claude Code CLI, then install `claire-claude`.
3. Connect your Claire Bible server `/mcp` endpoint (`https://<your-claire-server>/mcp`) using OAuth 2.1 with `readonly` and `write` scopes.

### Antigravity

1. Download `claire-antigravity-v<version>.tar.gz` (or `.zip`) and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases).
2. Verify checksums, extract into `~/.gemini/config/plugins/`, and set your Claire Bible server `/mcp` URL (`https://<your-claire-server>/mcp`) in `mcp_config.json`.

## Server Authentication Model

This repository distributes static client instructions, subagents, skills, and templates only. Zero server tokens or secrets are included. Every Claire Bible instance shares these bundles and authenticates dynamically over OAuth 2.1 (`PKCE S256` or `/device`).
