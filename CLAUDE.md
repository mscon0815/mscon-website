# CLAUDE.md — MSCon Website

## Project
Single-page consulting website for MSCon (IT-Architektur & Digitale Transformation).
Owner: Mark Schultes — schultes@mscon.one
Live: mscon.one (production) · staging.mscon.one (staging)
Repo: github.com/mscon0815/mscon-website

## Architecture
Single HTML file — no build tools, no framework.
- `index.html` — entire site (HTML + CSS + JS)
- `assets/logo.png` — logo (not yet in repo)
- Hosted via Cloudflare Pages: `main` → production, `staging` → preview

## Git Workflow
- Work on `staging` branch for all changes
- Test locally with Live Server before pushing
- Merge to `main` only when confirmed working on staging.mscon.one
- Commit style: short, lowercase, no Co-Authored-By line

## Planning & Implementation Guidelines
Consult `C:\Users\mark_\everything-claude-code` for planning strategies,
implementation patterns, and workflow guidelines before tackling complex features.
Key files: `the-shortform-guide.md`, `the-longform-guide.md`, `the-security-guide.md`

## Key Dependencies (CDN)
- Three.js r128 (galaxy background)
- Google Fonts: Fraunces, DM Sans, JetBrains Mono

## CI Colors
- Gold: `#E8A030` (class `.cg`)
- Red:  `#9B2335` (class `.cr2`)
- Olive: `#7A8C3A` (class `.co`)

## Formspree
Form ID: `xvzwgbzg` — sends to schultes@mscon.one

## Open Items
- Logo: save `assets/logo.png` to repo
- Impressum & Datenschutz: footer links point to `#` (legal requirement for DE)
