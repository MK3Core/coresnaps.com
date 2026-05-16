# coresnaps.com — Product Requirement Document

**Owner:** Alex Core  
**Status:** Active development  
**Live:** [coresnaps.com](https://coresnaps.com) · [GitHub](https://github.com/MK3Core/coresnaps.com)

---

## Problem Statement

Most personal portfolio sites look the same. They are assembled from templates, packed with buzzwords, and designed to appeal to everyone — which means they resonate with no one. They also carry unnecessary weight: JavaScript frameworks, build pipelines, and third-party dependencies for sites that are, at their core, just pages of content.

The problem this project solves is twofold:

1. **Identity** — There is no site that represents Alex Core as a person, not a resume. A place that reflects actual taste, values, and range — across engineering, product, photography, and motorsport — without sounding like a pitch deck.
2. **Ownership** — Most personal sites are rented from platforms. This one is fully owned, self-hosted in spirit (GitHub Pages + custom domain), and free of surveillance capitalism (no trackers, no analytics, no social embeds).

---

## Goals

| Goal | Description |
|---|---|
| Authentic presence | A site that sounds and feels like a person, not a brand |
| Zero dependencies | No frameworks, no build step, no JavaScript |
| Craft as signal | The technical implementation itself demonstrates taste and capability |
| Long-term maintainability | Push to `main`, it's live. No toolchain to rot. |
| Privacy by default | No third-party tracking, no embedded scripts beyond fonts |

---

## Success Metrics

This is a personal site, not a product with a funnel. Success looks like:

- Someone visits and gets an accurate read on who Alex is within the first 10 seconds
- The site holds up to scrutiny from designers, engineers, and product people — the three audiences most likely to look at the source
- Zero JavaScript in the final output (verified by browser devtools)
- Passes Core Web Vitals without optimization heroics — a natural result of serving static HTML/CSS

---

## Users

**Primary:** Anyone evaluating Alex professionally — hiring managers, collaborators, peers. They are likely technical or design-literate. They will notice if the site looks like every other portfolio.

**Secondary:** Alex himself. The site should be a pleasure to maintain and a reference point for his own thinking about craft.

---

## Features

### `[F-01]` Landing page

The single entry point. Contains the wordmark, chapter navigation, and footer socials. No hero paragraph, no tagline, no calls to action — the design does the work.

- Wordmark (`coresnaps.com`) is intentionally small — a mark, not a headline
- No copy beyond navigation labels and social handles
- Page loads with no visible motion unless the user is on a device that supports it (respects `prefers-reduced-motion`)

---

### `[F-02]` Chapter pile navigation

Four content chapters presented as a stacked pile of banner cards. On page load they appear overlapping; hovering deals the relevant card out from the stack.

**Chapters:**

| ID | Title | Status |
|---|---|---|
| `[01]` | Ethos | Built |
| `[02]` | Professional | Placeholder |
| `[03]` | Photography | Placeholder |
| `[04]` | Side Projects | Placeholder |

**Pile mechanics:**

- Cards overlap using negative `margin-bottom`, leaving an 80px visible tab per card
- `[01]` sits on top (highest `z-index`); `[04]` is at the bottom
- Hovering a card lifts it out of the stack with a `translateY` and box-shadow reveal
- Cards above the hovered card shift upward slightly to reinforce the physical metaphor

**The hover dead-zone problem (and how it's solved):**

When a lower card lifts via `translateY`, it physically covers the hover tab of cards above it — making them unreachable. Solved with invisible overlay `<a>` elements (`.pile-zone`) that divide the full pile height into four equal Y-position bands. Mouse position in the pile always maps to the correct card regardless of visual stacking. Cards have `pointer-events: none`; the zones are the actual navigation and interaction surface. Pure CSS — no JavaScript.

---

### `[F-03]` Ethos overlay

The `[01] Ethos` chapter opens as a CSS-only slide-up sheet overlay, triggered by the `#about` URL hash. No JavaScript. Closed by clicking the backdrop or any `href="#"` link.

**Contents:**

| Section | Description |
|---|---|
| `[01]` Hero | Mission statement — Alex's ethos on craft, intent, and form vs. function |
| `[02]` The crossroads | Tile grid: Design, Product, Photography, Motorsport, Security |
| `[03]` In my own words | Long-form prose section (placeholder — to be written) |

**Sheet behavior:**

- Slides up from the bottom, 90vh tall
- Constrained to `1080px` max-width, centered — a floating panel, not a full-screen takeover
- Backdrop blurs and dims the page behind it
- Sticky top nav with `← Close` and section label

---

### `[F-04]` Footer socials

Active and "dead" social links in the footer, rendered as a clean row using `[05.XX]` numbering.

**Active links:**

| ID | Platform | Handle |
|---|---|---|
| `[05.01]` | Signal | @core.911 |
| `[05.02]` | Matrix | @core:chat.coresnaps.com |
| `[05.03]` | Bluesky | @acore.bsky.social |
| `[05.04]` | GitHub | @MK3Core |

**Dead links (Instagram, Facebook, Twitter):**

Hovering a dead social reveals a tooltip with an inline SVG skull and the word "Dead." The skull animates with a chromatic aberration glitch effect using `filter: drop-shadow()` — pure CSS keyframe animation, no JavaScript.

---

### `[F-05]` Accessibility

- All interactive elements are keyboard-navigable
- `aria-label` on every social link and nav item
- `aria-hidden="true"` on decorative elements
- `prefers-reduced-motion` respected across all animations
- Semantic HTML throughout (`<nav>`, `<section>`, `<footer>`, `<main>`)

---

## Technical Constraints

| Constraint | Reason |
|---|---|
| No JavaScript | Intentional constraint — interactivity via pure CSS demonstrates craft; removes an entire class of security and maintenance risk |
| No build system | Push to `main` = deployed. No toolchain to maintain or rot. |
| No npm / node_modules | Zero dependency surface |
| Static HTML/CSS only | GitHub Pages compatible; loads instantly; survives indefinitely |
| Fonts via Google Fonts | Acceptable external dependency; system font fallbacks defined |

---

## Design System

**Palette**

| Token | Value | Use |
|---|---|---|
| `--bg` | `#0a0a0a` | Page background |
| `--fg` | `#e8e6e1` | Primary text |
| `--bronze` | `#C4973A` | Accent, active states, emphasis |
| `--bronze-bright` | `#d9ad4b` | Hover highlight on bronze elements |
| `--muted` | — | Secondary text, bracket glyphs |
| `--border` | — | Card and section borders |

**Typography**

- `Geist` — sans-serif body and UI
- `Geist Mono` — section numbers, labels, monospace details
- `[XX]` bracket notation used throughout for section numbering; brackets in `--muted`, numbers in `--bronze`

**Principles**

- Bronze is used sparingly — emphasis only, not decoration
- Every element must justify its presence
- Simple over complex; complexity earns its place
- Form and function are not in competition — the best results sit where they resolve

---

## Out of Scope

- Analytics or visitor tracking of any kind
- CMS or admin interface
- JavaScript of any kind, including progressive enhancement
- Dark/light mode toggle (dark is the design; there is no light mode)
- Comments, contact forms, or any server-side processing
- Social media embeds

---

## Roadmap

| Chapter | Work remaining |
|---|---|
| `[01]` Ethos | Write the `[03] In my own words` long-form prose section |
| `[02]` Professional | Build `work.html` — career history, roles, highlights |
| `[03]` Photography | Build `photography.html` — gallery, motorsport focus |
| `[04]` Side Projects | Build `projects.html` — self-hosted infra, builds, experiments |

---

## Repository

```
index.html      — landing page + Ethos overlay (inline)
styles.css      — all site-wide styles
about.css       — Ethos overlay styles
favicon.svg     — SVG wordmark/icon
CLAUDE.md       — AI context file (decisions, patterns, philosophy)
CNAME           — custom domain binding for GitHub Pages
```

---

*Hand-rolled on [github](https://github.com/MK3Core/coresnaps.com) &middot; &copy; Core 2026*
