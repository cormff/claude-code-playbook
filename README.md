# Claude Code Playbook

A practical playbook for using [Claude Code](https://claude.com/claude-code) effectively — how to set it up, how to build automation on top of it (skills, hooks, scheduled agents), how to connect it to an Obsidian knowledge vault, and a ready-to-adapt **skill tree** for keeping a growing project organized across sessions.

This isn't official Anthropic documentation. It's a distilled set of practices from real day-to-day use, including a few weeks of measuring actual token/cost behavior to see what really matters and what's just folklore. Every claim in here is either a documented product behavior or something observed in practice — nothing is speculative "best practice" copy-paste.

## Why this exists

Claude Code gives you a lot of primitives — skills, subagents, hooks, memory, MCP servers, scheduled routines — and very little guidance on *when* to reach for which one. Most of the friction we found in practice wasn't "Claude did the wrong thing," it was "a mechanism existed but was never triggered," or "the wrong mechanism was used for the job." This playbook exists to make those decisions explicit.

## How it's organized

| Doc | Covers |
|---|---|
| [01 — Installation & Configuration](docs/01-installation-and-configuration.md) | CLI setup, login, `/config`, permission modes, settings layers |
| [02 — Obsidian Vault Integration](docs/02-obsidian-vault-integration.md) | Why a vault, structure conventions, MCP vs. plain filesystem access |
| [03 — Writing Effective Skills](docs/03-writing-effective-skills.md) | What a skill is, when to write one, how to structure it, how it's triggered, how to maintain it |
| [04 — Models, Effort & the Agent Loop](docs/04-models-effort-and-workflow.md) | Model/effort selection, the conversation→tool→result loop, task tracking |
| [05 — Agents & Delegation](docs/05-agents-and-delegation.md) | Subagent delegation, custom agent types, multi-agent orchestration |
| [06 — Context & Memory Management](docs/06-context-and-memory-management.md) | Context windows, `/clear`, auto-memory, and a decision table for CLAUDE.md vs. Skill vs. Hook vs. Memory |
| [07 — MCP Servers](docs/07-mcp-servers.md) | What MCP is, when installing one is actually justified |
| [08 — Project Development Lifecycle](docs/08-project-development-lifecycle.md) | Research → design → implement → verify → document, and cross-session continuity |
| [09 — Hooks & Scheduled Automation](docs/09-automation-hooks-and-scheduling.md) | Event-driven hooks vs. skills, background waiting, loops, cron-scheduled agents |
| [10 — Risk & Safety Protocol](docs/10-risk-and-safety-protocol.md) | What counts as risky, pre-flight checks, confirmation format, the bypass ban |
| [11 — Continuous Improvement](docs/11-continuous-improvement.md) | Turning feedback into permanent rules, periodic skill review |
| [12 — Artifacts](docs/12-artifacts.md) | Publishing visual output — format rules that actually matter |

## The skill tree

[`skill-tree/`](skill-tree/) is a layered, copy-and-adapt set of skills for anyone starting a fresh Obsidian vault + Claude Code setup and wanting it to scale as a project grows, instead of writing everything ad hoc from day one.

```
skill-tree/
├── layer-0-foundation/     # the one skill everything else depends on
├── layer-1-universal/      # project-independent utilities
├── layer-2-orchestrator/   # project-specific orchestrator + phase skills
└── layer-3-maintenance/    # continuity and documentation habits
```

See [`skill-tree/README.md`](skill-tree/README.md) for the full rationale — including *why* you shouldn't write all four layers on day one.

## Quick start

1. Read [01 — Installation & Configuration](docs/01-installation-and-configuration.md) and get Claude Code running.
2. Read [02 — Obsidian Vault Integration](docs/02-obsidian-vault-integration.md) and set up (or connect) a vault. [`templates/vault-skeleton/`](templates/vault-skeleton/) has starter files.
3. Copy `skill-tree/layer-0-foundation/vault-format/` and `skill-tree/layer-1-universal/` into `~/.claude/skills/`, replace the `{VAULT_PATH}` placeholder, and adapt the taxonomy to your own vault.
4. Only add `layer-2-orchestrator/` skills once a real project actually needs cross-session continuity — see [03 — Writing Effective Skills](docs/03-writing-effective-skills.md#when-a-skill-is-worth-writing) for the threshold.
5. Read [10 — Risk & Safety Protocol](docs/10-risk-and-safety-protocol.md) before letting any of this run autonomously on real data.

## License

[MIT](LICENSE) — use, adapt, and redistribute freely.
