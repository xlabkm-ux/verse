# Verse Launch Checklist

## Purpose

This checklist is the operational sequence for turning the repository into a working MVP inside OpenAI Agent Builder.

## Step 1: Prepare Local Canon Files

Use the files in `knowledge/` as the local source of truth.

Minimum upload set for the first Preview cycle:

- `knowledge/world_bible/world_magic_rules.md`
- `knowledge/characters/character_anna_petrovna.md`
- `knowledge/characters/character_ilya_petrov.md`
- `knowledge/timeline/timeline_book1.md`
- `knowledge/chapters/chapter_03_funeral_summary.md`

Exit criteria:

- core world rules exist in one file
- each major character has one profile
- timeline contains dated or ordered events
- at least one prior chapter summary is available for consistency checks

## Step 2: Create Workflow Nodes in Agent Builder

Create nodes in this exact order:

1. `Start`
2. `Orchestrator`
3. `If/else`
4. `Generator`
5. `Editor`
6. `Project File Search`
7. `Context Checker`
8. `Finalizer`
9. `Output`

Exit criteria:

- all nodes exist
- names match the documentation
- each branch reaches `Finalizer`

## Step 3: Configure Prompts and Schemas

Apply:

- `prompts/orchestrator.md` + `schemas/orchestrator-output.schema.json`
- `prompts/generator.md` + `schemas/worker-output.schema.json`
- `prompts/editor.md` + `schemas/worker-output.schema.json`
- `prompts/context-checker.md` + `schemas/worker-output.schema.json`
- `prompts/finalizer.md`

Exit criteria:

- each agent has the correct system prompt
- the Orchestrator and worker agents use the right structured output schema

## Step 4: Wire Branch Conditions

Routing conditions:

- `task_type == "generate"` -> `Generator`
- `task_type == "edit"` -> `Editor`
- `task_type == "check_context"` -> `Project File Search`

Context branch:

- `Project File Search` -> `Context Checker` -> `Finalizer`

Exit criteria:

- each route is reachable
- no route bypasses `Finalizer`

## Step 5: Upload Canon Files

Upload the minimum file set from Step 1 into Builder knowledge/files.

Exit criteria:

- uploaded files are visible in Builder
- file names are stable and readable
- the context branch can search them

## Step 6: Run Preview Cases

Execute the cases from `tests/preview-cases.md` in this order:

1. routing
2. generate
3. edit
4. context
5. finalizer

Exit criteria:

- correct route for each prompt
- final answer contains no technical field labels
- context checks cite file-backed facts or clearly state limitations

## Step 7: Fix the First Failure

Prioritize in this order:

1. routing failures
2. context hallucinations
3. weak generator specificity
4. editor over-rewriting
5. finalizer formatting noise

## Step 8: Publish v1

Publish only when all core Preview cases pass and missing-file behavior is safe.
