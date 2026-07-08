---
name: document-after-change
description: Update a project's README and the relevant vault notes (overview / module map) after a meaningful code change — new feature, new file, new module, or an architectural decision. Should trigger on its own once a coding task is finished, without the user having to ask — skip it for trivial (typo/formatting) changes. Also runs manually when the user says "update the docs" or "document this."
---

# Document After Change

Code and documentation drift apart the moment documentation becomes a separate, remembered step instead of part of finishing the task. This skill is meant to close that gap automatically.

## When this should fire

- A new feature, file, or module was added.
- An architectural decision was made or changed.
- A module's behavior changed enough that its existing documentation is now wrong.

## When to skip

Trivial changes — typo fixes, formatting, comment-only edits — don't need this.

## What to update

1. **The project's README**, if the change affects how the project is used, built, or structured.
2. **The relevant vault note(s)** — the project overview if architecture changed, the specific file/module map (see [`vault-format`](../../layer-0-foundation/vault-format/SKILL.md#functionmodule-map-structure-for-code-projects)) if a file's functions changed.
3. **The project hub**, if a new note was created and needs to be linked in.

## Don't scope-creep

This skill documents what changed — it isn't an invitation to refactor unrelated documentation or expand scope beyond the actual change. If the existing docs have unrelated problems, note them separately rather than fixing them inside this pass.
