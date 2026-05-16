# coresnaps.com

Personal portfolio for [Alex Core](https://coresnaps.com) — engineer, photographer, motorsport enthusiast.

Live at **[coresnaps.com](https://coresnaps.com)** · Hosted on GitHub Pages.

---

## What this is

A hand-rolled static site. No build system, no framework, no JavaScript — just HTML and CSS doing everything.

The design intent is minimal and deliberate: dark palette, bronze accents, Geist type. Every element has a reason to be there.

## How it's built

| Thing | How |
|---|---|
| Hosting | GitHub Pages, custom domain via `CNAME` |
| Fonts | Geist + Geist Mono via Google Fonts |
| Interactivity | Pure CSS — no JS anywhere |
| Chapter overlay | CSS `:target` hash trigger (`#about`) |
| Dead social glitch | CSS `@keyframes` + `filter: drop-shadow()` |

## The pile nav

The chapter banners (`[01] Ethos`, `[02] Professional`, etc.) stack as an overlapping pile using negative margins and `z-index`. Hovering deals a card out from the stack.

The tricky part: when a lower card lifts via `translateY`, it physically covers the hover tabs of cards above it — making them unreachable. This is solved with invisible overlay `<a>` elements (`.pile-zone`) that divide the pile into equal Y-position bands. Your mouse position in the pile always maps to the right card, regardless of visual stacking. Cards themselves have `pointer-events: none`; the zones are the actual navigation surface.

No JavaScript. Pure CSS sibling combinator (`~`) fires all the hover effects.

## Structure

```
index.html      — landing page + Ethos overlay (inline)
styles.css      — all site-wide styles
about.css       — overlay-specific styles
favicon.svg     — inline SVG mark
CLAUDE.md       — AI context file (project decisions, patterns, philosophy)
```

The remaining chapter pages (`work.html`, `photography.html`, `projects.html`) are nav placeholders — not yet built.

## CLAUDE.md

The repo includes a `CLAUDE.md` — a persistent context file for [Claude Code](https://claude.ai/code). It captures design decisions, the pile mechanic, conventions, and the reasoning behind non-obvious choices so an AI agent can make judgment calls that stay true to the project without re-litigating resolved questions.

It's also an intentional demonstration of how to write an effective AI context file. A good `CLAUDE.md` isn't a README — it's a mental model handed to an agent at the start of every session.

---

&copy; Core · 2026
