# CLAUDE.md — Claude Code 逆向工程

## Project Overview

Static HTML educational course teaching how Claude Code works internally.
Audience: vibe coders who want to understand the internals to steer AI better.

## Course Structure

- `index.html` — Assembled course (build with `bash build.sh`)
- `styles.css` — Design system (warm white + vermillion)
- `main.js` — Interactive elements (chat, quiz, flow animations)
- `modules/` — Source HTML files (one per module)
- `build.sh` — Assembles `index.html` from modules + base + footer
- `_base.html` / `_footer.html` — Page shell

## Build

```bash
bash build.sh
```

## Deploy Configuration (configured by /setup-deploy)

- Platform: GitHub Pages
- Production URL: https://shaojie66.github.io/claude-code-course/
- Deploy workflow: Push to main branch → GitHub Pages auto-deploys (legacy)
- Deploy status check: https://shaojie66.github.io/claude-code-course/
- Project type: Static HTML course
- Post-deploy: Verify `index.html` loads without errors

## Design System

- Background: #FAF7F2 (warm off-white)
- Accent: #D94F30 (vermillion)
- Fonts: Bricolage Grotesque + DM Sans + JetBrains Mono
- No AI slop patterns
