# 01 — Installation & Configuration

## CLI installation

Claude Code is a terminal tool (the `claude` command), installed with `npm install -g @anthropic-ai/claude-code` or a platform-specific installer on macOS/Linux/Windows. The same underlying product is also available as a desktop app (Mac/Windows), a web app, and IDE extensions (VS Code, JetBrains) — they all share the same session and permission model.

## First login

`/login` connects to your Anthropic account (claude.ai or Console/API). If you belong to multiple organizations, this is where you pick which one you're operating under.

## Core settings (`/config`)

- **Theme** — light/dark/colorblind-friendly variants.
- **Permission mode** — controls how every tool call gets approved:

  | Mode | Behavior |
  |---|---|
  | `default` | Asks for confirmation on any potentially risky tool call |
  | `acceptEdits` | Auto-approves file edits, still asks on risky/hard-to-reverse operations |
  | `plan` | Read/research only; presents a plan and asks for approval before making changes |
  | `bypassPermissions` | No confirmations at all — only safe in an isolated, disposable environment (a worktree, a container) |

- **Model/effort selection** — see [04](04-models-effort-and-workflow.md).
- **Output/diff style** — formatting preferences.

## Settings layers

Settings (`settings.json`) live at three levels and override from narrow to broad:

1. **Project-local** (`.claude/settings.local.json`) — not checked into version control, personal/machine-specific.
2. **Project** (`.claude/settings.json`) — shared with the team.
3. **User** (`~/.claude/settings.json`) — applies everywhere.

Permission allowlists, hook definitions (see [09](09-automation-hooks-and-scheduling.md)), and environment variables all live here.

## A practical note on permissions

Every tool call that isn't pre-approved will interrupt you for confirmation. If you find yourself approving the same handful of read-only commands over and over, it's worth curating a project-level allowlist rather than tolerating the friction — but be deliberate about *what* you allow blanket-style: broad wildcards like unrestricted `Bash(*)` remove a safety net that exists for a reason (see [10 — Risk & Safety Protocol](10-risk-and-safety-protocol.md)).
