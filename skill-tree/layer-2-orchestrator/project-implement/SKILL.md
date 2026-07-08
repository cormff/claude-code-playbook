---
name: project-implement
description: Implementation phase for {PROJECT} — builds the actual code for a module or vertical slice against an existing design. Called by project-orchestrator once a design exists; also used directly when the user says "implement {PROJECT}'s X module."
---

# {PROJECT} Implementation

Builds to the design produced by [`project-design`](../project-design/SKILL.md) — this skill doesn't make new architectural decisions; if the design turns out to be wrong or incomplete once coding starts, that's a signal to go back to design, not to improvise around it silently.

## Work in vertical slices

Get one small, end-to-end piece working — a single entity, a single endpoint, a single test — verify it, commit it, then move to the next slice (see [08 — Project Development Lifecycle](../../../docs/08-project-development-lifecycle.md#small-slices-over-big-bang-delivery)). This keeps every step reviewable and lets the next step be shaped by the actual result of the last one.

## Risky or multi-step changes

Use branch isolation and checkpoint commits — each slice committed separately, with a clear message, so any step can be reverted without losing the rest. See [10 — Risk & Safety Protocol](../../../docs/10-risk-and-safety-protocol.md) before anything that touches shared state or production data.

## Definition of done

A slice is done when it's implemented, passes its own tests, and is committed — not merely "written." Hand off to [`project-verify`](../project-verify/SKILL.md) before considering the broader unit of work complete.
