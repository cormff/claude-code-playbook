---
name: inbox-triage
description: Process freeform notes and dropped files in {VAULT_PATH}/Inbox.md and {VAULT_PATH}/Inbox_Files/ — classify each item and file it into the correct place in the vault. Use when the user says "process my inbox," "sort my notes," or /inbox-triage.
---

# Inbox Triage

An inbox exists so that a fleeting thought or a dropped file doesn't have to be filed correctly *at the moment it's captured*. This skill does the filing later, in a batch.

## Step 1 — Read the Inbox

Read `Inbox.md` (freeform text notes) and list the contents of `Inbox_Files/` (dropped documents, audio, images, links).

## Step 2 — Classify Each Item

For each item, decide:

- **Which area/project it belongs to** (see [`vault-format`](../../layer-0-foundation/vault-format/SKILL.md) for the area/project structure).
- **What kind of note it becomes** — an addition to an existing note, a new topic note, a new project, or a task (route tasks to the source file used by [`task-list`](../task-list/SKILL.md)).
- **Whether it needs pre-processing** before it can be filed — e.g. a recorded audio file needs a transcript/summary before it's useful as a note; a long document needs a summary rather than being pasted in whole.

## Step 3 — File It

Move or write the content into the correct location following `vault-format` conventions (frontmatter, naming, navigation link, hub update). Once filed, remove the raw item from the Inbox — the inbox should end each triage pass empty or holding only genuinely unresolved items.

## Step 4 — Confirm

Give a short summary of what was filed where, so anything mis-classified can be corrected immediately rather than discovered later.

## Ambiguous items

If an item's destination genuinely isn't clear (not enough context to tell which project it belongs to), ask rather than guessing — a wrongly-filed note is often harder to find later than one left in the inbox.
