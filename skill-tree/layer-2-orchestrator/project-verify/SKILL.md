---
name: project-verify
description: Verification phase for {PROJECT} — unit and integration tests, isolation/access checks, and an end-to-end pass over the critical path for a completed slice. Called by project-orchestrator after implementation; also used directly when the user says "verify {PROJECT}'s X module" or "test this."
---

# {PROJECT} Verification

Confirms a change actually does what it claims — by exercising the affected flow, not just by trusting that tests/type-checks pass (see [08 — Project Development Lifecycle](../../../docs/08-project-development-lifecycle.md#where-code-review-and-verification-fit)).

## Checklist

1. **Unit + integration tests** for the new/changed code.
2. **Isolation and access checks**, if the project has multi-tenant or access-control requirements — e.g. confirm one tenant genuinely cannot see another's data, confirm role-based restrictions actually hold.
3. **End-to-end pass** on the critical path this slice touches — drive it, don't just read the code and assume it works.
4. **A code-review pass** before commit/PR — scanning for correctness bugs and simplification opportunities, separate from functional verification.

## When to skip

A diff that only touches tests or documentation, with no runtime surface to exercise, doesn't need the end-to-end pass — say so explicitly rather than fabricating a verification step that wouldn't observe anything real.

## On failure

Report exactly what failed and why, and hand back to [`project-implement`](../project-implement/SKILL.md) (or [`project-design`](../project-design/SKILL.md), if the failure reveals a design gap) rather than silently patching around a symptom.
