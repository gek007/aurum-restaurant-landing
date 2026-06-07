# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file static landing page for **Aurum**, a luxury fine dining restaurant. All HTML, CSS, and JavaScript live in `index.html`. There are no build tools, bundlers, frameworks, or npm dependencies — everything ships as-is to the browser.

## Running locally

```bash
npx serve .
```

Then open `http://localhost:3000` in a browser. No install step required.

## Architecture

**`index.html`** — the entire site in one file, structured in order:

1. `<head>` — meta tags, Google Fonts preconnect (`Cormorant Garamond` display, `Inter` body), inline SVG favicon
2. `<style>` — all CSS as a single `<style>` block with CSS custom properties defined on `:root`
3. `<body>` — semantic HTML sections: nav, hero, story, rooms, menu, press, reserve, footer
4. `<script>` — all JS at the bottom of `<body>`, no external dependencies

**`assets/`** — images in both WebP (primary) and JPG (fallback) formats. The `<picture>` element pattern with `<source type="image/webp">` + `<img>` fallback is used throughout.

## Design tokens (CSS custom properties)

Defined once on `:root` and used everywhere — do not hardcode colors or timing values:

| Token | Value | Purpose |
|---|---|---|
| `--gold` | `#C9A84C` | Primary accent, CTAs |
| `--champagne` | `#E8DCC8` | Body text |
| `--charcoal` | `#181614` | Background |
| `--ff-display` | Cormorant Garamond | Headings |
| `--ff-body` | Inter | Body / UI text |
| `--ease-out` | `cubic-bezier(0.22, 1, 0.36, 1)` | Standard easing |

## Key interactive features

- **Ken Burns hero slideshow** — CSS `@keyframes` pan/zoom on `assets/hero-dining.webp`, auto-advancing
- **Room card deck** — click-to-expand panels (`#rooms` section) wired via JS event delegation
- **Tabbed menu** — `.menu-tab` buttons toggle `aria-selected` and show/hide menu category panels
- **Reservation form** — inline validation with error/success states, no backend (front-end only)
- **Scroll-triggered nav** — `IntersectionObserver` or scroll listener adds `.scrolled` to `<nav>` for backdrop-blur effect

## Accessibility conventions

- `prefers-reduced-motion` media query wraps all CSS animations — always maintain this when adding new animations
- Keyboard navigation supported throughout; interactive non-button elements get `tabindex="0"` and `keydown` handlers
- Skip-link at top of `<body>` for keyboard users
- `aria-*` attributes on tabs, expanded panels, and nav toggle

## Image handling

Always provide both formats when adding new images:
```html
<picture>
  <source srcset="assets/name.webp" type="image/webp" />
  <img src="assets/name.jpg" alt="..." loading="lazy" />
</picture>
```

Hero image uses `rel="preload"` in `<head>` — add this for any above-the-fold image.
