# 08 — Project Development Lifecycle

## Five phases

A workable phase structure for any project big enough to span multiple sessions:

1. **Research.** Gather domain knowledge, reference data models, external API details; organize into accessible, cross-linked notes *before* writing code — this is what makes "existing solution vs. gap" clear up front instead of being discovered mid-implementation.
2. **Design.** Produce the data model, module boundaries, API contract, access control, and (if relevant) multi-tenancy/security design; record durable architectural decisions explicitly (an ADR-style note) so they aren't silently re-litigated later.
3. **Implement.** Build to the design.
4. **Verify.** Unit and integration tests, isolation/access tests where relevant, and an end-to-end check of the critical path.
5. **Document.** Update the README and the relevant vault notes (see [02](02-obsidian-vault-integration.md) and the [document-after-change](../skill-tree/layer-3-maintenance/document-after-change/SKILL.md) skill).

A single unit of work typically flows design → implement → verify; if a module's research is incomplete, go back to phase 1 first.

## Cross-session continuity ("where we left off")

Continuity should live in an external status file, not in context: an active-milestone-plus-next-action file, a chronological log, and a locked-decisions file. Every session starts by reading these; every milestone close updates them. Done well, a two-word message like "continue" is enough to restore full session state — see the [milestone-tracker](../skill-tree/layer-3-maintenance/milestone-tracker/SKILL.md) skill for a concrete implementation.

## Where code review and verification fit

- **A code-review pass** runs after code is written but before commit/PR — scanning for correctness bugs and simplification/efficiency opportunities.
- **A verification pass** confirms the change actually does what it claims by exercising the affected flow end-to-end, rather than trusting tests/type-checks alone. Skip it for diffs that only touch tests or docs with no runtime surface to exercise.

Both are the last checkpoint before moving to the next step (commit, milestone close).

## Small slices over big-bang delivery

Work in vertical slices: get one small end-to-end piece (a single entity, a single endpoint, a single test) working, verify it, commit it — then move to the next slice. This keeps every step reviewable and lets the next instruction be shaped by the actual previous result, instead of over-planning ahead. Combine this with branch isolation and checkpoint commits for risky/multi-step work — it keeps every step easy to reverse.

## Keeping the vault current as a habit, not an afterthought

- **Push research into the vault as you go**, not as a final write-up — a module researched mid-project should already be a cross-linked note before implementation starts, not reconstructed from memory afterward.
- **Document right after a meaningful code change** (a new feature, file, module, or architectural decision) — README and the relevant vault note, not just the code. Trivial changes (typo/formatting) don't need this.
- **Log vault changes as they happen** — after a note gets created/edited/deleted, a lightweight changelog entry keeps "what changed and why" traceable over time.
- **Treat the vault as the primary knowledge source, not a backup memory.** This is easy to state as a rule and easy to silently skip in an ad hoc session that never triggers the skills that enforce it — which is exactly why it's worth checking, explicitly, whether the relevant skill got triggered (see [03](03-writing-effective-skills.md#the-known-trap-a-written-skill-that-never-gets-used)).
