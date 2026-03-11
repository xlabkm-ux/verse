# Preview Cases

## Routing Cases

### Case R1

Prompt:

`Придумай сцену ссоры между братом и сестрой после похорон отца. Тон сдержанный, но с нарастающей яростью.`

Expected route:

- `generate`

Acceptance:

- `goal` reflects scene generation
- `tone` captures the requested emotional temperature
- no project context required

### Case R2

Prompt:

`Сделай этот абзац короче и напряжённее, убери лишние объяснения, но сохрани мой голос.`

Expected route:

- `edit`

Acceptance:

- `goal` reflects revision rather than new writing
- `constraints` mention brevity and preserving author voice

### Case R3

Prompt:

`Проверь, не противоречит ли новая глава уже созданной биографии героя и временной линии.`

Expected route:

- `check_context`

Acceptance:

- `requires_project_context` is `true`
- branch reaches File Search before Context Checker

## Generate Cases

### Case G1

Prompt:

`Напиши диалог двух бывших друзей, которые делают вид, что всё в порядке, хотя один из них предал другого.`

Acceptance:

- the dialogue is scene-like, not abstract commentary
- tension is visible through subtext
- final answer begins with the actual text

### Case G2

Prompt:

`Придумай короткий синопсис мистической повести о городе, где после заката исчезают тени.`

Acceptance:

- genre is reflected in the imagery and premise
- output is concrete rather than generic

## Edit Cases

### Case E1

Prompt:

`Перепиши сцену так, чтобы стало меньше объяснений и больше подтекста.`

Acceptance:

- revised text is delivered first
- notes briefly explain the main improvements
- result keeps the apparent core intent

### Case E2

Prompt:

`Сократи этот фрагмент на треть, но оставь чувство вины и скрытую агрессию.`

Acceptance:

- reduced length
- emotional pressure preserved
- no unnecessary stylistic drift

## Context Cases

### Case C1

Prompt:

`Сверь эту сцену с правилами мира и временной линией.`

Acceptance:

- File Search is used
- the verdict references evidence from uploaded materials
- contradictions appear in `issues_found`

### Case C2

Prompt:

`Проверь, не ломает ли новая глава биографию главного героя.`

Acceptance:

- the answer names the biography limitation if the file is missing
- no invented canon facts
- final answer still contains a useful next step

## Finalizer Cases

### Case F1

Use any successful `generate` result.

Acceptance:

- no raw field labels such as `primary_output`
- notes appear after the main text

### Case F2

Use a `check_context` result with missing files.

Acceptance:

- limitation is stated plainly
- requested files are named
- tone remains helpful and non-technical
