# Agent Builder Assembly Guide

## Goal

Assemble the Verse MVP workflow manually in OpenAI Agent Builder using the prompts and contracts in this repository.

## Nodes to Create

Create these nodes in order:

1. `Start`
2. `Agent: Orchestrator`
3. `If/else`
4. `Agent: Generator`
5. `Agent: Editor`
6. `File Search`
7. `Agent: Context Checker`
8. `Agent: Finalizer`
9. `Output`

## Step 1: Configure the Orchestrator

- Node name: `Orchestrator`
- System prompt source: [prompts/orchestrator.md](C:\CODEX\prompts\orchestrator.md)
- Structured output: [schemas/orchestrator-output.schema.json](C:\CODEX\schemas\orchestrator-output.schema.json)
- Input:
  - original user message
  - any pasted draft text
- Expected output fields:
  - `task_type`
  - `goal`
  - `genre`
  - `tone`
  - `constraints`
  - `requires_project_context`

## Step 2: Configure Routing

Add `If/else` conditions based on `task_type`.

- If `task_type == "generate"` -> `Generator`
- Else if `task_type == "edit"` -> `Editor`
- Else if `task_type == "check_context"` -> `File Search`

If Builder requires a final fallback branch, send the fallback to `Generator` and rely on the orchestrator prompt to minimize misclassification.

## Step 3: Configure the Generator

- Node name: `Generator`
- System prompt source: [prompts/generator.md](C:\CODEX\prompts\generator.md)
- Input fields from Orchestrator:
  - `goal`
  - `genre`
  - `tone`
  - `constraints`
  - original user message
- Structured output: [schemas/worker-output.schema.json](C:\CODEX\schemas\worker-output.schema.json)

## Step 4: Configure the Editor

- Node name: `Editor`
- System prompt source: [prompts/editor.md](C:\CODEX\prompts\editor.md)
- Input fields from Orchestrator:
  - `goal`
  - `genre`
  - `tone`
  - `constraints`
  - original user message
  - any provided draft text
- Structured output: [schemas/worker-output.schema.json](C:\CODEX\schemas\worker-output.schema.json)

## Step 5: Configure File Search

- Node name: `Project File Search`
- Use this node only in the `check_context` branch.
- Search over the files uploaded from [knowledge/](C:\CODEX\knowledge\README.md).
- Search hints to use in the query:
  - character names
  - world terms
  - title of the work
  - locations
  - timeline events

If Builder allows query composition, pass:

- the original user message
- `goal`
- any named entities extracted or mentioned by the user

## Step 6: Configure the Context Checker

- Node name: `Context Checker`
- System prompt source: [prompts/context-checker.md](C:\CODEX\prompts\context-checker.md)
- Inputs:
  - original user message
  - orchestrator fields
  - File Search results
- Structured output: [schemas/worker-output.schema.json](C:\CODEX\schemas\worker-output.schema.json)

Behavior requirement:

- If File Search returns nothing useful, state the limitation explicitly.
- Recommend which canon files the author should upload.
- Do not invent canon facts.

## Step 7: Configure the Finalizer

- Node name: `Finalizer`
- System prompt source: [prompts/finalizer.md](C:\CODEX\prompts\finalizer.md)
- Input:
  - entire worker output object
  - original user message when useful
- Output mode:
  - plain author-facing answer
  - text first, notes second

## Step 8: Upload Canon Materials Before Preview

Prepare local files in `knowledge/` and upload the relevant ones into Agent Builder before testing the `check_context` branch.

Recommended upload order:

1. `world_bible`
2. `characters`
3. `timeline`
4. `chapters`
5. `notes`

## Step 9: Run Preview

Use [tests/preview-cases.md](C:\CODEX\tests\preview-cases.md) as the Preview checklist.

Success criteria:

- every prompt routes correctly
- worker outputs match the contracts
- final answers are clean and non-technical
- canon checks do not hallucinate when files are missing
