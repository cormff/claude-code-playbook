# 12 — Artifacts

## When to use them

An Artifact turns an HTML/Markdown file into a hosted, private-by-default web page. Reach for it when visual communication is genuinely clearer than plain text — a dashboard, a comparison table, an interactive demo, a diagram, a report. Before writing one, load whatever design-guidance resource is available to calibrate how much design investment the page actually warrants (a simple table doesn't need a full dashboard treatment).

## Format rules

- Write the content to a file first, then publish it — don't inline the whole page in a single call.
- The file is **body content only** — no `<!DOCTYPE>`, `<html>`, `<head>`, or `<body>` tags; these get wrapped automatically at publish time (along with a minimal CSS reset).
- Always set a `<title>` — it names the browser tab and the gallery card, and should stay **stable** across redeploys.
- Pass a one-sentence description — it becomes the gallery card's subtitle.
- A favicon is **required**: one or two emoji, not markup. Keep it the same across redeploys of the same artifact — that's how a user recognizes the right tab. Only change it on a genuine topic pivot, not for incremental updates.

## Technical constraints

- **Must be fully self-contained.** A strict content-security policy blocks every request to an external host — CDN scripts, external stylesheets/fonts, remote images, fetch/XHR/WebSocket. Inline all CSS/JS and embed any images as `data:` URIs.
- **Must be responsive.** Relative units, flexbox/grid, `max-width:100%` on images. Wide content (tables, code blocks, diagrams) needs its own horizontally-scrolling container — the page body itself must never scroll horizontally.
- **Must be theme-aware.** Style for both light and dark: `@media (prefers-color-scheme: dark)` as the default signal, plus explicit dark/light attribute overrides on the root element, since a viewer's manual theme toggle needs to win in both directions.

## Updating

Publishing to the same file path redeploys the same URL; a different file path mints a new one. To update an artifact from an earlier, different conversation (or one referenced by a pasted URL), pass that artifact's URL explicitly — otherwise a new URL gets created even though the intent was to update the existing one.

## Privacy

An artifact is private by default; the user chooses if/when to share it. The same judgment call as uploading to any third-party tool applies (see [10 — Risk & Safety Protocol](10-risk-and-safety-protocol.md#what-counts-as-risky)): consider whether the content is sensitive before publishing, since it may be cached or indexed even after being taken down.
