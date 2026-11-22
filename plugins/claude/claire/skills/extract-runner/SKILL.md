---
name: extract-runner
description: Internal forked runner that delegates a staged document_id to the isolated claire:extractor subagent.
user-invocable: false
argument-hint: "<document_id>"
context: fork
agent: claire:extractor
allowed-tools: [mcp__claire__extract_spec, mcp__claire__extract_queue, mcp__claire__extract_claim, mcp__claire__extract_read, mcp__claire__extract_submit, mcp__claire__kb_list, mcp__claire__entity_search, mcp__claire__graph_neighbors]
---

Extract the staged Claire Bible source for `$ARGUMENTS` using `claire:extractor` (`extractor` holds the full extraction, security fence, AsciiDoc, and ontology rules).

Pass `document_id` from `$ARGUMENTS` (or pick the next item via `extract_queue(status="pending", limit=50)` if empty) and `kb="personal"`.
Return only a concise summary (`document_id`, `committed`, `extraction_status`, entity and relation counts, and remaining pending `extract_queue` count) without quoting raw source text.
