# CLAUDE.md

## Project

BayHeadlights — a single-page marketing site for a mobile headlight restoration
business on the Monterey Bay coast (Monterey, Carmel, Pacific Grove, Seaside,
Hollister). Everything lives in `index.html`: head metadata, styles, markup, then
scripts. It is ~975 lines and ~47KB — comfortable to read whole.

The four images are separate `.webp` files, not base64 data URIs; they were
extracted on 2026-08-16. `og-image.png` and `apple-touch-icon.png` are referenced
by absolute and root-relative URL respectively, so both must sit at the site root
or link previews and the iOS home-screen icon break.

Supporting files, each with a header comment explaining itself:

| File | Purpose |
|---|---|
| `404.html` | Branded error page, self-contained (no shared stylesheet exists) |
| `_headers` | Security and caching headers; Cloudflare consumes it, never serves it |
| `wrangler.jsonc` | Pins the Worker name and makes `404.html` serve on unmatched paths |
| `.assetsignore` | Keeps repo files (`CLAUDE.md`, config) from being served publicly |

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
  Solo repo, so branch only for work that might be thrown away. Confirmed with
  Jason on 2026-08-15.
- **Pushing deploys.** `git push` to `master` triggers a Cloudflare build that is
  live in ~40s. There is no separate deploy step and nothing to click. Never edit
  files in the Cloudflare dashboard — the next push overwrites them.
- **Verify visually before claiming a UI change works.** Serve the file and drive
  a browser. The window cannot be resized below ~700px here, so test phone widths
  by rendering the page in a 390px iframe — media queries evaluate against the
  iframe's own viewport.
- **Never invent business facts.** No fabricated reviews, ratings, opening hours,
  service radius, or credentials. The structured data deliberately omits address
  and opening hours because this is a mobile, by-appointment business. If a claim
  needs a real-world fact, ask for it.
- **The domain is real and permanent.** `bayheadlights.com` is registered and live;
  the absolute URLs in `index.html`, `robots.txt` and `sitemap.xml` are correct as
  written. The old `DEPLOY:` comments are gone, so if the host ever changes, grep
  for `bayheadlights.com` across those three files — nothing marks them any more.
  Treat the URL as fixed: this is a QR-code landing page and printed codes die if
  it moves.

## Where it lives

| | |
|---|---|
| Site | https://bayheadlights.com (and `www.`, which serves directly rather than redirecting) |
| Repo | `github.com/tokyolights03/bayheadlights`, branch `master` — **public** |
| Host | Cloudflare **Worker with static assets** named `bayheadlights` — *not* a Pages project |

Two traps worth knowing. Cloudflare's unified flow creates Workers, not Pages, so
Pages-specific instructions will not match the UI. And the Worker overview says
"Manually deployed" even when Git *is* connected — that label describes a single
version, not the project; check **Settings → Build** before concluding otherwise.
