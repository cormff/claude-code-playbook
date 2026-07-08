---
name: milestone-tracker
description: Update "where we left off" tracking for {PROJECT} when a milestone or unit of work closes. Updates Status.md (active milestone + next concrete action) and Milestone_Log.md (chronological record); touches Goal.md/Decisions.md if needed. Called by project-orchestrator at the end of every milestone; also used directly when the user says "log this milestone" or "update status."
---

# {PROJECT} Milestone Tracker

This skill is what makes a two-word message like "continue" enough to resume a project exactly where it was left — see [08 — Project Development Lifecycle](../../../docs/08-project-development-lifecycle.md#cross-session-continuity-where-we-left-off).

## Step 1 — Confirm the milestone actually meets its definition of done

Don't log a milestone as closed if its DoD isn't actually met — this file is read as ground truth by the next session, so an inaccurate close compounds.

## Step 2 — Update `Status.md`

Overwrite the "where we left off" section with:
- The now-active milestone (the next one in sequence, per `Goal.md`).
- The next concrete action — specific enough that a fresh session with zero conversational context could act on it immediately.

## Step 3 — Append to `Milestone_Log.md`

A dated, one-entry-per-milestone chronological record: what was done, any notable decision or deviation, links to the relevant vault notes/commits.

## Step 4 — Update `Goal.md` / `Decisions.md` if needed

If the milestone list itself changed, or a new durable architectural decision emerged during this milestone, reflect it here — don't let it live only in the conversation that produced it.

## This is the "clear-context" handoff point

If [`project-orchestrator`](../../layer-2-orchestrator/project-orchestrator/SKILL.md) is about to stop for the "capacity limit" condition, this skill runs **first** — the whole point of the stop-and-resume pattern (see [03 — Writing Effective Skills](../../../docs/03-writing-effective-skills.md#a-hard-limit-chaining-doesnt-reach-slash-commands)) depends on `Status.md` being fully resumable *before* the context gets cleared.
