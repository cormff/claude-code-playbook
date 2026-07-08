# 04 — Models, Effort & the Agent Loop

## Model selection

| Situation | Suggested model |
|---|---|
| Routine, everyday work (small fixes, single features, status checks) | The default mid-tier model (e.g. Sonnet-class) |
| The hardest, longest-horizon, multi-phase work (research → design → code → verify chains) | The top-tier model — chosen deliberately for that task, not left on by default |
| Mid-scale code review / architectural analysis | A high-capability model one notch below the top tier |
| Simple, repetitive, templated tasks | The smallest/fastest model available |

Switch with `/model`. Treat this as a per-task decision, not a set-and-forget one — matching the model to the actual difficulty of the task in front of you.

## Effort / reasoning level

The effort level (`low`/`medium`/`high`/`xhigh`/`max`, set with `/effort`) controls how much the model "thinks" before answering — higher effort is deeper but slower and more expensive, lower effort is faster but shallower. Routine, single-step fixes don't need high effort; genuinely ambiguous, multi-step problems solved at low effort tend to lock in bad early decisions.

## Fast mode

`/fast` toggles a faster-output mode for top-tier models. It is **not** a downgrade to a smaller model — it's the same model producing output more quickly.

## Choosing by task size

A simple question drives both model and effort: *how many steps does this take, how hard is it to reverse, how long will it run?* Low-step/routine work gets low model+effort; multi-phase, architecturally significant, or hard-to-reverse work gets high model+effort. Defaulting to the most powerful model for everything is not free — the most capable model tier is typically also the most expensive per token, so reserving it for genuinely hard work (rather than leaving it on across an entire session) is the difference between a deliberate cost and a wasted one.

**A real observation worth internalizing:** in one month of measured real-world usage, the single most expensive per-token model accounted for a disproportionate share of total spend *not* because it was overused, but because it was used correctly — a handful of sessions, each one genuinely the hardest available work. That's the target pattern: rare, deliberate use of the top tier, not constant use.

## The agent loop

Claude Code runs on an agentic loop: a user message arrives → Claude calls one or more tools (read a file, run a shell command, edit code, delegate to a subagent, …) → the tool's result comes back into context → Claude decides the next step → repeat until the goal is reached or control returns to the user. Independent tool calls can be issued in parallel; dependent ones run sequentially.

## Task tracking

For anything with three or more real steps, or work whose complexity warrants it, a task list is opened: each item moves `pending` → `in_progress` → `completed`. This exists so the user can see progress and so nothing gets silently skipped mid-multi-step task. For single-step or trivial work, skip it — it's pure overhead in that case.
