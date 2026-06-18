# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static, single-file company briefing website for VGSI PILE, a Vietnamese PHC (prestressed high-strength concrete) pile manufacturer. The entire application lives in `index.html` — no build step, no dependencies, no framework.

Deployed via GitHub Pages at `https://swnamvgsi-cmd.github.io/vgsi-pile-briefing/`. The `.nojekyll` file prevents GitHub Pages from invoking Jekyll.

## How to Run

Open `index.html` directly in any modern browser — no server needed. Everything works offline.

## Architecture

All content, styles, and logic are co-located in `index.html` (~390 lines):

- **Embedded CSS** (~164 lines): Design system using CSS custom properties (`--navy`, `--blue`, `--sky`, `--ink`, `--muted`, `--line`, `--bg`, `--white`, `--green`, `--orange`, `--red`). Responsive breakpoints at 1100px and 680px.
- **Embedded JavaScript** (~220 lines): Vanilla JS, no frameworks. Reads URL params and `localStorage` for language preference, then calls `render()` to populate `#app`.
- **Data layer**: A single `content` object keyed by language (`en`, `ko`, `vi`) contains all text, KPIs, project references, and news. `galleryAssets` and `sourceLinks` are separate top-level arrays.

### Key functions

| Function | Purpose |
|---|---|
| `render()` | Main function — builds entire DOM from `content[currentLanguage]` |
| `escapeHtml()` | XSS-safe HTML encoding, used on all dynamic string output |
| `bars()` | Renders horizontal bar chart SVG for KPI visualization |
| `classifyProject()` | Infers project category from name via regex (e.g. `/silo/i` → storage) |

### Content update pattern

All visible text is in the `content` object. To update copy, KPIs, project references, or news items, edit the relevant key inside that object for each of the three language variants (`en`, `ko`, `vi`).

## Conventions

- **No external dependencies**: Do not introduce CDN links, npm packages, or build tools.
- **All three languages must stay in sync**: Any content change to one language variant must be reflected in the other two.
- **XSS safety**: Pass user-facing strings through `escapeHtml()` before inserting into the DOM via `innerHTML`.
- **CSS variables over inline styles**: Use the existing custom properties for all colors and spacing.
- **Accessibility**: Maintain ARIA attributes (`role`, `aria-label`, `aria-pressed`) on interactive elements.
