---
name: extract-runner
description: Internal forked runner that delegates a staged document_id to the isolated claire:extractor subagent.
user-invocable: false
argument-hint: "<document_id> [model=<active_model_name>]"
context: fork
agent: claire:extractor
allowed-tools: [mcp__claire__extract_spec, mcp__claire__extract_queue, mcp__claire__extract_claim, mcp__claire__extract_read, mcp__claire__extract_submit, mcp__claire__kb_list, mcp__claire__entity_search, mcp__claire__graph_neighbors]
---

Extract the staged Claire Bible source for `$ARGUMENTS` using `claire:extractor` (`extractor` holds the full extraction, security fence, AsciiDoc, and ontology rules).

Pass BOTH `document_id` from `$ARGUMENTS` (or pick the next item via `extract_queue(status="pending", limit=50)` if empty) AND any active LLM model name supplied in `$ARGUMENTS` (telling `claire:extractor` to set `payload.model` to that exact plain model name verbatim, or omit `payload.model` if none was provided), along with `kb="personal"`.
Return only a concise summary (`document_id`, `committed`, `extraction_status`, entity and relation counts, and remaining pending `extract_queue` count) without quoting raw source text.
