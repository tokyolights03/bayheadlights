# CLAUDE.md

## Project

BayHeadlights — a single-page marketing site for a mobile headlight restoration
business on the Monterey Bay coast (Monterey, Carmel, Pacific Grove, Seaside,
Hollister). Everything lives in `index.html`: head metadata, styles, markup, then
scripts. `og-image.png` and `apple-touch-icon.png` are generated from the logo
embedded in the page and must ship alongside it.

The file is ~700 lines but ~590KB, because four images are inlined as base64 data
URIs. Read ranges rather than the whole file, and grep for anchors instead of
scrolling. The long lines are image data, not code.

## Response formatting

**Match the amount of structure to how many distinct categories the response
actually contains.** Over-structuring a small answer is as unhelpful as burying a
complex one in prose.

**One category** — write prose. No headings, no labels. A single fix is a
sentence or two, not a report.

**Two or more** — give each its own `##` heading so they render as visually
distinct blocks in the terminal. Use only the headings that apply, in this order:

### `## Changed`

What was actually done, and nothing else. Reference each edit as
`index.html:324` so it is clickable. If several files changed, use a table with
`File | Change` columns. Quote the specific selector, function, or copy that
changed rather than describing it vaguely.

### `## Verified`

What was checked and what came back — including failures, stated plainly with the
output. If something was skipped or could not be tested, say so here rather than
leaving it implied.

### `## Recommended`

Things worth doing that were **not** done. Numbered when priority matters,
bulleted when it does not. Each item carries a short reason. Anything needing
information only Jason has (photos, reviews, a travel radius, a real domain) says
so explicitly.

### `## Reminders`

Pending state and caveats, each as a `>` blockquote so it reads apart from the
lists above: uncommitted work, placeholder values, deferred decisions, open ports,
anything blocked on a reply.

## Formatting rules that always hold

- **Never let "Changed" and "Recommended" blend into one list.** A suggestion must
  never be phrased so it could read as completed work. This is the single most
  important rule here.
- State outcomes plainly. Done and verified is said without hedging; a failure is
  said with the evidence.
- Put a bare artifact or local URL on its own line so it stays tappable.

## Working conventions

- **Commit directly to `master`** in small pieces with messages that explain why.
  This is a solo repo with no remote, so branch only for work that might be thrown
  away. Confirmed with Jason on 2026-08-15.
- **Verify visually before claiming a UI change works.** Serve the file and drive
  a browser. The window cannot be resized below ~700px here, so test phone widths
  by rendering the page in a 390px iframe — media queries evaluate against the
  iframe's own viewport.
- **Never invent business facts.** No fabricated reviews, ratings, opening hours,
  service radius, or credentials. The structured data deliberately omits address
  and opening hours because this is a mobile, by-appointment business. If a claim
  needs a real-world fact, ask for it.
- Absolute URLs in the head still use the placeholder host `bayheadlights.com`,
  flagged by a `<!-- DEPLOY: -->` comment. Confirm the real domain before deploying.
