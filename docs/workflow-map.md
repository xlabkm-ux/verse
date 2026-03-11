# Workflow Map

## Purpose

Verse MVP is a compact literary workflow for OpenAI Agent Builder. It routes the author's request into one of three paths and normalizes every result through a shared finalizer.

## Node Graph

1. `Start`
2. `Agent: Orchestrator`
3. `If/else`
4. `Agent: Generator` for `task_type == "generate"`
5. `Agent: Editor` for `task_type == "edit"`
6. `File Search -> Agent: Context Checker` for `task_type == "check_context"`
7. `Agent: Finalizer`
8. `Output`

## Routing Logic

- Route to `generate` when the user asks for a new scene, dialogue, synopsis, idea, description, or variants of fresh text.
- Route to `edit` when the user provides a draft and asks to shorten, intensify, clarify, restructure, or add subtext without replacing the core intent.
- Route to `check_context` when the user asks to verify consistency with canon, biography, world rules, chronology, facts, or previous chapters.

## Data Flow

### Input to Orchestrator

- raw author request
- optional draft text
- optional attached project files already uploaded to Builder

### Orchestrator Output

- `task_type`
- `goal`
- `genre`
- `tone`
- `constraints`
- `requires_project_context`

### Worker Outputs

- `primary_output`
- `notes`
- `issues_found`
- `recommended_next_step`

## Context Path

The `check_context` branch should always use `File Search` before the context checker when relevant materials are available.

- Search targets: character names, title, locations, timeline markers, key events
- If no files are available, the context checker must explicitly state the limitation and ask for the missing canon materials instead of inventing facts

## Finalizer Role

The finalizer converts every branch result into a clean author-facing answer.

- show the usable text first
- show short notes second
- remove routing labels and internal fields
- keep one stable response style across all branches
