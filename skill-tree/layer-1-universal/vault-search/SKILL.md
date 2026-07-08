---
name: vault-search
description: Look up information about a project, decision, or topic by searching {VAULT_PATH} before answering. Use whenever the user asks something like "how does X work," "what did we decide about Y," "remind me what Z was," or references a project/file by name. Always check the vault before answering from general recollection.
---

# Vault Search — Look Before You Answer

The vault is the **primary** source of truth (see [`docs/02-obsidian-vault-integration.md`](../../../docs/02-obsidian-vault-integration.md)). This skill exists to make "check the vault first" the default, not an afterthought.

## Search strategy — search in cascading order, don't stop at the first hit

1. **Exact name match** — if the user names a project/file, look for its main note and hub directly.
2. **Hub-level scan** — read the relevant area/project hub to see what exists before diving into individual notes.
3. **Full-text search** — search across the vault for the topic keyword if no obvious note matches.
4. **Cross-links** — once a relevant note is found, follow its `[[links]]` outward; the answer is often in a linked note, not the first one found.

Don't answer from general/background knowledge if the vault has a specific, more authoritative note on the topic — read it and answer from that, even if it takes an extra step.

## When nothing is found

Say so explicitly rather than filling the gap with a plausible-sounding guess. Offer to create the missing note if the information was just established in conversation.

## Output

Answer using what the vault says, and cite which note(s) the answer came from (as a `[[Note_Name]]` reference) so the user can verify or update it directly.
