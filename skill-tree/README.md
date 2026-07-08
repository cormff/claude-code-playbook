# Skill Tree

A layered, copy-and-adapt set of skills for running Claude Code against an Obsidian vault while a project (or several) grows. It's not a template to copy verbatim — it's a worked example of *which layer is needed when*, following the threshold described in [`docs/03-writing-effective-skills.md`](../docs/03-writing-effective-skills.md#when-a-skill-is-worth-writing).

```
skill-tree/
├── layer-0-foundation/     # the one skill everything else depends on
│   └── vault-format/
├── layer-1-universal/      # project-independent utilities
│   ├── vault-search/
│   ├── inbox-triage/
│   ├── task-list/
│   └── decision-log/
├── layer-2-orchestrator/   # project-specific, only add once a project needs it
│   ├── project-orchestrator/
│   ├── project-research/
│   ├── project-design/
│   ├── project-implement/
│   └── project-verify/
└── layer-3-maintenance/    # continuity and documentation habits
    ├── milestone-tracker/
    ├── document-after-change/
    └── changelog/
```

## Why layered, and why not write it all on day one

Writing every layer up front is the most common way to end up with skills that are technically present but never actually triggered — an orchestrator built before any project is multi-phase enough to need it tends to become dead weight rather than a habit. Add layers in order, and only when a real need has already shown up:

| Layer | Add it when |
|---|---|
| **0 — Foundation** | The moment a vault exists. Every other skill assumes this baseline; retrofitting it later leaves earlier notes inconsistent. |
| **1 — Universal** | Within the first few sessions, once the general need (search, inbox, tasks) is clearly recurring. Still entirely project-independent. |
| **2 — Orchestrator** | Only once a *specific* project crosses the threshold: multi-phase work that will span sessions, where a future session will need to ask "where did we leave off." |
| **3 — Maintenance** | As the project's work continues and a real, recurring maintenance need (status tracking, post-change documentation, change logging) appears. |

A small, single-session project might reasonably stop at layer 1 forever. That's a correct outcome, not an incomplete one.

## Placeholders used throughout

- `{VAULT_PATH}` — the absolute path to your vault.
- `{PROJECT}` — a project name/slug, used as a naming prefix for layer-2/3 skills (e.g. `{PROJECT}-orchestrator`).
- `{PROJECT_NOTES_PATH}` — the vault subfolder holding that project's notes.

Copy a skill's folder into `~/.claude/skills/` (or `<project>/.claude/skills/` for a project-scoped one), replace the placeholders, and adapt the specifics (your own folder names, tag taxonomy, status-file names) to match your own vault conventions from [`layer-0-foundation/vault-format`](layer-0-foundation/vault-format/SKILL.md).

## Adapting layer 2 to your own project

The five orchestrator-layer skills are named generically (`project-orchestrator`, `project-research`, …) — in practice you'd rename them with your actual project's name (e.g. `acme-orchestrator`, `acme-research`) so that unrelated projects in the same vault don't collide, exactly the way [`layer-0-foundation`](layer-0-foundation/vault-format/SKILL.md) recommends naming vault-specific skills distinctly from generic ones.
