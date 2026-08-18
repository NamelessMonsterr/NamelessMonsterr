# Design System — H.A.M.M.A.D AI Operating System

This is the single source of truth for the profile's visual language. If you add a new
asset, pull values from here rather than inventing new ones — that's what keeps 38+
independent SVG files feeling like one product instead of a folder of experiments.

## Color tokens

| Token | Hex | Use |
|---|---|---|
| `--bg-base` | `#0D1117` | Card / panel background |
| `--bg-deep` | `#080B10` | Terminal / recessed surfaces |
| `--bg-raised` | `#131C28` | Panel-on-panel (dashboard tiles) |
| `--border-muted` | `#1B2733` | Default hairline borders, chart gridlines |
| `--primary-cyan` | `#00F5FF` | Primary accent — status-good, links, primary data series |
| `--secondary-violet` | `#8B5CF6` | Secondary accent — AI/cognition elements, secondary data series |
| `--accent-emerald` | `#00FF99` | Tertiary accent — "active/live" state, success, tertiary data series |
| `--text-primary` | `#E8F1F2` | Headlines, primary values |
| `--text-secondary` | `#C9D1D9` | Body copy inside panels |
| `--text-muted` | `#8FA3B0` | Captions, axis labels |
| `--text-faint` | `#5B6B7A` | Section eyebrows, timestamps |
| `--text-ghost` | `#3D4A57` | Disclaimers, fine print |

Rule: **never introduce a fourth hue.** Every accent in the system is cyan, violet, or
emerald — status/semantic color (amber `#FFBD2E` for "warn", red `#FF5F56` only for
literal window-chrome dots) is the only exception, and it's used sparingly.

## Typography

- **Everything is monospace** — `Consolas, 'Courier New', monospace`. This is the one
  typographic decision that does the most to make the profile feel like an OS rather
  than a document. Don't mix in a sans-serif for "readability" — it breaks the illusion.
- Section eyebrows (`SYSTEM.PROFILE`, `AI CORE // SIGNAL PATH`) are set in
  `letter-spacing: 3–6px`, `font-size: 11–13px`, `--text-faint`, and prefixed with
  `` `> ` `` in the README headings to read as a command prompt.
- Headline weight is 700; body/label weight is 400–500. No italics anywhere.

## Spacing & grid

- SVG canvases standardize on three widths: **1200×H** (full-bleed sections), **600×H**
  (half-width paired panels), **64×64** (icons).
- Internal panel padding is consistently **20–26px** from the card edge.
- Section-to-section rhythm in the README: `divider → floating label → content`.
  No section skips the divider — it's the equivalent of vertical spacing in a real
  design system, just implemented as an asset because Markdown has no margin control.

## Motion principles

- **Duration bands:** micro-interactions (blinking cursor, status dot) run 1–1.6s;
  ambient motion (particle drift, glow pulse) runs 2.2–4s; narrative reveals (boot
  sequences, chart draw-in) run 3.5–5s total and use `begin` offsets to stagger, never
  a single simultaneous fade.
- **Easing:** SMIL here uses linear/default timing almost everywhere — GitHub's SVG
  renderer is inconsistent about custom `calcMode`/keySplines support, so the system
  intentionally favors simple `values` lists over hand-tuned easing curves. This is a
  constraint decision, not an oversight.
- **Repetition:** ambient loops use `repeatCount="indefinite"`; narrative sequences use
  `fill="freeze"` and do not repeat, so a returning visitor's second scroll-past doesn't
  re-trigger a boot animation mid-view (SVG `<img>` animations do restart per-view, which
  is a known GitHub limitation — see `docs/asset-guide.md`).

## Components

- **Card**: `rx=14`, 1.4–1.6px border, gradient stroke or solid `--border-muted`,
  optional radial glow at low opacity in a corner.
- **Status dot**: 4–5px circle, semantic color, `opacity` pulse 1↔0.3 on a 1.3–1.4s loop.
- **Pill/tag**: `rx=11`, transparent fill, 1px stroke in one of the three accents, 10px
  monospace label.
- **Progress bar**: `rx` = half of height, `--bg-raised`-on-`--border-muted` track,
  accent-gradient fill animated via `width`.
- **Divider**: full-bleed 1200px-wide SVG, 24–50px tall, one distinct motif each — see
  the divider catalogue in `docs/asset-guide.md`. Never reuse a motif back-to-back.
- **Section label**: 460×58 SVG, centered, ~380px display width. A thin hairline
  breaks into two corner ticks with a pulsing status dot on each end, title
  text floating in the gap — no numbering, no `//` comment styling. Replaces
  Markdown `##` headings so the page reads as a HUD, not a doc. See
  `labels/label-*.svg` and the "Section labels" entry in `docs/asset-guide.md`.
  The closing section (`easter-egg.svg` → `footer.svg`) intentionally has no
  label — the page should quiet down at the end, not keep announcing itself.

## Icons

6-icon set at 64×64, `rx=14` card frame, single accent stroke, one small animated detail
per icon (never more than one moving part — icons are punctuation, not scenes).

## What this system deliberately does not do

- No drop shadows (GitHub's `<img>` embedding can clip filter regions unpredictably —
  glow is done with a blurred radial-gradient shape instead of `feDropShadow`).
- No text set below 10px (illegible at GitHub's default README image scaling on mobile).
- No animation that depends on `<script>`, hover, or click — SMIL-only, because that's
  what survives GitHub's sanitizer when the SVG is referenced via `<img>`.
