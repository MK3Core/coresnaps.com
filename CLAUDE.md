# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static HTML/CSS personal portfolio for coresnaps.com, hosted on GitHub Pages. No build system, no JavaScript, no dependencies — just `index.html`, `styles.css`, and `favicon.svg`.

Changes go live by pushing to `main`. The `CNAME` file pins the custom domain.

## Architecture

`index.html` is the landing page. It links to four chapter pages (`about.html`, `work.html`, `photography.html`, `projects.html`) that do not yet exist in the repo — they are placeholders in the nav.

All styling lives in `styles.css`. There is no component system; layout and theming use CSS custom properties defined in `:root`.

## Design system

- **Palette** — near-black background (`#0a0a0a`), off-white text (`#e8e6e1`), bronze accent (`#C4973A` / `--bronze`)
- **Fonts** — Geist (sans) and Geist Mono, loaded from Google Fonts; fall back to system fonts
- **Pattern** — numbered sections use `[XX]` bracket notation in `var(--font-mono)`; hover states reveal bronze highlights and slide-in underlines
- **No JavaScript** — all interactivity (hover tooltips, chapter zoom, skull glitch) is pure CSS

When adding new pages, match the `:root` variables, font stack, and section-numbering convention from `index.html`.
