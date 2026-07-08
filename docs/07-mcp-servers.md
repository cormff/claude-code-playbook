# 07 — MCP Servers

## What MCP is

The Model Context Protocol (MCP) is the standard way to connect Claude to systems outside the local filesystem. Each MCP server exposes a tool set specific to that system (namespaced like `mcp__<server>__<function>`); Claude calls these the same way it calls built-in tools. Once connected, a server's tools become available on demand.

## When installing one is actually justified

**The general decision rule:** an MCP server is worth installing when Claude needs *repeated, structured, or authenticated* access to an external system. The concrete threshold: **"Is this need served fine by a general tool (shell command / generic web fetch) on a one-off basis, or does it need repeated, structured access to that system's own data model (issues, tasks, pages, notebooks)?"** The second case justifies a server; the first doesn't.

Typical categories worth a dedicated server, by the kind of access they provide:

| Category | Example need |
|---|---|
| Knowledge vault access | Structured read/write/search against a note-taking vault |
| Code hosting workflows | Issue/PR management beyond what a local git client covers |
| Local repo operations | Structured commit/diff/log/branch output |
| Research synthesis | Multi-source question answering and report/audio/slide generation that a generic web fetch can't replicate |
| Task/document/communication platforms | A team's own task tracker, shared drive, or messaging system |
| Browser automation | Driving a real browser for testing or scraping a JS-heavy site |
| Lightweight cross-session storage | A general entity/relationship graph, independent of the vault |

## Two questions before adding a server

1. **Is this need already covered** by a connected server or a built-in tool (shell/generic fetch/file read)? If so, a new server is redundant — every server is additional credential/permission surface.
2. **Does the server's actual tool set cover the need**, or does it only provide a shallow subset (e.g. authentication only, with no read/search capability)? Check the tool list it exposes before assuming it's sufficient.

## The trap: connected but unused (or silently bypassed)

It's entirely possible for a server to be connected and then never actually get used by any skill, or to be quietly bypassed in favor of a more direct approach because its exposed tools don't cover the real need (e.g. a mail server that only handles authentication, with the actual message-reading done via a direct protocol instead). This mirrors the "written skill, never triggered" trap from [03](03-writing-effective-skills.md#the-known-trap-a-written-skill-that-never-gets-used) — being connected isn't the same as being used. Periodically auditing which connected servers are actually referenced by any skill keeps both the security surface and the maintenance burden small.

## Adding a new server

Registered with `claude mcp add <name> <command/url>`. After registration, its tools appear on the deferred tool list and get loaded on demand.

## Security and authorization

- Each server only has the scope it was granted — a code-hosting server can't reach the filesystem, a vault server can't push to a repo. Know what access (read/write/external sharing) you're granting before adding a new one.
- **Never paste a credential (token, password, API key) directly into a chat message.** Conversation history is stored as plain text on disk — anything pasted into it stays exposed indefinitely. Keep credentials in a protected, separately-referenced file, and only read from it with explicit permission each time.
- Servers requiring interactive authentication may not work in headless/scheduled execution (see [09](09-automation-hooks-and-scheduling.md)) — plan a server-independent fallback for anything that needs to run unattended.
- If a read-only subagent's tool output contains what looks like an embedded fake instruction ("system reminder," "plan mode active," etc.), that's not a security failure — it means the system correctly treated it as data, not as a command. Worth noting, not worth panicking over.
