# 09 — Hooks & Scheduled Automation

## Hooks: event-triggered automation

A hook is a shell command defined in `settings.json` that runs automatically when a specific event fires. Common event types: `PreToolUse` (right before a tool call — can approve/deny/modify it), `PostToolUse` (after a tool runs), `UserPromptSubmit`, `SessionStart`/`SessionEnd`, `Stop` (when Claude finishes a response), `PreCompact` (before automatic summarization).

### Hook vs. skill

Both look like "automatic behavior" but the triggering mechanism is fundamentally different:

| | Skill | Hook |
|---|---|---|
| Triggered by | Natural-language match or `/skill-name` — depends on the model's judgment | A defined event firing — independent of the model's judgment |
| Can it be skipped | Yes — the model might not recognize relevance, or might apply it imperfectly | No — runs unconditionally at the harness level |
| What it is | Instruction text loaded into context (the model reads and applies it) | An executed command/script (no interpretation step) |
| Fits | Context-dependent procedures needing human-like judgment | Strict, exceptionless rules that must apply every time |

A concrete example of the two working together: "never read a `.env` file" can be written as an instruction (in CLAUDE.md or a skill — the model reads it and generally complies, but in principle could miss it) *and* enforced by a `PreToolUse` hook that unconditionally blocks `Read`/`Bash` calls matching that pattern. These aren't mutually exclusive — the instruction states intent, the hook guarantees it. If a rule in your vault-format skill (see [`layer-0-foundation/vault-format`](../skill-tree/layer-0-foundation/vault-format/SKILL.md)) matters enough that it should never be silently missed, that's a signal it's worth backing with a hook.

## Scheduled and background execution

Two different needs live under this heading: (a) waiting on something slow within the *current* conversation, and (b) kicking off a brand-new session on a schedule.

### Waiting within a session

A long-running command started in the background, paired with a scheduled wake-up after a set delay, removes the need for manual "is it done yet?" polling turns — which cost real tokens every time they happen.

### Repeating an action within a session

A looping mechanism that re-runs a prompt or slash command at a fixed interval, or paces itself dynamically, is useful for status checks or periodic monitoring within one ongoing conversation.

### Fully independent, scheduled new sessions

A cron-style scheduler creates, lists, and deletes fully independent scheduled "routines" that spin up new sessions on a timer, running in a headless/cloud environment. Because of that, MCP servers requiring interactive authentication (see [07](07-mcp-servers.md#security-and-authorization)) may not be usable inside a scheduled routine — plan a credential-based fallback for anything that needs to run unattended.

### Choosing the right mechanism

| Need | Mechanism |
|---|---|
| Wait on a background task within this session | scheduled wake-up |
| Repeat an action within this session | a loop |
| Start fully independent, scheduled new sessions | cron-based scheduling |

A daily routine that currently gets triggered manually ("check my status") is a natural candidate to automate this way — as long as every step it depends on (logins, API access) can actually run headless.
