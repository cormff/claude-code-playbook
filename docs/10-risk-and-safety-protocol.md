# 10 — Risk & Safety Protocol

## What counts as risky

- **Destructive operations** — deleting files/branches, dropping a database table, killing a process, `rm -rf`, overwriting uncommitted work.
- **Hard-to-reverse operations** — force-pushing (can overwrite a remote branch too), `git reset --hard`, amending an already-published commit, removing/downgrading a dependency, changing a CI/CD pipeline.
- **Actions visible to others or affecting shared state** — pushing code, opening/closing/commenting on issues or PRs, sending a message (chat, email), posting to an external service, changing shared infrastructure or permissions.
- **Uploading content to third-party tools** (a diagram renderer, a paste service, a gist) — this *publishes* the content; even if deleted later, it may already be cached or indexed. Consider whether the content is sensitive before sending it.

## Pre-flight checks

- Before any command that could discard uncommitted work (`git checkout/restore/reset/clean`, `rm -rf` inside a repo, restoring from a snapshot), **run a status check first** and stash (including untracked files) or commit whatever's there.
- After a broad stage/add, review what actually got staged — even an innocuous-looking filename deserves a second look if there's any chance it holds a secret.
- If a lock file exists, find out what process holds it before deleting it.
- Unfamiliar state (a file, branch, or config you don't recognize) gets investigated before it's deleted or overwritten — it might be someone's in-progress work. When in doubt, moving/renaming beats deleting.

## Confirmation format: options with trade-offs

At a genuinely risky or hard-to-reverse decision point, present options with their **trade-offs stated explicitly** — pros and cons for each — and mark a recommended option if there is one. This lets the decision-maker evaluate quickly instead of parsing a wall of prose.

**Keep the scope of an approval narrow.** Approval for one action applies to that action's stated scope — "approved this push" doesn't imply blanket approval for every future push. Don't extend authorization beyond what was actually granted.

## The bypass ban

- Skip a safety/signing check (`--no-verify`, `--no-gpg-sign`, etc.) only when explicitly asked to.
- Never force-push — especially to a main/default branch — without explicit approval.
- If a pre-commit hook fails, fix the issue and make a **new** commit rather than amending — the failed hook means the commit never actually happened, so an amend at that point would rewrite a *different*, earlier commit.
- Prefer staging files by name over a broad "stage everything" — it prevents an accidental `.env`, credential file, or large binary from getting swept in.
- When blocked by a permission error or a safety check, investigate the root cause rather than routing around it as a shortcut.

## The credential rule

Never paste a token, password, or API key directly into a chat message — conversation history persists as plain text on disk, so anything pasted there stays exposed indefinitely. Keep credentials in a separate, protected file and read from it only with explicit permission each time.

## Dry-run plus a confirmation gate for bulk changes

Before a change that touches production data or a large number of records, run a dry run, show the result, and only apply the real change after explicit confirmation. This single pattern — preview, then ask, then act — catches the overwhelming majority of "that bulk update did something unexpected" incidents before they happen.
