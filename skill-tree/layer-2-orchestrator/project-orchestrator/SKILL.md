---
name: project-orchestrator
description: Main orchestrator for building {PROJECT}. Tracks cross-session continuity, reads "where we left off," routes to the correct phase sub-skill, and runs autonomously. Use when the user says "continue {PROJECT}," "work on {PROJECT}," "{PROJECT} where did we leave off," or /project-orchestrator. This is for the BUILD side of {PROJECT} — not for unrelated day-to-day tasks about it.
---

# {PROJECT} — Main Orchestrator

> Rename this skill (and the four phase skills it routes to) to something specific, e.g. `acme-orchestrator` / `acme-research` / …, once you adapt it to a real project. See [`skill-tree/README.md`](../../README.md#adapting-layer-2-to-your-own-project).

This skill runs the build of {PROJECT} across sessions. The vault is the single source of truth — everything else is derived from it.

## Workspace (fill in with real paths)

- **Continuity folder:** `{PROJECT_NOTES_PATH}/Dev_Tracking/`
  - `Status.md` — WHERE WE LEFT OFF (read this FIRST, every session)
  - `Goal.md` — the deep goal: product, architecture, milestone list
  - `Milestone_Log.md` — chronological progress
  - `Decisions.md` — locked architectural decisions (ADRs)
  - `Source_Map.md` — where the existing code/notes live
- **Code (target repo):** fill in
- **Existing code being migrated from, if any:** fill in

## Operating mode: default to autonomous

Once triggered, work **without stopping for permission** at every step. Keep the research → design → implement → verify → log → **next milestone** chain going continuously. Don't ask "should I start?", "should I continue?", "do you approve?" — make every reasonable, reversible call yourself and keep moving.

**Stop only for these three conditions:**

1. **The project is done** — every milestone in `Goal.md` is checked off. (Give a completion report.)
2. **A genuinely critical decision** — narrowly: (a) an architectural fork that would change a locked decision in `Decisions.md`, (b) a change to the scope/milestone list itself, (c) a hard-to-reverse or outward-facing action — pushing to a remote repo, sending data to an external system, using a real/live credential, a destructive file/DB operation (see [10 — Risk & Safety Protocol](../../../docs/10-risk-and-safety-protocol.md)). Ask briefly with `AskUserQuestion`, get the answer, keep going. Nothing outside this narrow set should interrupt the loop.
3. **Capacity limit** — context/token budget running out. Before stopping, **always** leave `Status.md` fully updated and resumable (the next concrete action must be unambiguous), then summarize where things stand in one paragraph.

If genuinely unsure but the work is reversible and consistent with locked decisions → don't ask, apply the most reasonable option. A brief progress note to the user after each milestone is fine, but don't stop and wait for a reply to it — move straight to the next unit of work.

## Flow (every session)

> Steps 1→5 are a **loop**: after step 5, return to step 2 and start the next milestone. The loop only breaks on one of the three stop conditions above.

### 1. Load status
Read `Status.md` and `Goal.md`; `Decisions.md` and `Source_Map.md` as needed. Identify the **active milestone** and the **next concrete action**.

### 2. Determine intent
- "Continue" → execute the next concrete action.
- A specific module/task named → jump to the relevant milestone in `Goal.md`.
- Ambiguous → pick the most reasonable incomplete sub-task under the active milestone's definition of done. (High autonomy: choose, don't ask.)

### 3. Route to the correct phase sub-skill
| Phase | Sub-skill | When |
|---|---|---|
| Research | [`project-research`](../project-research/SKILL.md) | Module/domain knowledge is missing |
| Design | [`project-design`](../project-design/SKILL.md) | Before coding — data model/boundaries/API/access control |
| Implement | [`project-implement`](../project-implement/SKILL.md) | Actual implementation |
| Verify | [`project-verify`](../project-verify/SKILL.md) | After implementation |
| Log | [`milestone-tracker`](../../layer-3-maintenance/milestone-tracker/SKILL.md) | At the close of every milestone |

A milestone typically flows design → implement → verify. If a module's research is incomplete, go to research first.

### 4. Advance the work (autonomously)
Carry the chosen unit of work through to its definition of done. Stay consistent with `Decisions.md`; add a new ADR entry if a new durable decision emerges.

### 5. Close the milestone and CONTINUE
When the work meets its definition of done, **always** call [`milestone-tracker`](../../layer-3-maintenance/milestone-tracker/SKILL.md): it updates `Status.md` + `Milestone_Log.md` and opens the next milestone. **Don't stop** — return immediately to step 2 for the next milestone. This loop only breaks on one of the three stop conditions above.

## Locked context (quick reference — fill in for your project)

- **Stack:** fill in.
- **Architecture:** fill in (e.g. modular monolith, multi-tenant, service boundaries).
- **Sequencing:** fill in which subsystems come first.
- If detail conflicts, `Goal.md` and `Decisions.md` are authoritative.
