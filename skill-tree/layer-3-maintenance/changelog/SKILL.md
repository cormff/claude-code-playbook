---
name: changelog
description: Log a vault change (note created/edited/deleted) to a running daily changelog note. After any write to the vault, ask the user "should this be logged?" and run this skill on confirmation. Also runs directly when the user says /changelog.
---

# Vault Changelog

Keeps "what changed in the vault and why" traceable over time — useful once a vault has enough activity that reconstructing history from memory stops being reliable.

## When this runs

After a note is created, edited, or deleted, ask the user once: "log this?" Don't log automatically without asking — not every vault write is worth a changelog entry, and asking keeps the log meaningful instead of noisy.

## Format

Append to the current day's changelog entry (create it if it doesn't exist yet):

```markdown
## YYYY-MM-DD

- **[HH:MM]** <What changed> — <Why, one clause> — [[Note_That_Changed]]
```

## Keep it short

One line per change is enough. This is a traceability log, not a narrative — if more detail matters, that detail belongs in the note itself or in [`decision-log`](../../layer-1-universal/decision-log/SKILL.md), not here.
