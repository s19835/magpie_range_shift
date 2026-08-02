# magpie_range_shift

Two static data-story pages about *Urocissa ornata* (Sri Lanka blue magpie) occurrence
records, 1990–2024. Published at https://s19835.github.io/magpie_range_shift/

## Layout

- `Where the Magpies Are.dc.html` — the narrative story (uses the `_ds/` design system, theme toggle, scroll reveals)
- `Range Shift - Urocissa ornata.dc.html` — the technical note (small multiples, difference maps, centroid table)
- `Where the Magpies Are -local assets-.dc.html` — same story, videos from `assets/` instead of CloudFront
- `index.html` — redirect to the story (GitHub Pages entry point)
- `support.js` — the `.dc.html` runtime; must load before the page
- `data/density.js` — `window.__DENSITY__`, the KDE grids. Both pages load this via `<script>` and fall back to `fetch('data/density.json')`
- `data/source_density_urocissa_ornata.json` — 12 MB source, not read by the pages

## How the pages work

Each file is a self-contained `.dc.html`: markup inside `<x-dc>` with `{{ }}` bindings,
logic in the trailing `<script type="text/x-dc">` as a `DCLogic` subclass. `renderVals()`
returns the binding values; canvases are painted imperatively via `ref` callbacks.
The seven 300×300 grids are base64 `Uint8Array`s decoded in `componentDidMount`.

Console errors about `<svg> attribute viewBox: Expected number, "{{ vb }}"` are normal —
the browser parses the raw template before `support.js` hydrates it.

## Working on this

Serve over HTTP (`python3 -m http.server`); `file://` breaks the JSON fallback.
Verify a change by checking a canvas actually has varied pixels, not just that the page loads.
Filenames contain spaces — URL-encode them in links.
