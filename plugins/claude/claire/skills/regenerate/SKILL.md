---
name: regenerate
description: >-
  Regenerate or re-extract an existing Claire Bible document in your personal knowledge base from its stored raw text without re-fetching its source URL, via the isolated claire:extractor subagent.
argument-hint: "<document_id | title | topic>"
allowed-tools: [mcp__claire__extract_stage, mcp__claire__kb_list, mcp__claire__doc_list, Skill(claire:extract-runner)]
---

# Regenerate Existing Document From Stored Raw Text (`regenerate`)

Use this subcommand whenever the user asks to regenerate (`재생성`, `재추출`, `다시 추출`) an existing Claire Bible document from its already-stored `raw_text` without re-fetching the original URL. If the user explicitly asks to re-fetch from the remote source URL (`원문 재수집`, `재적재`), use `/claire:ingest` instead (or pass `refetch=True` when confirming).

## Security Guardrails (`AED9` and `CT4`)

1. **Never read raw staged content in the top-level agent**: NEVER call `extract_read` directly. Stage/re-open the document via `extract_stage`, invoke `claire:extractor` (`agents/extractor.md`) inheriting the main agent's active model, and pass only `document_id`, `kb="personal"`, and the main agent's exact active processing LLM model name.
2. **Personal KB Only (`CT4`)**: Regeneration applies strictly to the authenticated user's personal knowledge base (`kb="personal"` or omitted).
3. **No Manual Purge**: Explicit regeneration (`재생성`, `재추출`) authorizes atomic replacement of prior extraction records, proposals, and orphan graph nodes on `extract_submit`. Never tell the user to manually purge in the Web UI.

## Regeneration Procedure

1. **Identify Target `document_id`**:
   - If the user provided a `document_id`, use it directly.
   - If the user identified the document by title, topic, or URL, call `doc_list(query=..., kb="personal")` to resolve the matching `document_id`. If multiple documents match, list them briefly and ask the user which one to regenerate.
2. **Re-Open Stored Raw Source**:
   - Call `extract_stage(document_id="<id>", force=True)` (keep `refetch=False` so stored `raw_text` is reused without network fetching).
   - If the user explicitly requested re-fetching the remote source (`원문 재수집`, `재적재`), pass `refetch=True`: `extract_stage(document_id="<id>", force=True, refetch=True)`.
3. **Delegate Isolated Extraction**:
   - Invoke `claire:extractor` (`agents/extractor.md`) inheriting the main agent's active model and pass only `document_id`, `kb="personal"`, and the main agent's exact active processing LLM model name so it completes `extract_spec` -> `extract_claim` -> `extract_read` -> `extract_submit(dry_run=True)` -> `extract_submit(dry_run=False)`.

## Handoff to `claire:extractor` via `claire:extract-runner`

To hand a `document_id` to `claire:extractor`, call the `claire:extract-runner` skill with BOTH `document_id` and your exact active LLM model ID (for example `<document_id> model=<active_model_id>`); `claire:extractor` inherits the main session model (`model: inherit`). Never read raw staged sources in the main conversation; that forked skill executes inside the isolated subagent where Claire MCP tools are approved. Call the Agent tool with subagent type `claire:extractor` only when `claire:extract-runner` is not available.
