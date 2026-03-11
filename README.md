# Verse MVP for OpenAI Agent Builder

Verse MVP is a near-ready handoff package for assembling a Russian-language literary workflow in OpenAI Agent Builder.

## Project Goal

The first version helps an author:

- generate new literary material
- edit an existing fragment
- check a new fragment against project canon and uploaded materials

The package is documentation-first. It does not deploy or configure Agent Builder automatically; instead, it gives a complete workflow map, production-ready Russian prompts, stable data contracts, and Preview test cases.

## Project Structure

- `docs/` - workflow map, node contracts, Builder assembly guide, Preview protocol
- `prompts/` - Russian system prompts for all agents
- `schemas/` - structured output contracts between nodes
- `tests/` - curated prompts and acceptance checks for Preview
- `knowledge/` - local source of truth for canon materials before upload to Builder

## Workflow Shape

`Start -> Orchestrator -> If/else`

- `generate -> Generator -> Finalizer`
- `edit -> Editor -> Finalizer`
- `check_context -> File Search -> Context Checker -> Finalizer`

`Finalizer -> Output`

## Recommended Build Order

1. Read [docs/workflow-map.md](C:\CODEX\docs\workflow-map.md).
2. Execute the operational sequence in [docs/launch-checklist.md](C:\CODEX\docs\launch-checklist.md).
3. Create nodes in Agent Builder using [docs/agent-builder-setup.md](C:\CODEX\docs\agent-builder-setup.md).
4. Paste prompts from `prompts/`.
5. Upload relevant files from `knowledge/` into Builder.
6. Run Preview scenarios from [tests/preview-cases.md](C:\CODEX\tests\preview-cases.md).

## Default Assumptions

- Primary user language: Russian
- `check_context` is in scope for MVP
- Canon files are prepared locally first, then uploaded into Builder
- No `While`, `Human Approval`, `MCP`, or advanced shared state in v1
