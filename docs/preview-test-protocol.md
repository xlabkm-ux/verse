# Preview Test Protocol

## Purpose

Validate the Verse MVP workflow in OpenAI Agent Builder Preview before publishing the first version.

## Test Procedure

1. Upload the relevant canon files from `knowledge/` into Builder.
2. Open Preview for the workflow.
3. Run each case from [tests/preview-cases.md](C:\CODEX\tests\preview-cases.md).
4. Record:
   - selected route
   - notable output quality issues
   - whether the finalizer removed technical noise
   - whether context checks used files correctly

## Acceptance Criteria

- The orchestrator selects the expected `task_type` for each core case.
- The generator produces usable literary material rather than outline-only filler.
- The editor preserves author intent while improving clarity, rhythm, or tension.
- The context checker identifies contradictions when canon files exist.
- The context checker refuses to hallucinate canon when canon files are absent.
- The finalizer returns one clean author-facing answer across all branches.

## Common Failure Modes to Watch

- Orchestrator overuses `generate` for editing tasks.
- Generator becomes generic and ignores tone or genre.
- Editor rewrites too aggressively and loses the author's voice.
- Context checker reports certainty without evidence from files.
- Finalizer repeats internal labels such as `task_type` or `primary_output`.

## Recommended Fix Order

1. Fix routing errors in the orchestrator prompt and structured output examples.
2. Tighten generator or editor style rules if output quality is too broad or too stiff.
3. Clarify context checker limitations around missing files and ambiguous evidence.
4. Adjust finalizer formatting only after branch behavior is stable.
