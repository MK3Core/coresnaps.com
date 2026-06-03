# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

It is also a deliberate example of how to write an effective AI context file. Alex Core is a Product Manager who is intentionally expanding into design and engineering, not to become a full-stack developer, but to close the gap that exists in most product triads: the PM who can only describe what they want, not participate in how it gets made. Working directly in HTML, CSS, and agentic AI tooling (Claude Code) is part of that practice. A well-written CLAUDE.md is a demonstration of that thinking: not just a README, but a persistent mental model handed to an AI agent at the start of every session. It should capture decisions already made, patterns to preserve, and the reasoning behind non-obvious choices so the agent can make judgment calls that align with the project's intent without re-litigating resolved questions. This file is written with that goal in mind.

---

## Project

Static HTML/CSS personal portfolio for coresnaps.com, hosted on GitHub Pages. No build system, no frameworks, no dependencies. Minimal scripting: one small keyboard handler for ESC to close overlays. Core files: `index.html`, `styles.css`, `chapter.css`, `favicon.svg`.

Changes go live by pushing to `main`. The `CNAME` file pins the custom domain.

## Architecture

`index.html` is the single page. All four chapters (`[01] About`, `[02] Professional`, `[03] Photography`, `[04] Side Projects`) open as CSS `:target` overlay panels triggered by hash navigation (`#about`, `#work`, `#photography`, `#projects`). Closed by clicking the backdrop, the close link (`href="#"`), or pressing ESC (handled by a small inline script).

Separate chapter pages also exist (`about.html`, `work.html`, `photography.html`, `projects.html`) but are not linked from the pile — content lives in the overlays in `index.html`.

All styling lives in `styles.css` (site-wide) and `chapter.css` (overlay panels, chapter page layouts, photography grid, lightbox, resume, project cards). No component system. Layout and theming use CSS custom properties defined in `:root`.

## Design system

- **Palette:** near-black background (`#09090b`), off-white text (`#fafaf9`), indigo accent (`#6366f1` / `--accent`), bright indigo (`#818cf8` / `--accent-bright`)
- **Fonts:** Geist (sans) and Geist Mono, loaded from Google Fonts; fall back to system fonts
- **Numbering:** sections use `[XX]` bracket notation rendered in `var(--font-mono)`; brackets themselves use `--muted`, numbers use `--bronze`
- **Minimal scripting:** one inline `<script>` in `index.html` handles ESC key to close overlays/lightbox. All visual interactivity (hover effects, overlays, tooltips, skull glitch, photo lightbox) is pure CSS
- **Tone:** clean, minimal, intentional. No decoration for decoration's sake. Every element should justify its presence

## Design philosophy (owner-stated)

These are Alex's stated principles for this project. Preserve them when making suggestions or changes:

- **Intent over precision:** get the direction right first; refinement follows
- **Outcomes over process:** solve the actual problem, not the process of solving it
- **Form and function are not in competition:** the best result is where they resolve together, technically sharper for the user and still a delight to use
- **Simple over complex:** prefer beautiful simplicity over complex power; complexity should only appear where it earns its place
- **No AI-slop aesthetics:** avoid generic patterns, clichéd copy, and design that looks like it came from a template. Writing should sound like a person, not a pitch deck. No em dashes.

## Banner pile — how it works

The chapter navigation banners (`[01] About`, `[02] Professional`, etc.) are stacked as a CSS-only overlapping pile:

- Cards use `margin-bottom: calc(-1 * (card-height - 80px))` to overlap, showing only an 80px tab of each lower card
- `[01]` has the highest `z-index` (top of pile), `[04]` the lowest
- **The hover dead-zone problem:** when cards overlap via z-index, hovering a lower card hides all cards above it, making them impossible to hover. This was solved without JavaScript using invisible overlay links (`.pile-zone`) absolutely positioned over the pile
- Each `.pile-zone` is a transparent `<a>` tag covering an equal-height slice of the total pile (`--zone-h: (card-height + 240px) / 4`), with `z-index: 20` above all cards
- Cards have `pointer-events: none`; zones are the actual navigation links
- CSS sibling combinator (`~`) fires all hover effects on the correct card: `pile-zone[data-zone="02"]:hover ~ .chapter[data-ch="02"]`
- Zones divide the pile into **four equal Y-position bands** so mouse position in the pile always maps to the same card, regardless of visual stacking

Do not break this pattern when editing the banner section. If card height or tab size changes, update both the card `margin-bottom` calc and the `--zone-h` variable in `.chapters` together.

## Chapter overlays

All four chapters open as CSS `:target` panels. Key details:

- Triggered by hash links (`#about`, `#work`, `#photography`, `#projects`), closed by `href="#"` or ESC
- Panels are `min(900px, 100% - gutter)` wide, `88vh` tall, centered, with a scrollable body and sticky header
- The About bio is intentional and precise copy. Do not rewrite it without being asked.
- Photography uses a 2-column grid; clicking a photo opens a fullscreen CSS lightbox (`#photo-N`); closing returns to `#photography`

## Conventions

- Bronze is used for emphasis and active states. Use it sparingly and deliberately.
- The `coresnaps.com` wordmark in the hero is intentionally small (`clamp(22px, 2.6vw, 32px)`). It is a wordmark, not a headline.
- When adding new chapter pages, match `:root` variables, font stack, and `[XX]` numbering from `index.html`
- Commit messages should be descriptive; use the imperative mood
