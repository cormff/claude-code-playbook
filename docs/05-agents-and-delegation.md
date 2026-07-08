# 05 — Agents & Delegation

## Subagent delegation

Whenever a research or exploration need shows up — scanning multiple files, APIs, or logs — delegating it to a subagent (a read-only explorer type, a general-purpose type, or a custom type) keeps that work out of the main thread's context instead of accumulating it there.

The critical detail is **when**, not just **whether**: delegation is most valuable throughout the whole session, not only at the start. A common failure pattern in practice is delegation clustering in the first 10-15% of a session — a few subagent calls up front — followed by hours of exploration and debugging done directly in the main thread with raw file/shell commands, which grows the main thread's context for no benefit. If a step genuinely requires scanning across multiple files/APIs/logs, delegate it *at that point*, not only at kickoff. Bulk, splittable work (e.g. reclassifying hundreds of files) can be split across parallel subagents by category or directory rather than handled one item at a time.

For work that genuinely needs many coordinated agents with deterministic control flow (fan-out/fan-in, staged pipelines, adversarial verification passes), a dedicated multi-agent orchestration tool exists — but it's meant to be invoked deliberately for large-scale review/research/migration efforts, not as a default. Reach for single subagent delegation first; reach for full multi-agent orchestration only when the task's scale genuinely calls for it.

## Defining a custom agent type

Beyond the built-in general-purpose agent types, project- or domain-specific agent types can be defined as `.claude/agents/<name>.md` files. The structure mirrors a skill: frontmatter (`name`, `description`, which tools it can access, an optional model override) plus a body describing that agent's role/system prompt.

**When it's worth it:** a recurring, delegable role exists that the general-purpose types don't fit well — e.g. a review-only agent with no write access, or an agent specialized in searching one particular domain. The threshold is the same one that governs skills in general: a one-off delegation isn't worth a custom type; a repeating pattern of delegation is.

**Restrict tool access deliberately.** Giving an agent only the tools it actually needs is both a safety and a predictability win — a read-only exploration agent having no write/edit access is a concrete example of this design. Apply the same discipline to a custom agent: more access than the role needs is pure downside risk.

## Nested delegation and context isolation

A subagent starts with its own **isolated, fresh** context — it does not inherit the parent thread's accumulated conversation history. This is a large part of *why* delegating research keeps a long session's cost under control: the exploration happens in a context that doesn't carry the parent's weight, and only a condensed result gets folded back in. It's worth being explicit that this is different from clearing the parent's own context (see [06](06-context-and-memory-management.md)) — isolation prevents the parent from growing further during that sub-task, it doesn't shrink what's already there.
