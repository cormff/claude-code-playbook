# 06 — Context & Memory Management

## What a context window is, and why it matters

A context window is the full conversation history, tool results, and system instructions sent to the model on every turn. Each new turn re-sends (mostly via cheap cache reads) everything that came before — so as a conversation grows, the amount being re-transported grows with it. The real issue isn't context "filling up" so much as **per-turn cost and noise growing as the transported amount grows.**

**A measured real-world pattern worth internalizing:** sessions that ran past several hundred turns without ever clearing showed dramatically higher average cost per turn — on the order of several times higher — than short sessions under ~10 turns. This is a direct, structural consequence of how caching works, not a fluke of one dataset. Long, uncleared single sessions are consistently one of the biggest avoidable cost centers in practice.

## Clearing context

Clear (`/clear`) at natural breakpoints: when a phase/milestone/feature is done, write the state somewhere durable (a status file, a vault note) *before* clearing, then clear. A practical rule of thumb: once a session passes roughly 150 turns and a natural cut point has arrived (a sub-feature shipped, a report delivered), there's no reason to wait. Exception: a genuinely unsplittable, tightly cross-referential piece of iterative work (naming/categorization decisions that keep referring back to earlier choices) can reasonably stay in one session — the test is "do I need to remember the previous step?"; if the answer is no, it's splittable.

**This is always a manual/external step.** A slash command like `/clear` cannot be triggered by a skill or tool on its own (see [03 — Writing Effective Skills](03-writing-effective-skills.md#a-hard-limit-chaining-doesnt-reach-slash-commands)). The most an orchestrator skill can do is "write state, then stop" — a human (or an external trigger like a scheduled routine) has to actually issue the clear and re-invoke the skill to complete the cycle.

## Automatic summarization (compaction)

As a conversation approaches its context limit, the system automatically summarizes earlier messages; the summary plus the un-summarized recent messages carry forward into the next context window. This makes a conversation feel effectively unbounded, but it isn't a substitute for a deliberate `/clear` — automatic summarization risks losing detail, whereas a deliberate clear plus an explicit status-file write is a more reliable continuity method.

## Practical guidance

- Define a task in a single, complete message when you can, and point at a ready plan/report file rather than re-explaining it — this triggers large work without polluting context with irrelevant back-and-forth.
- Delegate incidental side-work (a one-off export script, a quick lookup) to a subagent instead of doing it in the main thread, to avoid inflating the main context for something tangential.
- At the end of a long block of work, ask for (or state) a short status confirmation — "what actually changed here, code or just a report?" — cheap insurance against a much more expensive misunderstanding later.

## The memory system

A persistent, file-based memory system stores information across conversations — separate from both the vault and the current session's task list. Each memory lives in its own file (frontmatter: `name`, `description`, a `type`), and a lightweight index file summarizes all of them; the index loads automatically at the start of every session.

### Four memory types

| Type | Contains |
|---|---|
| **user** | The user's role, expertise, responsibilities — how to address them |
| **feedback** | Working styles the user has confirmed or corrected — "don't do it that way" or "yes, keep doing exactly that" |
| **project** | Ongoing work/decision/event context not visible from the code itself — the "why" |
| **reference** | Pointers to information in external systems (an issue tracker, a chat channel, a dashboard) |

### Memory vs. Plan vs. Task

Memory is **cross-session** persistent — it survives past the end of a conversation. Plan and Task are **session-scoped, ephemeral** tracking mechanisms instead: a Plan aligns on an implementation approach before a large change; a Task list tracks progress on a multi-step piece of work within that one session. Once a plan is approved, "the plan was approved" isn't written to memory — it has no durable value once the session ends.

### Memory vs. the vault vs. CLAUDE.md

Three layers, three different scales:

- **Memory** — small, personalized, "how Claude should work with this person" — a few sentences at a time.
- **The vault** (see [02](02-obsidian-vault-integration.md)) — large, structured, cross-linked *content*: research, architecture, decisions. This is where a knowledge lookup should go first.
- **Project files / CLAUDE.md** — anything derivable from the code itself (architecture, conventions, layout) doesn't need a separate memory entry; the code and CLAUDE.md are already authoritative.

**Practical rule:** "Can this be re-derived from the code, git history, or the vault?" If yes, don't write it to memory. Only information that lives nowhere else and will shape future collaboration belongs there.

## Four mechanisms, one decision

The question that recurs throughout this playbook — "where does this rule/fact belong?" — always resolves to one of four places:

| Mechanism | Triggers when | Scope | Example |
|---|---|---|---|
| **CLAUDE.md** | Every session, automatically, unconditionally loaded | The directory it lives in and everything below it; shared with a team via the repo | An architectural rule, a convention, a forbidden operation |
| **Skill** (see [03](03-writing-effective-skills.md)) | Natural-language match or explicit `/skill-name` — triggered on demand | Personal or project-scoped | A multi-step orchestrator or procedure |
| **Hook** (see [09](09-automation-hooks-and-scheduling.md)) | A specific event fires, independent of the model's judgment | `settings.json` level (user/project) | Run a formatter after every edit; block access to a credential file |
| **Memory** (above) | Automatic, cross-session; *whether* something gets remembered depends on the model noticing it's worth keeping | User-wide, persists across conversations | A preference, a piece of feedback, project status |

Decision criteria:

1. A rule that's independent of the code but should **always** apply? → **CLAUDE.md**
2. A recurring but context-dependent procedure that needs human-like judgment? → **Skill**
3. An unconditional rule the model must never be able to skip or forget? → **Hook**
4. A small, person/project-specific fact that needs to survive across sessions? → **Memory**

These layer, they don't compete: a rule can be *explained* in CLAUDE.md or a skill and *guaranteed* by a hook at the same time — the instruction states intent, the hook enforces it.
