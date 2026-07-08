---
name: task-list
description: Aggregate all open tasks across the vault (personal, project-specific, deadline-driven) into one prioritized list. Use when the user asks "what do I need to do," "what's on my plate," "to-do list," or /task-list.
---

# Task List — Unified View Across Sources

Tasks tend to live scattered across several source files (a personal to-do note, per-project status files, a deadlines note). This skill's job is to read all of them and produce one view, not to be the single place tasks are written.

## Step 1 — Read Every Source

Read each known task source in the vault (adapt this list to your own vault — e.g. a general to-do note, each active project's status file, a deadlines note).

## Step 2 — Merge and Prioritize

Combine everything into one list, ordered:

1. Overdue
2. Due today
3. Due this week
4. Later / no fixed date
5. Undated

Tag each item with which source file it came from, so the user can go fix it at the source rather than in this aggregated view.

## Step 3 — Present

A flat prioritized list, source-tagged, is usually more useful than grouping by project — the point of aggregation is to answer "what's most urgent right now" across everything at once.

## Adding or completing a task

If the user adds or completes a task during this conversation, write the change to its **source file**, not to this skill's output — the aggregated list is always regenerated, never the source of truth itself.
