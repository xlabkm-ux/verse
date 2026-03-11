# Node Contracts

## Primary Input

The workflow accepts a free-form author request in Russian. The request may include:

- a prompt for new text
- a draft for editing
- a request to verify consistency against project materials

Optional runtime context:

- uploaded project files in Agent Builder knowledge/files
- pasted excerpt or chapter fragment

## Orchestrator Contract

The orchestrator must return structured output matching [schemas/orchestrator-output.schema.json](C:\CODEX\schemas\orchestrator-output.schema.json).

### Required Fields

- `task_type`: one of `generate`, `edit`, `check_context`
- `goal`: short normalized statement of the user's intent
- `constraints`: explicit user constraints as an array
- `requires_project_context`: boolean

### Optional Fields

- `genre`
- `tone`

### Contract Rules

- Keep `goal` short and actionable.
- Put only explicit user instructions into `constraints`.
- Set `requires_project_context` to `true` when the task depends on canon files or project materials.
- Do not generate literary output in this node.

## Worker Contract

Generator, Editor, and Context Checker all return the same shape defined in [schemas/worker-output.schema.json](C:\CODEX\schemas\worker-output.schema.json).

### Required Fields

- `primary_output`: the main deliverable for the user
- `notes`: short supporting notes
- `issues_found`: an array of issues or empty array
- `recommended_next_step`: a short next action

### Branch Expectations

- Generator:
  - `primary_output` contains the literary text or variants
  - `notes` may mention tone or structural choices
  - `issues_found` is usually empty
- Editor:
  - `primary_output` contains the revised text
  - `notes` summarize the main edits
  - `issues_found` is usually empty unless the source text has a blocking problem
- Context Checker:
  - `primary_output` contains the consistency verdict
  - `notes` contain concise recommendations
  - `issues_found` lists contradictions, missing facts, or limitations

## Finalizer Contract

The finalizer consumes the worker contract and produces user-facing prose.

### Output Rules

- The main result always comes first.
- Supporting notes come after the main result.
- Internal field names are never shown.
- If `issues_found` is non-empty, present it as concise remarks without sounding technical.
- If context files are missing, state that the canon check is limited and name the files that should be uploaded.
