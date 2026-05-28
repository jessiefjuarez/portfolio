# START HERE — Jessie

This folder is your new portfolio site. You can update it yourself using Claude Code (an AI coding assistant). No web-dev background required.

## 1. See the site

Double-click `index.html`. It opens in your browser. That's the site.

## 2. Set up Claude Code (one time, ~5 min)

1. Download from **https://claude.com/code** (Mac, free tier available)
2. Open Terminal (Spotlight → "Terminal")
3. Drag this folder onto Terminal so it shows the path, then type `cd ` (with a space) BEFORE the path. Hit return.
4. Type `claude` and hit return.

That's it. Claude now has full context for your site — there's a `CLAUDE.md` in this folder that briefs it on the design rules, fonts, colors, and where to edit things.

## 3. Things you can ask Claude to do

Just type these in plain English. Examples:

| You say | What happens |
|---|---|
| "Update my email to jessie@hisrealdomain.com" | Swaps the email in 3 places (contact card, footer, mailto link) |
| "Add a new project called 'Sunshine' for Adobe — Vimeo ID 123456789, 2026, brand film" | Adds a new tile to the work grid using the right structure |
| "Replace the Pink Saves thumbnail with the new one I just put in assets/" | Updates the image source |
| "Change the hero headline to say 'Jessie Juarez — DP, Editor, Director'" | Edits the h1 |
| "Make the page load faster on mobile" | Audits + suggests optimizations |
| "Deploy this to Netlify" | Walks you through it step-by-step |
| "Swap the reel for a new file called new-reel.mp4 I just added" | Updates the video sources |
| "Change the gold accent to a different color" | Tweaks the OKLCH value in one place |

Claude reads `CLAUDE.md` automatically so it already knows:
- Your name, role, location, bio
- The four projects on the site, their Vimeo IDs, their roles
- The design system (fonts, colors, spacing)
- What's a banned change (don't introduce Inter, don't break the bento, etc.)

## 4. Hosting it (publish to the internet)

Easiest path — **Netlify Drop** (free, 30 seconds):

1. Go to https://app.netlify.com/drop
2. Drag THIS WHOLE FOLDER onto the page
3. You'll get a URL like `jessie-juarez-portfolio.netlify.app` immediately
4. To use `jessiejuarez.com`, point your domain there in Netlify settings

Alternatively, ask Claude: "Deploy this to Netlify" and it'll walk you through.

## 5. What I still need from you

Three quick things to finish:

1. **Your real booking email** (currently `hello@jessiejuarez.com` is a placeholder)
2. **Tell me what brand `logo-extra.png` is** (the 4th client logo — I couldn't ID it from your current site)
3. **Decide hosting**: keep using Webflow for the domain, or move to Netlify/Vercel (cheaper, faster, more control)

## What's in this folder

- `index.html` — the whole site
- `assets/` — your videos, photos, logos
- `CLAUDE.md` — instructions for Claude (don't delete, don't edit unless you know what you're doing)
- `README.md` — original notes on what changed from the old Webflow site
- `START-HERE.md` — this file

## If something breaks

- Made a change you don't like? Ask Claude: "undo my last change"
- Site looks broken in browser? Ask Claude: "the site isn't rendering right, can you fix it"
- Got an idea for a new section? Just describe it. Claude will ask follow-up questions and build it.

The whole point: you don't need to know HTML. Describe what you want. Claude does it.
