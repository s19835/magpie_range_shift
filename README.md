# Handoff: "Where the Magpies Are" — scrollytelling data story

## Overview

A single-page, long-form data story about the Sri Lanka blue magpie (*Urocissa ornata*).
Its argument: the ~90× growth in sighting records between 1990 and 2024 reflects **observer
effort**, not range expansion — the species' centre of gravity moved only a few kilometres in
34 years.

The page is a linear scroll narrative with three interactive/animated data views:

1. A **records-per-bin bar chart** (7 five-year bins, 1990–2024).
2. An **animated KDE density map** painted to `<canvas>`, playable across the seven time bins.
3. A **centroid drift plot** (SVG) showing the balance point of sightings wandering inside a
   16 × 16 km box.

Three generated video loops (hero, subject, mid-page break) and one still carry the atmosphere.

---

## About the design files

The files in this bundle are **design references created in HTML** — prototypes that show the
intended look, motion, and behaviour. They are **not production code to copy directly**.

The task is to **recreate these designs in the target codebase's existing environment**
(React, Vue, Svelte, SwiftUI, native, etc.) using its established patterns, component library,
and data layer. If no environment exists yet, choose the framework most appropriate for the
project and implement the designs there.

Two caveats specific to this build:

- The prototype uses a bespoke template runtime (`support.js`, `<x-dc>`, `<sc-for>`,
  `{{ }}` holes). **Do not port that runtime.** Read the markup as plain HTML + inline styles
  and rebuild with normal components; `<sc-for list="{{ xs }}" as="x">` is a `.map()`,
  `{{ x.foo }}` is an interpolation.
- All styling is **inline**, deliberately. In production, move it to whatever the codebase
  uses (CSS modules, Tailwind, styled-components) — but keep the token values below exact.

## Fidelity

**High-fidelity (hifi).** Colours, type, spacing, motion timing, and interaction states are
final. Recreate pixel-accurately against the design tokens listed below, substituting the
codebase's own primitives where equivalents exist.

---

## Design system

The visuals follow the **Wilderness Design System** (bundled here under `_ds/`). It is a
wildlife-storytelling brand: near-black ink and warm cream surfaces, a single warm gold accent,
serif display type against a geometric sans for UI chrome, full-bleed documentary imagery, and
frosted-glass floating chips over photography.

If the target codebase already has a design system, map to it rather than importing these
tokens verbatim — but preserve the *relationships* (two surfaces, one accent, serif display /
sans UI split).

### Colour tokens

Defined in `_ds/…/tokens/colors.css`, on `:root` and `[data-theme="dark"]`.

| Token | Light | Dark |
| --- | --- | --- |
| `--ink-950` | `#0d1117` | — |
| `--ink-900` | `#12161d` | — |
| `--ink-800` | `#1a1f27` | — |
| `--ink-700` | `#242b35` | — |
| `--ink-600` | `#3a4250` | — |
| `--cream-50` | `#faf8f3` | — |
| `--cream-100` | `#f4f1ea` | — |
| `--cream-200` | `#ece6d8` | — |
| `--cream-300` | `#ddd4bd` | — |
| `--cream-400` | `#c7bb9a` | — |
| `--gold-300 / 400 / 500 / 600` | `#e2cda0` / `#d9bd85` / `#c9a86a` / `#a8823f` | — |
| `--brown-300 / 500 / 700` | `#b08e68` / `#8a6a4a` / `#5c4632` | — |
| `--moss-400 / 500 / 700` | `#7c9184` / `#5c7a6e` / `#3c5249` | — |

Semantic aliases (these are what the design actually references):

| Token | Light | Dark |
| --- | --- | --- |
| `--surface-bg` | `--cream-100` | `--ink-950` |
| `--surface-card` | `#ffffff` | `--ink-800` |
| `--surface-sunken` | `--cream-200` | `--ink-900` |
| `--text-primary` | `--ink-950` | `--cream-50` |
| `--text-secondary` | `rgba(13,17,23,.68)` | `rgba(250,248,243,.72)` |
| `--text-tertiary` | `rgba(13,17,23,.46)` | `rgba(250,248,243,.5)` |
| `--text-accent` | `--brown-700` | `--gold-400` |
| `--border-hairline` | `rgba(13,17,23,.12)` | `rgba(250,248,243,.12)` |
| `--border-strong` | `rgba(13,17,23,.28)` | `rgba(250,248,243,.24)` |
| `--accent-primary` | `--gold-500` | `--gold-400` |
| `--accent-primary-strong` | `--gold-600` | `--gold-300` |

Theme switching is done by setting `data-theme="light" | "dark"` on `document.documentElement`.

### Typography

- **Serif (display, body copy, pull-quotes):** `Ibarra Real Nova`, fallback
  `Georgia, 'Times New Roman', serif`. Weight 400 throughout; true italics used for emphasis.
- **Sans (all UI chrome, eyebrows, labels, data numerals):** `Manrope`, fallback
  `-apple-system, BlinkMacSystemFont, sans-serif`. Weights 500 / 600.
- Both from Google Fonts (`_ds/…/tokens/fonts.css`).

Applied scale in this design:

| Role | Family | Size | Weight | Line-height | Tracking |
| --- | --- | --- | --- | --- | --- |
| Hero h1 | serif | `clamp(38px, 6.4vw, 96px)` | 400 | 1.05 | `-.02em` |
| Section h2 | serif | `clamp(30px, 3.2vw, 46px)` | 400 | 1.14 | `-.02em` |
| Question / statement lead | serif | `clamp(28px, 3.4vw, 48px)` | 400 | 1.28 | `-.02em` |
| Pull-quote | serif italic | `clamp(22px, 3.4vw, 48px)` | 400 | 1.25 | `-.015em` |
| Body copy | serif | 19px (17px in cards) | 400 | 1.7 | 0 |
| Hero deck | serif | `clamp(16px, 1.5vw, 22px)` | 400 | 1.7 | 0 |
| Closing statement | serif | `clamp(24px, 2.8vw, 40px)` | 400 | 1.3 | `-.02em` |
| Eyebrow label | sans, uppercase | 11px | 600 | — | `.16em` |
| Chart numerals | sans | `clamp(9px, 1.5vw, 14px)` | 600 | — | 0 |
| Chart axis labels | sans | `clamp(8px, 1.3vw, 11px)` | 500 | — | 0 |
| Micro caption | sans | 10px | 500–600 | — | `.06em`–`.16em` |
| Button label | sans, uppercase | 11px | 600 | — | `.14em` |

`text-wrap: balance` on headlines, `text-wrap: pretty` on body paragraphs.

### Radius, shadow, motion

| Token | Value |
| --- | --- |
| `--radius-sm` | `6px` |
| `--radius-md` | `12px` |
| `--radius-lg` | `20px` |
| `--radius-xl` | `28px` |
| `--radius-pill` | `999px` |
| `--shadow-sm` | `0 2px 8px rgba(13,17,23,.08)` |
| `--shadow-md` | `0 8px 24px rgba(13,17,23,.14)` |
| `--shadow-lg` | `0 24px 64px rgba(13,17,23,.24)` |
| `--shadow-float` | `0 20px 50px rgba(0,0,0,.35)` |
| `--blur-glass` | `16px` |
| Easing | `cubic-bezier(.4, 0, .2, 1)` everywhere |
| Durations | 150 / 260 / 420ms for UI; 700–1100ms for scroll reveals |

**Glass chip recipe** (used for every floating metadata chip, in both themes):
`background: rgba(13,17,23,.55); backdrop-filter: blur(16px); border-radius: var(--radius-md); box-shadow: var(--shadow-float); color: var(--cream-50)`.

### Spacing rhythm

- Content column: `max-width: 1180px`, horizontal padding `clamp(24px, 6vw, 64px)`.
- Section top padding: `clamp(72px, 9vw, 120px)` (first content section `clamp(80px, 10vw, 132px)`).
- Two-column splits: `grid-template-columns: repeat(auto-fit, minmax(min(100%, 320px), 1fr))`, `gap: 56px`.
- Three-card row: `minmax(min(100%, 260px), 1fr)`, `gap: 24px`.
- Section dividers: `border-top: 1px solid var(--border-hairline)` + `padding-top: 54px`.

---

## Screens / views

There is one screen — a continuous scroll. Sections in document order:

### 1. Hero

- **Purpose:** state the question and the tension in one screen.
- **Layout:** full-viewport `min-height: 100svh` section with `background: var(--ink-950)` and
  `padding: clamp(10px, 1.6vw, 18px)`. Inside sits a single inset frame:
  `border-radius: var(--radius-xl)`, `overflow: hidden`, `min-height: min(640px, 88svh)`.
  This dark-canvas-holding-an-inset-frame composition is a fixed brand motif — keep it in both
  themes.
- **Components:**
  - **Background video**, `object-fit: cover`, autoplaying, muted, looping, `playsinline`.
    Wrapped in an absolutely-positioned parallax layer at `inset: -14%` (overscan so the
    parallax translate never exposes an edge).
  - **Scrim:** `linear-gradient(180deg, rgba(13,17,23,.62) 0%, rgba(13,17,23,.22) 40%, rgba(13,17,23,.86) 100%)`.
  - **Content layer:** absolute inset 0, `flex-direction: column`, `justify-content: space-between`,
    padding `clamp(22px,4vw,56px) clamp(22px,5vw,72px) clamp(32px,6vw,72px)`.
    - Top eyebrow: "Wilderness · field note 04", `var(--gold-400)`.
    - Bottom row: `display: flex; align-items: flex-end; justify-content: space-between; gap: clamp(20px,4vw,48px); flex-wrap: wrap`.
      - **Text block** (`flex: 1 1 380px; max-width: 760px`): eyebrow "Sri Lanka · wet zone · 1990–2024"
        at `rgba(250,248,243,.62)`; h1 "Did the blue magpie *move*?" (the word *move* is
        `<em>` in `var(--gold-400)`), `max-width: 16ch`; deck paragraph at
        `rgba(250,248,243,.82)`, `max-width: 52ch`; then a "↓ Begin the story" cue —
        the arrow runs a 2.4s `cue` keyframe (translateY 0 → 7px → 0, opacity .5 → 1 → .5).
      - **Records glass chip** (`flex: 0 0 auto`): label "RECORDS", value counting up to
        **3,620** in serif `clamp(26px,3vw,34px)` with `font-variant-numeric: tabular-nums`.
        Wrapping this chip into the same flex row (rather than absolutely positioning it) is
        what makes the hero work on narrow screens.

### 2. The subject

- Two-column split. Left: `<figure>` at `aspect-ratio: 16/9`, `--radius-md`, `--shadow-md`,
  holding the magpie video plus a bottom `figcaption` — 34px/18px/14px padding,
  `linear-gradient(to top, rgba(13,17,23,.85), transparent)`, uppercase sans 10px, text
  "Urocissa ornata · reference render".
- Right: eyebrow "The subject", h2 "A loud blue bird that lives in one wet corner of one island",
  two body paragraphs, and a micro caption "Synthetic reference imagery · not a field photograph".

### 3. The question

Divider rule, then a large serif statement — "The question sounds simple." followed by an
italic `var(--text-accent)` clause "Has this bird's range moved in thirty-four years?" —
`max-width: 24ch`. One body paragraph beneath at `max-width: 62ch`.

### 4. The evidence (bar chart)

- Eyebrow "The evidence, honestly", h2 "Those sightings are not spread evenly. Not even close."
- **Chart card:** `var(--surface-card)`, `--radius-md`, `--shadow-sm`,
  padding `clamp(24px,3vw,38px)`.
  - Bar row: `display: flex`, `gap: clamp(4px,1.4vw,14px)`, `align-items: flex-end`,
    `height: clamp(180px,34vw,280px)`, `border-bottom: 1px solid var(--border-strong)`.
  - Each column: numeral above, bar below. Bar height is a **percentage** of the row
    (`n / 2009 × 87%`) so it stays responsive. `border-radius: 6px 6px 0 0`.
  - Bar fill: `rgba(13,17,23,.16)` light / `rgba(250,248,243,.22)` dark — except the final
    2020–24 bar, which is `--gold-500`. Its numeral is `--text-accent`; all others
    `--text-tertiary`.
  - Axis labels below: "90–94", "95–99", "00–04", "05–09", "10–14", "15–19", "20–24".
- Two body paragraphs beneath, with "ninety-fold" and "how hard we looked" emphasised.

**Data:** 22 · 29 · 70 · 140 · 341 · 1,009 · 2,009.

### 5. The map (animated KDE)

- Two-column split. Left column is a card (`--surface-card`, `--radius-md`, `--shadow-sm`,
  14px padding) containing:
  - A `<canvas>` at `aspect-ratio: .672` (backing store 640 × 952), `--radius-sm`,
    background `--cream-50`.
  - A **glass pill** top-right showing the active bin label (e.g. "2010–2014"),
    `--radius-pill`, 6px/14px padding, sans 12px/600.
  - A **scale bar** bottom-left: 52px hairline + "12 KM" in sans 9.5px, tracking `.16em`.
  - Below the canvas, a control row: a gold pill **Play timeline / Pause** button
    (`--accent-primary` fill, `--ink-950` label, `--radius-pill`, 10px/20px padding,
    hover `translateY(-1px)` + `--accent-primary-strong`); a row of seven 3px-tall clickable
    tick bars (`--gold-500` active, `--border-strong` already-seen, `--border-hairline`
    upcoming); and a right-aligned `n = 1,009` readout.
- Right column: eyebrow "What the sightings look like", h2, three body paragraphs.

**Canvas rendering** — this is the one piece that must be ported as code, not markup:

- Source data is a 300 × 300 grid of `Uint8` density values per time bin, base64-encoded in
  `data/density.json` (also emitted as `data/density.js`, which assigns `window.__DENSITY__`).
  Grid bounds: lat `6.1159 → 7.4549`, lon `80.0811 → 80.9814`.
- Each pixel bilinearly samples the grid, then maps `t ∈ [0,1]` through a **cream → moss → ink**
  ramp:

  | stop | rgb |
  | --- | --- |
  | 0.00 | `250, 248, 243` |
  | 0.05 | `236, 230, 216` |
  | 0.17 | `199, 187, 154` |
  | 0.34 | `124, 145, 132` |
  | 0.55 | `92, 122, 110` |
  | 0.78 | `60, 82, 73` |
  | 1.00 | `13, 17, 23` |

- **Uncertainty dither:** confidence is `min(1, sqrt(n / 500))`. Where a per-cell Bayer +
  hash noise value exceeds confidence, the pixel is blended back toward paper by
  `k = 0.10 + 0.18·t`. Early, sparse bins therefore render visibly grainy — this is a
  deliberate honesty device, not a bug.
- **Contours** at levels `0.32 / 0.55 / 0.78` via marching squares,
  `rgba(13,17,23, (0.18 + 0.14·i) × (0.45 + 0.55·confidence))`, `lineWidth: 1.1`.
- **Raw sighting dots:** 1.5px squares at `rgba(13,17,23,0.5)`.
- Frames are cached per bin after first paint, and bin changes **crossfade over 420ms** via an
  offscreen snapshot — never a hard cut.

### 6. Full-bleed break

- `height: clamp(360px, 74vh, 760px)`, `background: var(--ink-950)`, video `object-fit: cover`.
- Scrim `linear-gradient(180deg, rgba(13,17,23,.28) 0%, rgba(13,17,23,.78) 100%)`.
- Pull-quote pinned bottom-left, `padding: 0 clamp(24px,6vw,64px) clamp(40px,9vh,120px)`:
  serif italic, `border-left: 2px solid var(--gold-500)`, `padding-left: clamp(14px,2.4vw,24px)`,
  `max-width: 20ch`, `--cream-50`.
  Copy: *"More sightings means more people looking. It does not mean more birds."*
- The quote fades up (opacity 0 → 1, translateY 28px → 0, 1100ms) at 50% intersection.

### 7. The answer (centroid drift)

- Two-column split, text left, plot right.
- Text: eyebrow "The answer", h2, two paragraphs. Two computed values are injected inline as
  sans 17px `--text-accent`: net drift (**≈6.5 km**) and maximum excursion (**≈9.7 km**).
- **Plot card:** same card recipe, inner square `aspect-ratio: 1`, `--cream-50` ground.
  - SVG `viewBox="-8 -8 16 16"` — one user unit = 1 km, so the frame is a 16 × 16 km box.
  - Grid: lines every 2 km, `rgba(13,17,23,.09)`, `vector-effect: non-scaling-stroke`.
  - Trail: seven centroids joined in time order. Rendered twice — solid segments for the draw
    animation, and one dashed (`stroke-dasharray="3 3"`) full path for the resting state, both
    `rgba(13,17,23,.34)`, `stroke-width: 1.2`.
  - Points: circle radius `0.30 + sqrt(n) / sqrt(2009) × 1.45` km, so area encodes sample size.
    Inactive `fill: rgba(13,17,23,.04)`, `stroke: rgba(13,17,23,.3)`; active
    `fill: rgba(201,168,106,.28)`, `stroke: #a8823f`. A 0.09-unit `#0d1117` dot marks the exact
    centre.
  - Labels: first, last, and active bins only, sans 10px/600, offset `+1.6%` x / `-3.4%` y.
  - Scale bar bottom-left: 31.2% width + "5 KM".
  - Caption below the card: "16 × 16 km · circle size shows how many sightings stand behind each point".

### 8. Why it matters

h2 "Why bother saying nothing happened?", then three equal cards (`--surface-card`,
`--radius-md`, `--shadow-sm`, 32px padding), each an eyebrow + one paragraph at 17px:
"Because the shape is seductive" · "Because decisions follow" · "Because the fix is known".

### 9. Close

Full-width `--ink-950` card, `--radius-lg`, `--shadow-lg`, padding `clamp(40px,6vw,84px)`.
A 44 × 2px `--gold-500` rule, then the closing statement in `--cream-50` (`max-width: 26ch`)
and a supporting paragraph at `rgba(250,248,243,.72)` (`max-width: 62ch`).

### 10. Footer

Sans 11px `--text-tertiary`, flex-wrapped: methodology note, imagery disclaimer, and a
right-aligned link to the technical note. Link styling: `color: var(--text-accent)`,
`border-bottom: 1px solid var(--accent-primary)`; on hover both go `--text-primary`.

---

## Persistent chrome

- **ThemeToggle** — fixed, `top/right: clamp(12px, 2.5vw, 22px)`, `z-index: 40`. Writes
  `data-theme` on `<html>`.
- **Reading-progress chip** — fixed bottom-right, same offsets, `pointer-events: none`.
  The Wilderness `ProgressIndicator`: glass chip, uppercase sans label "FIELD NOTE", serif
  percentage. Fades in (opacity + 10px translate, 420ms) once scroll passes 60% of viewport
  height; percentage is `scrollY / (scrollHeight - innerHeight)`.

---

## Interactions & behaviour

| Trigger | Behaviour |
| --- | --- |
| Map card reaches 45% visibility | Timeline auto-plays once — advances bin every 1100ms, stops at bin 7. Suppressible via the `autoplayTimeline` prop. |
| Play/Pause button | Toggles the interval. If already at the last bin, restarts from bin 1. |
| Tick bar click | Jumps to that bin and stops playback. |
| Bin change | 420ms canvas crossfade; label pill, `n =` readout, tick colours, and the active centroid all update together. |
| Bar chart reaches 30% visibility | Bars grow from the baseline over 820ms, staggered 90ms apart; numerals fade in on the same stagger. |
| Centroid plot reaches 35% visibility | Segments draw sequentially (`stroke-dashoffset` → 0, 420ms each, 260ms apart); each point's radius eases 0 → r on the same cadence. At ~2150ms the solid trail crossfades into the dashed one over 520ms. |
| Any text block / card enters view | Fades up: opacity 0 → 1, translateY 26px → 0, 700ms. Side-by-side pairs stagger by 90ms; the three "why" cards by 0 / 110 / 220ms. |
| Scroll (continuous) | Parallax layers translate by `(elementCentre − viewportCentre) × speed`, batched in one `requestAnimationFrame`. Hero video speed `0.10` at `inset: -14%`; the subject and break videos are pinned at speed `0` so their framing is never cropped. |
| Hero mount | Records chip slides down and counts 0 → 3,620 over 1700ms, cubic ease-out, after a 420ms hold. |
| Videos | Autoplay, loop, muted, `playsinline`. Muting is enforced defensively — `defaultMuted`, `muted`, `volume = 0`, and `audioTracks` disabled, re-applied on `loadedmetadata`, `loadeddata`, `canplay`, `play`, `playing`, and `volumechange`. |
| Reduced motion | **Not yet implemented.** Add a `prefers-reduced-motion` path in production: skip parallax and reveals, jump the counter and bars to their end state, keep the timeline manual. |

---

## State management

Component-local; no external store, no network calls beyond loading the density data.

| State | Type | Purpose |
| --- | --- | --- |
| `loaded` | boolean | Density data parsed and ready |
| `active` | 0–6 | Current time bin — drives canvas, label, `n`, ticks, active centroid |
| `playing` | boolean | Timeline interval running |
| `theme` | `'light' \| 'dark'` | Mirrored onto `document.documentElement[data-theme]` |
| `progress` | 0–100 | Reading progress percentage |
| `barsIn` | boolean | Bar chart entrance fired |
| `trailIn` / `trailDone` | boolean | Centroid draw started / finished (drives the solid→dashed crossfade) |

Derived per render: bin descriptors (height, colours, delay, click handler), centroid
descriptors (position, radius, label position, colours), trail segments with per-segment dash
length and delay, and the net/max drift figures.

**Data loading:** prefers `window.__DENSITY__` (set by `data/density.js`), falling back to
`fetch('data/density.json')`. Bins arrive base64-encoded — decode with `atob` into a
`Uint8Array` before painting. In production, prefer a binary endpoint or a typed-array asset
over base64 in JS.

**Props exposed for configuration:**

- `initialTheme` — `'light' | 'dark'`, default `'light'`.
- `autoplayTimeline` — boolean, default `true`.

---

## Responsive behaviour

No media queries — the layout is fluid by construction:

- Every two/three-column grid uses `repeat(auto-fit, minmax(min(100%, Npx), 1fr))`, so columns
  collapse rather than overflow.
- Type scales with `clamp()`; the hero floor is 38px, body copy stays 19px.
- The hero uses `svh` so mobile browser chrome doesn't cause a jump, and the records chip
  wraps below the headline instead of overlapping the theme toggle.
- Bar heights are percentages, with fluid gaps and type, so all seven bins stay legible to
  ~320px.
- `overflow-x: hidden` on `html, body` guards against parallax bleed.

---

## Assets

All imagery is **AI-generated and illustrative** — there are no field photographs, and the page
says so in two places. Do not present it as documentary evidence; swap in licensed photography
before publication.

The three video files ship **inside this bundle**, under `assets/`:

| File | Use | Size | Notes |
| --- | --- | --- | --- |
| `assets/hero-rainforest-mist.mp4` | Hero background | 2.1 MB | Misty lowland rainforest at first light, locked-off camera |
| `assets/subject-magpie-branch.mp4` | "The subject" figure | 4.2 MB | Blue magpie on a mossy branch, locked-off long lens |
| `assets/break-understory-pushin.mp4` | Full-bleed break | 6.9 MB | ~10s cinematic dolly push-in, understory → magpie close-up |

In the main design file the videos are still referenced by their original remote CDN URL, and
attached at runtime via `data-src` (the element's `src` is assigned on mount). The companion
file `Where the Magpies Are -local assets-.dc.html` is byte-identical except that those three
URLs point at `./assets/*.mp4` — open that one to run the design fully offline.

In production: host the clips yourself, keep `muted playsinline loop`, add
`preload="metadata"`, supply a poster frame (grab frame 0 of each clip), and offer a WebM/AV1
sibling source. No poster stills are included in this bundle.

Fonts load from Google Fonts. No icon set — the design uses text labels and two bare glyphs
(`↓`, `→`), matching the Wilderness guidance.

---

## Data

`data/density.json` (~900 KB) — the whole analysis, precomputed:

- `grid`: `{ n: 300, lat0, lat1, lon0, lon1 }` — bounds of the density raster.
- `bins[]`: seven five-year windows, each with `n` (record count), `clat` / `clon` (centroid),
  `g` (base64 `Uint8Array`, 300 × 300 density, row-major from the **south** edge up), and
  `points` (raw `[lat, lon]` sightings).
- Method: gaussian KDE, bandwidth 0.90 km, over 3,620 records.

`data/density.js` is the same payload wrapped as `window.__DENSITY__ = {…}` for environments
where a relative `fetch` isn't available.

---

## Files in this bundle

| Path | What it is |
| --- | --- |
| `Where the Magpies Are.dc.html` | The main design — the story page documented above (remote video URLs) |
| `Where the Magpies Are -local assets-.dc.html` | Same design, wired to the bundled `assets/*.mp4` — runs offline |
| `assets/*.mp4` | The three video loops (see Assets) |
| `data/source_density_urocissa_ornata.json` | Original source records the KDE grids were computed from |
| `Range Shift - Urocissa ornata.dc.html` | The technical note linked from the footer |
| `support.js` | Prototype template runtime — **reference only, do not port** |
| `data/density.json` | Precomputed KDE grids, centroids, and raw sightings |
| `data/density.js` | Same data as a global assignment |
| `_ds/wilderness-design-system-…/tokens/*.css` | Colour, type, spacing, effect, and font tokens |
| `_ds/wilderness-design-system-…/styles.css` | Token entry point |
| `_ds/wilderness-design-system-…/_ds_bundle.js` | Wilderness React components (ThemeToggle, ProgressIndicator, MediaFrame, QuoteBlock, Card, Badge, and the form primitives) |

Open `Where the Magpies Are.dc.html` directly in a browser to see the design running.

---

## Suggested build order

1. Static layout and type at the token values above — no motion.
2. Bar chart and centroid plot from the data, static.
3. Canvas KDE renderer (colour ramp → dither → contours → dots), then the bin crossfade.
4. Timeline controls and the intersection-triggered entrances.
5. Parallax and the reading-progress chip.
6. Theme toggle, then the `prefers-reduced-motion` path.
