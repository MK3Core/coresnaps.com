# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

It is also a deliberate example of how to write an effective AI context file. Alex Core is a Product Manager who is intentionally expanding into design and engineering, not to become a full-stack developer, but to close the gap that exists in most product triads: the PM who can only describe what they want, not participate in how it gets made. Working directly in HTML, CSS, and agentic AI tooling (Claude Code) is part of that practice. A well-written CLAUDE.md is a demonstration of that thinking: not just a README, but a persistent mental model handed to an AI agent at the start of every session. It should capture decisions already made, patterns to preserve, and the reasoning behind non-obvious choices so the agent can make judgment calls that align with the project's intent without re-litigating resolved questions. This file is written with that goal in mind.

---

## Project

Static HTML/CSS personal portfolio for coresnaps.com, hosted on GitHub Pages. No build system, no JavaScript, no dependencies. Just `index.html`, `styles.css`, `about.css`, and `favicon.svg`.

Changes go live by pushing to `main`. The `CNAME` file pins the custom domain.

## Architecture

`index.html` is the single landing page and the only fully-built page. The Ethos chapter opens as a CSS-only slide-up overlay triggered by the `#about` URL hash (no JavaScript), styled in `about.css`. The remaining chapter pages (`work.html`, `photography.html`, `projects.html`) do not yet exist. They are nav placeholders.

All styling lives in `styles.css`, with overlay-specific styles in `about.css`. No component system. Layout and theming use CSS custom properties defined in `:root`.

## Design system

- **Palette:** near-black background (`#0a0a0a`), off-white text (`#e8e6e1`), bronze accent (`#C4973A` / `--bronze`), bright bronze (`#d9ad4b` / `--bronze-bright`)
- **Fonts:** Geist (sans) and Geist Mono, loaded from Google Fonts; fall back to system fonts
- **Numbering:** sections use `[XX]` bracket notation rendered in `var(--font-mono)`; brackets themselves use `--muted`, numbers use `--bronze`
- **No JavaScript:** all interactivity is pure CSS: hover effects, the Ethos overlay, tooltips, the skull glitch animation on dead social links
- **Tone:** clean, minimal, intentional. No decoration for decoration's sake. Every element should justify its presence

## Design philosophy (owner-stated)

These are Alex's stated principles for this project. Preserve them when making suggestions or changes:

- **Intent over precision:** get the direction right first; refinement follows
- **Outcomes over process:** solve the actual problem, not the process of solving it
- **Form and function are not in competition:** the best result is where they resolve together, technically sharper for the user and still a delight to use
- **Simple over complex:** prefer beautiful simplicity over complex power; complexity should only appear where it earns its place
- **No AI-slop aesthetics:** avoid generic patterns, clichéd copy, and design that looks like it came from a template. Writing should sound like a person, not a pitch deck. No em dashes.

## Banner pile — how it works

The chapter navigation banners (`[01] Ethos`, `[02] Professional`, etc.) are stacked as a CSS-only overlapping pile:

- Cards use `margin-bottom: calc(-1 * (card-height - 80px))` to overlap, showing only an 80px tab of each lower card
- `[01]` has the highest `z-index` (top of pile), `[04]` the lowest
- **The hover dead-zone problem:** when cards overlap via z-index, hovering a lower card hides all cards above it, making them impossible to hover. This was solved without JavaScript using invisible overlay links (`.pile-zone`) absolutely positioned over the pile
- Each `.pile-zone` is a transparent `<a>` tag covering an equal-height slice of the total pile (`--zone-h: (card-height + 240px) / 4`), with `z-index: 20` above all cards
- Cards have `pointer-events: none`; zones are the actual navigation links
- CSS sibling combinator (`~`) fires all hover effects on the correct card: `pile-zone[data-zone="02"]:hover ~ .chapter[data-ch="02"]`
- Zones divide the pile into **four equal Y-position bands** so mouse position in the pile always maps to the same card, regardless of visual stacking

Do not break this pattern when editing the banner section. If card height or tab size changes, update both the card `margin-bottom` calc and the `--zone-h` variable in `.chapters` together.

## Ethos overlay

The `#about` overlay is the built-out chapter. Key details:

- Triggered by `href="#about"` (hash navigation), closed by `href="#"` or clicking the backdrop
- The sheet panel is constrained to `var(--maxw)` (1080px), centered, and slides up 90vh. It is a floating panel, not a full-width takeover.
- The hero paragraph is the owner's mission statement / ethos. Treat it as intentional and precise copy. Do not rewrite it without being asked.
- The `[03] In my own words` section is a lorem ipsum placeholder for future long-form prose

## Conventions

- Bronze is used for emphasis and active states. Use it sparingly and deliberately.
- The `coresnaps.com` wordmark in the hero is intentionally small (`clamp(22px, 2.6vw, 32px)`). It is a wordmark, not a headline.
- When adding new chapter pages, match `:root` variables, font stack, and `[XX]` numbering from `index.html`
- Commit messages should be descriptive; use the imperative mood
