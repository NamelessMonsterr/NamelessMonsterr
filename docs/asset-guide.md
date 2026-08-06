# Asset Guide

Every file in `assets/`, what it's for, and which README section uses it. If you're
adding a new asset, put it in the right category below and update this table — an
asset library nobody can navigate isn't actually reusable.

## Hero / boot

| File | Purpose |
|---|---|
| `hero.svg` | Full-width hero — avatar frame, callsign, ambient particles |
| `terminal.svg` | Primary boot sequence (6-line log → access granted) |
| `boot.svg` | Compact circular boot-progress loader (BOOTING → READY) |

## AI / cognition

| File | Purpose |
|---|---|
| `core.svg` | Idle-state AI core — pulsing center, 3 orbiting rings |
| `ai-core.svg` | Request pipeline: Python → FastAPI → LLMs → RAG → Backend → Deploy |
| `brain.svg` | Neural cognition map — two hemispheres, cross-firing synapses |
| `knowledge-graph.svg` | Node graph linking skills, tools, and domains |
| `orbits.svg` | Layered system-architecture rings (core → service → gateway → client) |
| `network.svg` | Service-mesh topology (API gateway, vector store, LLM, auth, DB, client) |

## Status / telemetry

| File | Purpose |
|---|---|
| `dashboard.svg` | 3-panel mission-control dashboard (uptime ring, throughput bars, event log) |
| `cpu.svg` | Standalone CPU load monitor with waveform |
| `memory.svg` | Standalone memory-allocation block grid |
| `radar.svg` | Circular radar sweep — "what I'm tracking" |
| `scanner.svg` | Linear scan-beam over a repo-index grid |
| `galaxy.svg` | Ambient starfield background panel |
| `analytics.svg` | Hand-drawn line/area chart + language-split donut |

## Project database

| File | Purpose |
|---|---|
| `projects.svg` | Terminal-style index of all repos (name / status / stack / type) |
| `healthx.svg` | Full flagship dashboard for HealthX (architecture + features + stack) |
| `mission.svg` | Mission-statement holographic panel |
| `projects/healthx.svg` | Compact project card — paired grid view |
| `projects/faultseeker.svg` | Compact project card |
| `projects/setrs.svg` | Compact project card |
| `projects/deepdive.svg` | Compact project card |

## Skill matrix

| File | Purpose |
|---|---|
| `skills.svg` | Animated proficiency bars |
| `icons/icon-python.svg`, `icon-fastapi.svg`, `icon-react.svg`, `icon-brain.svg`, `icon-git.svg`, `icon-terminal.svg` | 6-icon stack set |

## Timeline / logs

| File | Purpose |
|---|---|
| `timeline.svg` | Execution-log build timeline |
| `terminal-diagnostics.svg` | Self-check terminal screen (7/7 checks) |
| `console.svg` | Interactive-feeling AI console (whoami / status / philosophy) |
| `terminal-deploy.svg` | CI/CD deploy-log terminal screen |
| `terminal-access.svg` | Encrypted access-log terminal screen |

## Section labels (14 — floating nav markers)

`labels/label-*.svg` — one per section (`boot`, `core`, `mission`, `status`,
`experiments`, `projects`, `skills`, `architecture`, `knowledge`, `research`,
`analytics`, `terminal`, `opensource`, `contact`). Replaces the old
`` `> 01 // TITLE` `` Markdown headings: a thin HUD hairline with corner ticks
and a pulsing status dot on each end, centered title text, no numbering. Text
color cycles cyan → violet → emerald across the sequence. Each sits directly
under an `<a id="slug"></a>` anchor so the quick-nav bar at the top of
`README.md` can link to it — see "Navigation" below before adding a 15th.

## Navigation

The quick-nav bar under the hero is plain Markdown (`<a href="#slug"><code>` —
not an image), because SVG `<a>` links don't work once the file is loaded via
`<img>` — GitHub's `<img>`-embedded SVGs are inert, no click-through. Each nav
item points at an `<a id="slug"></a>` anchor placed right before that
section's label image. If you rename a section, update both the anchor `id`
and the nav bar `href` together or the link silently breaks (no 404, it just
scrolls nowhere).

## Dividers (10 — no motif repeats back-to-back)

`divider-circuit.svg` · `divider-wave.svg` · `divider-hex.svg` · `divider-dna.svg` ·
`divider-laser.svg` · `divider-hologram.svg` · `divider-galaxy.svg` · `divider-matrix.svg` ·
`divider-scanlines.svg` · `divider-energy.svg`

## Closing

| File | Purpose |
|---|---|
| `footer.svg` | Closing banner with mission quote |
| `easter-egg.svg` | Real 7-character binary-encoded ASCII message (not decorative gibberish) |

## Known GitHub-specific limitations (read before "fixing" something that isn't broken)

- **Every root `<svg>` needs `xmlns="http://www.w3.org/2000/svg"`.** A file missing
  it is still valid enough to inline into an HTML page (the HTML parser assigns
  the SVG namespace for you), but these files are fetched standalone via
  `<img src="assets/x.svg">` — no host document to inherit a namespace from — so
  without an explicit `xmlns` they can fail to render at all. Ten files shipped
  without it (`core`, `cpu`, `memory`, `projects`, `orbits`, `knowledge-graph`,
  `galaxy`, `scanner`, `boot`, `healthx`) and were patched. If a new SVG "does
  nothing" when referenced via `<img>` but opens fine when pasted inline, check
  this first.
- **Gradients on zero-area shapes need `gradientUnits="userSpaceOnUse"`.** A
  `<linearGradient>` in the default `objectBoundingBox` mode is defined relative
  to the bounding box of the shape it's painted on — a horizontal `<line>` has
  zero height, so that box is degenerate and some renderers just don't paint
  the gradient. Give it explicit `userSpaceOnUse` coordinates instead (see
  `labels/label-*.svg` hairlines for the pattern).
- **Animations restart on every view**, not just first load — GitHub re-fetches `<img>`
  SVGs and SMIL runs from t=0 each time. There's no way to persist animation state
  across a scroll or repo visit; this is a platform constraint, not a bug in these files.
- **`prefers-reduced-motion` cannot be honored.** SMIL has no standard media-query hook,
  and GitHub strips `<style>` from inline README HTML where a `@media` block could
  otherwise live. The honest mitigation used here: keep motion ambient/subtle rather than
  jarring, and keep narrative sequences short (3.5–5s) so they resolve to a static end
  state quickly rather than looping forever.
- **`role="img"` + `aria-label`** is set on every new SVG in this batch so screen readers
  get a description instead of silence; GitHub's renderer passes these through untouched
  since it treats the SVG as an opaque image resource. Earlier files in this repo
  (`hero.svg`, `terminal.svg`, etc.) predate this pass — see the self-review for what's
  not yet backfilled.
