---
name: project-research
description: Research and data-gathering phase for {PROJECT} — domain knowledge, reference data models, external API details, and a gap analysis against what already exists. Called by project-orchestrator when a module/topic's knowledge is missing; also used directly when the user says "research {PROJECT}'s X module."
---

# {PROJECT} Research

Produces one accessible, cross-linked vault note per module/topic — not a one-off answer that disappears at the end of the conversation.

## Step 1 — Scope the module/topic

Identify what's being researched and where its note should live under `{PROJECT_NOTES_PATH}`.

## Step 2 — Gather

- Domain knowledge relevant to this module.
- Reference data models or prior art, internal or external.
- Any external system/API details this module needs to integrate with.

Delegate to a subagent whenever this involves scanning multiple sources — see [05 — Agents & Delegation](../../../docs/05-agents-and-delegation.md).

## Step 3 — Gap analysis

Explicitly compare "what exists already" against "what this module needs" and call out the gap — this is the part most likely to get skipped if research turns into a generic summary instead of decision-relevant input for the design phase.

## Step 4 — Write the note

Follow [`vault-format`](../../layer-0-foundation/vault-format/SKILL.md) conventions; link it from the project's map/hub. This note is what [`project-design`](../project-design/SKILL.md) will read next — write it for that purpose, not as a narrative transcript of the research process.
