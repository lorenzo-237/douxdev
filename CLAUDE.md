# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static landing page for douxdev.fr — a single self-contained `index.html` (inline CSS + vanilla JS, no build step, no dependencies, no tests). It lists the author's small web apps as "ice-cream flavours" (e.g. Whiteboard → wb.douxdev.fr). All user-facing copy is in French and uses an ice-cream / strawberry metaphor throughout (parfums, cornet, sucre, vanille/chocolat); keep new copy in that voice and in French. Code comments are also in French.

## Running

Open `index.html` directly in a browser, or serve the folder (e.g. `npx serve .` / `python -m http.server`). The only external resource is Google Fonts (Archivo Black, Space Mono).

## Architecture notes

- **Theming**: two themes, `vanille` (light) and `chocolat` (dark), set via `data-theme` on `<html>`. Theme-dependent colors are CSS custom properties (`--bg`, `--ink`, `--paper`, `--soft`) defined per `[data-theme]`; brand colors (`--fraise`, `--cornet`, `--pistache`, `--on-fraise`) are theme-independent. A small inline script in `<head>` applies the saved theme from `localStorage` key `douxdev-theme` before paint to avoid a flash; the footer script syncs the toggle buttons' `aria-pressed`. Use the variables rather than hard-coded colors for anything that should flip with the theme (some elements — hero chip, contact box, `.btn-*` — intentionally hard-code colors because they sit on fixed-color backgrounds).
- **Sticky top bar**: marquee + "sugar gauge" scroll-progress bar (`role="progressbar"`, updated via rAF-throttled scroll handler).
- **Scroll reveal**: elements with class `.reveal` fade in via IntersectionObserver; they become visible immediately when the observer is unavailable or `prefers-reduced-motion` is set.
- **Reduced motion**: all decorative animations (marquee, cone bob, drips, reveal, sprinkles on cone click) are disabled under `prefers-reduced-motion`; preserve this for any new animation.
- **Tout doux liste** (todo list, receipt-styled section before the footer): each `<li>` has a `data-etat` status — `frigo` (not started), `fond` (in progress), `deguste` (done, struck through), `volonte` (ongoing/endless) — plus an invented flavour name and a scoop color via `--c`. On scroll it "prints": JS adds `.line` + `--i` to each row and `.is-printing` to the ticket (staggered CSS animations, statuses "stamped"), and the total rolls numbers before landing on ∞. Line spacing is the single `--vitesse-ticket` variable (in ms — the JS reads it to time the counter); the full sequence and tuning knobs are documented in the CSS comment above `.ticket .line`.
- **Adding a project**: duplicate the `<a class="card reveal">` block in the "La carte des parfums" section (update link, `PARFUM N°xx`, title, description). The dashed `.card.soon` placeholder is meant to be removed once there's a second real project.
