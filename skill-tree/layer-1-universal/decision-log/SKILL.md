---
name: decision-log
description: Record an architectural decision, a resolved bug's root cause, a coding standard, or a system configuration as a structured note in the vault. Use when the user says "remember this," "save this to the vault," "document this decision," or /decision-log.
---

# Decision Log — Recording Durable Knowledge

Not everything worth remembering belongs in Claude's [cross-session memory](../../../docs/06-context-and-memory-management.md#the-memory-system) — memory is for small, personal working-style facts. Anything with enough substance to be useful to a *future reader of the project*, not just future-Claude, belongs in the vault instead.

## What qualifies

- An architectural decision and its reasoning (why this approach, what was ruled out and why).
- A non-obvious bug's root cause and fix — not "what the fix was" (that's in the diff/commit) but *why it happened* and what would have caught it sooner.
- A coding standard or convention specific to this project.
- A system/environment configuration that isn't self-evident from the code.

## What doesn't

- Anything already derivable by reading the code or git history — don't duplicate it.
- Ephemeral, in-progress task state — that belongs in a status file (see [`milestone-tracker`](../../layer-3-maintenance/milestone-tracker/SKILL.md)), not a permanent decision note.

## Format

```markdown
---
date: YYYY-MM-DD
tags: [decision, <project>]
summary: One sentence stating the decision or fact.
---
# <Decision Title>

## Context
What situation led to this.

## Decision
What was decided.

## Why
The reasoning — including alternatives considered and why they were rejected, if relevant.

## Navigation
→ [[Project_Hub]] · [[_Hub]]
```

File it under the relevant project folder, following [`vault-format`](../../layer-0-foundation/vault-format/SKILL.md) conventions, and link it from that project's hub.
