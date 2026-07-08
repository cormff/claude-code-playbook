---
name: project-design
description: Design phase for {PROJECT} — data model, module boundaries, API contract, access control, and any multi-tenancy/security design, before code is written. Called by project-orchestrator before implementation; also used directly when the user says "design {PROJECT}'s X module."
---

# {PROJECT} Design

Produces the design artifacts that [`project-implement`](../project-implement/SKILL.md) will build against, and records any durable architectural decision.

## Produce

1. **Data model** — entities, relationships, and any project-wide invariants (e.g. tenant scoping, required indexes).
2. **Module boundaries** — what this module owns, what it exposes to (and consumes from) other modules.
3. **API contract** — request/response shapes, or an OpenAPI-style spec if the project already uses one.
4. **Access control** — who/what can do what.
5. **Cross-cutting concerns**, if relevant — multi-tenancy isolation, security constraints.

## Recording decisions

Any decision that would be expensive to silently reverse later (a schema choice, a boundary, a naming convention applied project-wide) goes into `Decisions.md` (see [`project-orchestrator`](../project-orchestrator/SKILL.md#workspace-fill-in-with-real-paths)) as a short ADR: context, decision, why, alternatives considered. Anything already locked there is a constraint on this design, not a fresh choice — flag it as the "genuinely critical decision" stop condition in the orchestrator if a real conflict comes up.

## Output

A design note under the module's vault folder, structured so [`project-implement`](../project-implement/SKILL.md) can build directly from it without re-deriving decisions that were already made here.
