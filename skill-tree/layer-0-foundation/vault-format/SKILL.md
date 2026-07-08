---
name: vault-format
description: Formatting rules, folder structure, and note conventions for the Obsidian vault at {VAULT_PATH}. Claude must follow this skill whenever creating or editing any file under {VAULT_PATH}. This is the foundation skill — every other vault-facing skill assumes these conventions.
---

# Vault Format — Conventions for {VAULT_PATH}

This skill applies **only** to `{VAULT_PATH}`. If you maintain more than one vault, give each its own copy of this skill scoped to its own path, and never let one vault's conventions leak into another.

**Tool choice:** decide once whether this vault is accessed via an MCP server or via plain filesystem tools (`Read`/`Write`/`Edit`/`Glob`/`Grep`) — see [`docs/02-obsidian-vault-integration.md`](../../../docs/02-obsidian-vault-integration.md) for the trade-off. Write the decision here explicitly so it isn't re-litigated every session:

> Access method: _____ (fill in)

## Vault structure

```
{VAULT_PATH}/
├── _Hub.md                    — top-level index
├── Inbox.md / Inbox_Files/    — unsorted notes and dropped files awaiting triage
├── _Templates/                — starter files for new notes
├── Areas/<Area>/               — top-level life/work areas (e.g. Work, Personal, Study)
│   ├── <Area>_Hub.md
│   └── <Project or Topic>/     — one folder per project/topic within that area
└── ...
```

Every project/topic folder is a **growable set of notes**, not a fixed pair of files — anything that doesn't fit the main-note/function-map pattern below (a meeting note, a research finding, a decision record) can be added freely using the note template.

## Required frontmatter

Every file:

```yaml
---
date: YYYY-MM-DD
tags: [area, topic]
summary: One specific sentence describing what this file contains.
---
```

`summary` must be specific — "project note" is not acceptable; describe the actual content in under ~15 words.

## Tag taxonomy

Keep a living list here as the vault grows. Suggested starting categories: `hub` (index files), `project`, `reference`, `index`, plus one tag per area and one per active project/topic.

## Function/module map structure (for code projects)

Do **not** merge multiple source files' documentation into a single "function map" note — it becomes unmaintainable. Use three tiers instead:

1. **`<Project>_Overview.md`** — what the project is, its architecture, its stack.
2. **`<Project>_Map_Hub.md`** — an index of every per-file map, as a table: `| File | Path | Summary | Link |`.
3. **`Maps/<Project>_<FileName>_Map.md`** — one note per meaningful source file (including files in subdirectories, not just entry points), with a `| Function | Line | What it does |` table using real line numbers.

Trivial files (empty `__init__.py`-style files, migrations, config-only files) don't get their own note — list them as a single "no logic / config" row in the hub table instead.

**For very large codebases:** the unit can be a logical module/subsystem instead of a literal file, if a project has grown past the point where per-file notes stay navigable. This should be the exception, not the default.

**For multi-layer projects** (e.g. a backend + a mobile client): use layer-based subfolders under `Maps/` (e.g. `Maps/Backend/`, `Maps/Mobile/`), with one hub linking both.

## Sensitive-document rule

Anything in the category of personal bureaucracy (IDs, legal documents, applications, financial records) gets an **index entry only** — file name, real disk path, a one-line description derived from the file name. **Never read, copy, or summarize the actual content.**

## Credential exclusion rule

Never read, reference, or write the content of:

- Real `.env` files (a `.env.example` is fine — it's safe by convention)
- Private key files (e.g. `~/.ssh/id_*`)
- Any credential store file
- Anything containing a token/secret/password/license key

If a project has a `.env`, the note simply says "configured via `.env` (not in the repo)" — nothing about its contents.

## File naming and links

- File names: a single consistent case convention (pick one — e.g. `Title_Case_With_Underscores` — and never deviate).
- Internal links: `[[File_Name]]` (no extension), `[[Folder/File_Name]]`, `[[File_Name#Heading]]`, `[[File_Name|Display Text]]`.

## Navigation convention (required, one format only)

Every file ends with:

```markdown
## Navigation
→ [[Parent_Note_or_Hub]] · [[_Hub]]
```

Pick exactly one heading name for this ("Navigation," in this example) and use it everywhere — consistency here is what makes the vault traversable.

## Hub file format

Category/project hubs use a structured table:

```markdown
| File | Summary | Status |
|------|---------|--------|
| [[File_Name]] | One-sentence description | ✅ |
```

Status markers: ⬜ not started · 🟡 in progress · ✅ done · ⛔ blocked/dropped.

## Post-write checklist

1. Frontmatter present (`date` + `tags` + `summary`, all three required)?
2. `summary` is one specific sentence?
3. `tags` follow the taxonomy?
4. File ends with the one navigation heading (no alternate headings)?
5. If it's a function/module map: correct three-tier structure, not merged into one file?
6. If it's a sensitive document: index-only, content not copied?
7. Was any `.env`/credential file read? (It should not have been.)
8. Was the relevant hub updated with a link + summary for any new note?
9. Naming convention and `[[wikilink]]` usage followed?
