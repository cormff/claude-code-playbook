# 02 — Obsidian Vault Integration

## Why a vault at all

Claude's built-in [memory](06-context-and-memory-management.md) is deliberately small — it's meant for personal working preferences, not for bulk knowledge. An Obsidian vault is where the *actual content* lives: project architecture, research findings, decisions, ongoing status. The rule that matters most in practice:

> When a question requires looking something up, the vault is the **primary** source of truth. Claude's file-based memory is secondary — a thin layer of "how to work with this person," not a knowledge base.

If this priority gets inverted (Claude answers from vague recollection instead of reading the vault), you get confidently wrong answers about things that are written down two clicks away.

## Structure conventions

A vault that stays usable as it grows needs a small number of enforced conventions, not a rigid schema:

- **Hub notes** — an index file per category/project, linking out to everything else in a table.
- **A main note per project** — what it is, its architecture, its stack.
- **A function/module map for code projects** — one note per meaningful source file (or per logical module, for very large codebases), with a line-numbered table of what's defined where. Do **not** merge multiple files' maps into one note — it becomes unmaintainable and un-linkable.
- **Consistent frontmatter** — every note gets `date`, `tags`, and a one-sentence `summary`. This is what makes search actually work instead of relying on remembering file names.
- **A single navigation convention** — e.g. every note ends with a link back to its parent hub. Pick one and never deviate; consistency is what lets Claude (and you) traverse the vault reliably.

Folders should be treated as *growable sets of notes*, not a fixed two-file pattern — a project folder can hold meeting notes, research, and decisions alongside its main note and function map.

## Connecting Claude to the vault

There are two valid approaches:

- **Via an MCP server** (e.g. an Obsidian MCP integration) — gives you purpose-built tools (`read_file`, `write_file`, `edit_file`, `search_files`, etc.). The catch: an MCP connection is typically pinned to **one** vault. If you ever run two vaults side by side (e.g. during a migration), it can't follow you to the second one.
- **Via plain filesystem tools** (`Read`/`Write`/`Edit`/`Glob`/`Grep`) — an Obsidian vault is just a folder of Markdown files, so ordinary file tools work perfectly well. This is the better choice if multiple vaults need to coexist, since file tools aren't pinned to anything.

In practice: pick one method per vault and write it down as a rule in your [foundation skill](../skill-tree/layer-0-foundation/vault-format/SKILL.md) — mixing both for the same vault in different sessions creates inconsistent behavior for no benefit.

## Setting up a new vault

Rather than building the skeleton by hand every time, script it once: a small installer that (1) registers the MCP server if you're using one, (2) copies your baseline skills (format rules, search, inbox triage) into `~/.claude/skills/`, and (3) creates the initial hub/index files if they don't exist yet. See [`templates/vault-skeleton/`](../templates/vault-skeleton/) for a minimal starting point, and [03 — Writing Effective Skills](03-writing-effective-skills.md#a-worked-example-bootstrapping-from-scratch) for the order in which to actually write the skills that operate on it.
