# 03 — Writing Effective Skills

## What a skill is

A skill is an instruction file (`SKILL.md`) — YAML frontmatter plus a Markdown body — that packages a recurring procedure. When triggered, its full content is loaded into context and Claude follows it step by step. Two scopes exist:

- **Personal**: `~/.claude/skills/<name>/SKILL.md` — available regardless of working directory.
- **Project-scoped**: `<project>/.claude/skills/<name>/SKILL.md` — only suggested while working inside that project; it shows up name-prefixed by its directory (e.g. `apps/web:deploy`). If a personal and a project-scoped skill share a name, the more specific one wins.

## When a skill is worth writing

The core threshold: **will this work span multiple sessions, or will a future session need to ask "where did we leave off"?** If yes, a skill is worth the investment. If it's a one-off request, or something that genuinely requires different human judgment every time, don't — over-generalizing a skill just makes it useless or actively confusing.

Additional, concrete triggers worth watching for:

- **The same mistake or inefficiency is at risk of repeating.** Turning a one-time correction into a permanent rule inside the relevant skill file (instead of just fixing it in the moment) stops the same issue from recurring in every future session.
- **You have N nearly-identical tasks.** Five almost-identical "prepare for exam X" skills are better collapsed into one parametric skill driven by a config table — it removes both the maintenance burden and the ambiguity of "which skill will even trigger."
- **The work has multiple phases and needs continuity across sessions** (research → design → implement → verify → log, for example). This is exactly what an orchestrator skill is for (see below).

Not worth it: a genuinely one-off request; decisions that require different human judgment every single time; anything that hasn't repeated at least 2-3 times yet. This is the skill-writing equivalent of "three similar lines beat a premature abstraction."

## How to write one

### Frontmatter

Only `name` and `description` are required:

```yaml
---
name: skill-name
description: What it does + concrete trigger phrases + (if relevant) what's explicitly OUT of scope.
---
```

`description` is the most important field — Claude reads it to decide whether an incoming request matches this skill, so it functions like a matching engine. A good `description`:

- Opens with a third-person "use this when…" sentence.
- Lists real example trigger phrases, in quotes.
- States the explicit `/skill-name [argument]` invocation syntax.
- Adds negative scope where needed — e.g. "NOT for X, which is handled by the `x` skill" prevents collision with a topically-adjacent skill.

### Body patterns

Three shapes cover almost everything:

1. **Procedural (step-by-step).** A fixed "Step 0 → 1 → 2 → 3" loop, with repeating parameters (a name, an ID, a file path) pulled into a single config table and referenced with a placeholder like `{TARGET}` in the body. Result: one file plus an N-row table, instead of N near-duplicate files.
2. **Orchestrator.** Reads a status file → determines intent → routes to the right sub-skill (via the `Skill` tool) → advances the work → closes out and automatically moves to the next unit of work. Continuity lives in an external status file, not in the conversation's context — so the work can resume cleanly even after the context has been cleared.
3. **Reference/rule.** Not a procedure at all — a fixed set of constraints to follow on every relevant action ("format every file this way," "never read files matching this pattern").

Pick pattern 1 for a concrete, multi-step, one-shot task; pattern 2 for a multi-phase area of responsibility that spans sessions and splits into sub-tasks; pattern 3 for a standing rule that applies to every relevant action.

## How skills get triggered

- **Natural language** — when a message matches the keywords/example phrases in a skill's `description`.
- **Explicit invocation** — `/skill-name [argument]`.
- **Chaining** — a skill can call another skill via the `Skill` tool. An orchestrator closing out a unit of work by calling a logging skill, or a daily-routine skill that automatically also runs a task-aggregation skill, are both normal.

### A hard limit: chaining doesn't reach slash commands

A skill can call another skill or tool, but it **cannot** trigger a CLI slash command like `/clear`, `/model`, or `/effort` on itself. Those aren't tools Claude calls — they're commands the harness processes as user input. The practical consequence: an orchestrator skill cannot decide "I'll clear my own context now" mid-run. If the context *were* cleared mid-skill, the knowledge of being partway through that skill would vanish with it, unless that state was externalized first.

The actual pattern used to approximate "automatic" context clearing is: **skill + external status file + a manual `/clear`, working together.** An orchestrator's "I'm out of room" stop condition writes a fully resumable state to a status file, then stops; a human (or a scheduled trigger) clears the context and re-invokes the skill, which reads the status file and picks up exactly where it left off. See [06 — Context & Memory Management](06-context-and-memory-management.md#clearing-context) for the other half of this pattern.

A related but distinct mechanism: when an orchestrator delegates exploration to a subagent (see [05](05-agents-and-delegation.md)), that subagent starts with an **isolated, fresh** context — it doesn't inherit the parent thread's accumulated history. That keeps the parent thread from growing, but it does not clear the parent's own context; the subagent's summarized result still gets added back to it.

### The known trap: a written skill that never gets used

Writing the skill is necessary but not sufficient. It's entirely possible to build a solid orchestrator skill and then proceed to do the same class of work ad hoc anyway, without ever triggering it — the skill sits there, unused, and the investment is wasted. The fix isn't technical, it's a habit: at the start of a multi-step task, explicitly check "is there already a skill for this?" before defaulting to building the structure from scratch again.

## Maintaining a skill set

- **Turn feedback into a permanent rule.** When a mistake or inefficiency is caught, don't just fix it in the moment — write the correction into the relevant skill file so it doesn't recur.
- **Review periodically, in bulk.** A single, bounded-but-fully-authorized pass ("read every skill, find and fix duplication and format inconsistencies") tends to surface real duplication (e.g. five near-identical skills that should be one parametric one) and real mistakes (a skill file with malformed frontmatter) far more efficiently than reviewing skills one at a time as you happen to touch them.
- **Keep frontmatter format consistent** — always YAML; `name` should match the directory name.

## A worked example: bootstrapping from scratch

If you're setting up a fresh vault and skill set for a new project, sequencing matters — writing everything on day one both wastes effort and violates the threshold above.

**Vault setup, briefly:**
- Base a fresh setup on an installer script rather than building the skeleton by hand each time.
- Decide the access method (MCP vs. plain filesystem, see [02](02-obsidian-vault-integration.md)) on day one — it's painful to change later if multiple vaults end up coexisting.
- The **first** skill you write should be the format/rule skill — folder skeleton, frontmatter standard, naming, the navigation convention. Every skill written afterward assumes this baseline; retrofitting it later leaves earlier notes inconsistent.

**Skill-writing order — organic growth:**

| Phase | When | Skill type | Example |
|---|---|---|---|
| 0 | The moment the vault exists | Format/rule (universal, project-independent) | vault-format |
| 1 | First few sessions, once general needs are clear | Search/retrieval + inbox triage + task list (still project-independent) | vault-search, inbox-triage, task-list |
| 2 | Once a project is concrete enough to cross the threshold above (multi-phase, spans sessions) | Project-specific orchestrator + phase sub-skills | `<project>-orchestrator` → `-research`/`-design`/`-implement`/`-verify` |
| 3 | As the work continues, once ongoing maintenance needs appear | Continuity/maintenance skills | milestone-tracker, document-after-change, changelog |

Writing phase 2 at the same time as phase 0 is the most common way to end up with the "written skill, never used" trap above — an orchestrator built before the project is actually multi-phase tends to become a novelty rather than a habit. Write each skill after a real repetition or need has shown up, not in anticipation of one.

See [`skill-tree/`](../skill-tree/) for a ready-to-adapt implementation of exactly this layering.
