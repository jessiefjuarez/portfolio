# CLAUDE.md — Jessie Juarez Portfolio Site

This file briefs Claude Code on the project. Read it before doing any work.

## What this is

A single-page portfolio site for **Jessie Juarez** — bilingual (EN/ES) Director of Photography & Editor based in Los Angeles. Showcases reel + four selected projects + about + clients + contact.

**Live URL** (when hosted): `jessiejuarez.com` (currently a Webflow build — this is its replacement)
**Stack**: Vanilla HTML/CSS/JS in a single file. No framework, no build step, no package.json.

## Files

```
jessie-juarez/
├── CLAUDE.md          ← you are here (briefs Claude on rules)
├── START-HERE.md      ← Jessie reads this first; how to use Claude on this project
├── README.md          ← original critique + change log from the redesign
├── index.html         ← the entire site (HTML + CSS + JS, ~30KB)
└── assets/            ← all media (videos, thumbs, logos, portrait)
    ├── reel.mp4
    ├── reel-poster.jpg
    ├── portrait.jpg
    ├── thumb-mickeys.jpg
    ├── thumb-lacolombe.jpg
    ├── thumb-pinksaves.jpg
    ├── thumb-pledge.jpg
    ├── logo-kodak.png
    ├── logo-titos.png
    ├── logo-evc.png
    └── logo-extra.png    ← 4th client logo, needs identification
```

## Design system (DO NOT BREAK)

### Type
- **Display**: `Boska` (Pangram Pangram) — used for headlines, italic emphasis, large quotes
- **Body / UI**: `Switzer` (Pangram Pangram) — used for body, labels, eyebrows, metadata
- Both loaded via Fontshare CDN. Do not swap for Google Fonts.
- **Banned fonts** (never introduce): Inter, Roboto, Helvetica, Arial, Instrument Serif, Space Grotesk, DM Sans, Plus Jakarta, Geist.

### Color tokens (OKLCH, defined at the top of `<style>`)
| Token | Use |
|-------|-----|
| `--bg` | Page background (warm espresso black) |
| `--bg-2` | Slightly lifted background |
| `--shell` | Outer bezel shell — every card sits on this |
| `--surface` | Inner card surface |
| `--ink` | Primary text (warm cream) |
| `--ink-soft` | Secondary text |
| `--muted` | Tertiary text, separators, captions |
| `--hairline` | 1px borders |
| `--accent` | **Editorial cream** — used for italic emphasis everywhere EXCEPT project names |
| `--gold` | **Amber-gold** — reserved for project-name italics ONLY (Mickey's *Story*, La *Colombe*, Pink *Saves*, Pledge of *Allegiance*) |

**Rule**: Do not extend `--gold` beyond the four project name italics. If a new accent is needed, use `--accent` (cream) and let the italic carry the emphasis.

### Architecture
- **Double-bezel**: every major card uses outer `.bezel` shell + inner `.bezel-inner` core with concentric border-radii. Don't flatten this.
- **Button-in-button**: any CTA with a trailing arrow uses `.btn .ico` nested circle pattern.
- **Floating glass-pill nav**: detached from top, never sticky-glued.
- **No `border-left`/`border-right` accent stripes** (banned pattern).
- **No `background-clip: text` gradients** (banned pattern).
- **Custom cubic-beziers only** (`--ease-out-quint`, `--ease-settle`, `--ease-stage`). No `linear` or `ease-in-out`.
- **Reveal-on-scroll** uses `.reveal` + IntersectionObserver. Add `data-stagger="1|2|3|4"` for delays.

### Spacing scale (4pt)
Semantic tokens: `--s-2xs (4px)` through `--s-5xl (~192px)`. Don't hardcode pixel values for spacing — always use a token.

## Where to edit common things

| Want to change... | Where |
|---|---|
| Page title / SEO meta | `<head>` lines ~6–14 |
| Nav links / brand name | `.nav-island` block, around line 320 |
| Hero headline | `.hero-title` h1, around line 345 |
| Hero tagline / stats | `.hero-tagline` p, around line 355 |
| Project tile (title, year, role, Vimeo ID) | `.tile` articles in `.work` grid, around lines 400–470 |
| Reel timing label | `.reel-frame .corner-tl` + `.corner-br`, around line 490 |
| About bio | `.bio .copy` paragraphs, around line 520 |
| Spec cells (Role/Languages/Format/Based) | `.spec-cell` blocks, around line 545 |
| Featured client tile | `.c-feature .inside`, around line 580 |
| Client logos | `.c-logo` blocks + swap files in `assets/` |
| Contact statement | `.statement` div, around line 620 |
| Contact email / social links | `.ccard` anchors, around lines 630–680 |
| Color tokens | `:root` at top of `<style>`, around line 17 |

## Open TODOs (carried forward from original redesign)

1. **Real email**: currently placeholder `hello@jessiejuarez.com` — needs to be his actual booking email. Lives in 3 places: contact card, footer, mailto link.
2. **Identify `logo-extra.png`**: the 4th client logo. Was unnamed in the source Webflow. Update `alt` text and file name.
3. **Optional**: add per-project case-study pages (currently every project opens a Vimeo lightbox; expanded pages would give SEO surface).
4. **Optional**: real Calendly / booking widget instead of plain mailto.

## When Jessie asks for changes — patterns

### "Update [a project]"
Edit the matching `<article class="tile t-*">` block. Keep the structure intact (bezel > bezel-inner > frame > scrim > corner-tag > info). Don't remove `data-vimeo` — it powers the lightbox.

### "Add a new project"
Duplicate one of the four existing tiles. Pick the aspect ratio class (`.r-wide` / `.r-tall` / `.r-square` / `.r-cinema`) and the grid span (`.t-name` styles, or add a new one). Add the new thumb to `assets/`. Keep the bento asymmetric.

### "Swap the reel"
Replace `assets/reel.mp4` and `assets/reel-poster.jpg`. Keep the same filenames. The `<video>` tag at the hero AND inside the reel feature both reference these.

### "Change colors"
Edit the OKLCH values in `:root`. Use OKLCH only (not HSL, not hex). Keep `--gold` reserved for project names.

### "Deploy this"
Three good options:
- **Netlify Drop**: drag the folder onto netlify.com/drop — live in 30 seconds, free SSL, jessiejuarez.netlify.app
- **Vercel**: `npx vercel` in this folder
- **GitHub Pages**: push to a `gh-pages` branch
- **Existing Webflow**: paste the HTML into a Webflow custom code embed (replaces the current page)

For a custom domain (jessiejuarez.com), point DNS at the host.

## Things NOT to do

- Don't add a build step / package.json. This is intentionally a single file.
- Don't add jQuery, React, GSAP, Bootstrap, Tailwind, or any framework. Vanilla works.
- Don't introduce additional fonts.
- Don't add `border-left` accent stripes or gradient text.
- Don't extend gold beyond the four project-name italics.
- Don't make the layout symmetric. The asymmetric bento is the design.
- Don't add tracking pixels / analytics without asking Jessie first.
