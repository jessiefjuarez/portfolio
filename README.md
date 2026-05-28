---
title: Jessie Juarez — Site Redesign
type: project
created: 2026-05-27
source: claude
tags: [redesign, videographer, web]
---

# Jessie Juarez — Site Redesign

Scraped `jessiejuarez.com` and rebuilt as a single-page, reel-forward portfolio with editorial-cinema typography. Self-contained HTML + local assets — open `index.html` in any browser.

## Open it

```bash
open "Claude/Inbox/jessie-juarez/index.html"
```

## What was broken on the current site

| # | Issue | Impact |
|---|-------|--------|
| 1 | `<title>Modo - Webflow HTML website template</title>` | Tab + Google search show "Modo template" instead of his name. SEO/credibility hit. |
| 2 | OG image is `Ophen Graph_1x.webp` (template demo) | Anyone sharing his link on socials sees a generic placeholder graphic, not his work. |
| 3 | Capitalization typos throughout: "JEssie Juarez", "YOutube", "ANgeles", "Los ANgeles" | Reads like an unfinished CMS — undermines the "operational precision" claim in the bio. |
| 4 | Project years are out of order: 2026 → 2026 → 2025 → 2024, with *two* 2026 projects clustered | Looks sloppy. Was meant to be reverse-chrono, isn't. |
| 5 | About section layers his real portrait with **two leftover stock-photo template faces** (`pfp-2.avif`, `pfp-3.avif`) | He literally never deleted the template's fake people. They flash in/out via Webflow's slider animation. |
| 6 | Logo strip: Kodak appears twice (filename `KODAK.png` + `KOADK.png` typo'd duplicate) and one logo is named "Web Logos" | Reads as 5 clients when it's really 3. |
| 7 | Empty `<section id="exhibitions">` left in the DOM | Dead template scaffolding. |
| 8 | No contact email anywhere. No form. Only social icons. | Buyers who land here can't book him without leaving the site. |
| 9 | Project tiles link offsite to vimeo.com | Loses the visitor. No control of viewer context, no analytics, no second project view. |
| 10 | Hero is a silent looping background video with **no name overlay, no role, no CTA** | Visitor lands on a 4-second loop of footage with zero context. The reel is the story but it's buried. |
| 11 | Only font: `Inter` (Google) | Generic. A DP's brand IS visual taste — the type has to do work. |
| 12 | No structured data, no `<meta>` for Twitter card preview | Loses share/preview quality. |

## What the redesign does

**Typography.** Pairs `Instrument Serif` (italic display, editorial cinema-feel) with `Inter Tight` (UI/body). One face for personality, one for clarity. Both Google Fonts — free, fast.

**Hero.** Same reel video as background, but now overlaid with his name at display scale, the line `cinematographer & editor`, role tags (LA / Bilingual EN-ES / 6+ years / 21+ TV markets), and a `WATCH REEL` button that scrolls to the full-frame player below.

**Marquee strip.** Editorial italic ticker under the hero lists his roles + Super Bowl LX credit. Cinema-vibe, very low effort to update.

**Selected Work.** Bento grid (7+5 / 5+7) instead of 4 equal squares. Each tile shows year + role + tile title, lifts on hover, and **opens an in-page Vimeo lightbox** instead of redirecting offsite. Click play → modal iframe with autoplay, chromeless. ESC or backdrop click closes.

**Reel feature.** Full-bleed video card with center play button. Click toggles unmute/play. Corner labels: "2025 reel" + "click to unmute".

**About.** His portrait + 4-paragraph bio (italicized key phrases) + a 2×2 spec block (Role · Languages · Specialty · Based). No more template stock faces.

**Selected Clients.** Logo wall as a bordered 4×2 grid. Logos sit in monochrome (white-on-black, 55% opacity, hover to 100%). Deduped to the 4 actual logos + 4 italic text cells for credits without logos (NFL · Super Bowl LX · Spanish Broadcast · 21+ TV Markets).

**Contact.** Big italic statement ("In a world drowning in content, *make something real.*" — kept his original line, fixed grammar) + actual email + Vimeo / YouTube / IG / LinkedIn in a clean grid.

**Meta/SEO.**
- Title: "Jessie Juarez — Director of Photography & Editor, Los Angeles"
- Description: bilingual DP/editor in LA, Kodak/Tito's/EVC/Super Bowl LX credits
- OG image: his reel poster (not a template placeholder)
- Twitter `summary_large_image`

**Motion.** IntersectionObserver reveal-on-scroll (28px translate + opacity, 900ms ease). Respects `prefers-reduced-motion` — kills marquee + background video + reveals for users who opt out.

**Performance.** Single HTML file. Two Google Font families. No GSAP, no jQuery, no Webflow runtime (current site loads ~5 separate Webflow JS bundles). Local assets only.

## What Jessie still needs to provide

1. **Real email address** (currently placeholder `hello@jessiejuarez.com` — swap for his actual booking email)
2. **Identify the 4th client logo** (currently `logo-extra.png` with alt="Client" — unclear what brand that is on his current site)
3. **Confirm "21+ TV markets" and "Super Bowl LX National Spanish Radio Broadcast"** are accurate to repeat in metadata
4. **Optional: longer-form case-study pages** for the top 2 projects (Mickey's Story + La Colombe) — currently every project just opens the Vimeo. Detail pages would let him show stills, role breakdown, gear, deliverables, and stretch SEO surface.
5. **Optional: a contact form or Calendly** if he wants to reduce email back-and-forth.

## Files

```
Claude/Inbox/jessie-juarez/
├── README.md               ← this file
├── index.html              ← the redesign (open in browser)
└── assets/
    ├── reel.mp4            (3.4 MB hero reel)
    ├── reel-poster.jpg
    ├── portrait.jpg
    ├── thumb-mickeys.jpg
    ├── thumb-lacolombe.jpg
    ├── thumb-pinksaves.jpg
    ├── thumb-pledge.jpg
    ├── logo-kodak.png
    ├── logo-titos.png
    ├── logo-evc.png
    └── logo-extra.png      (unidentified — needs naming)
```

## If we want to ship this

Three reasonable paths:

1. **Hand him the HTML.** He uploads to whatever host he uses (Netlify, Vercel, GitHub Pages, even drag-into-Webflow as a custom code embed). Cheapest path. ~1 hour of his time.
2. **Rebuild in Webflow** to match this layout. Keeps his current CMS. ~4 hours of Webflow Designer work.
3. **Skip Webflow entirely.** Move him to a static Astro site (template/CI from your boring-stories-site pattern) — costs less per year, faster, more control. ~1 day of work.

If he's a paying client, option 3 is the best version. If he's a referral or favor, option 1 is fine.
