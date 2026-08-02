# Where the Magpies Are — Sri Lanka Blue Magpie range shift

A data story built on kernel density estimation over GBIF occurrence records for
*Urocissa ornata* (Sri Lanka blue magpie), binned into seven 5-year windows, exploring
whether the species' recorded range shifted between 1990 and 2024. Portfolio/exploratory
project — not formal research, not peer reviewed.

**Live:** https://s19835.github.io/magpie_range_shift/

**Headline finding:** the density-weighted centroid moves **6.5 km net** over 34 years,
never straying more than **9.7 km** from where it started — well within noise
for a centroid computed from as few as 22 records in the early bins. Record counts grow
~90x over the same period, from 22 to 2,009 per bin. That growth is birders, not birds
(see Known limitations below). Read the two facts separately: the story is not "the
magpie moved," it's "the range looks stable and the sightings exploded."

## What's in this repo

Two static, self-contained HTML pages plus a shared design system and precomputed data:

| Path | What it is |
| --- | --- |
| `index.html` | Redirect to the story (GitHub Pages entry point) |
| `Where the Magpies Are.dc.html` | The scrollytelling story — bar chart, animated KDE map, centroid drift plot |
| `Where the Magpies Are -local assets-.dc.html` | Same story, videos served from `assets/` instead of a remote CDN |
| `Range Shift - Urocissa ornata.dc.html` | The technical note — small multiples, difference maps, centroid table |
| `support.js` | Runtime the `.dc.html` pages depend on (must load before them) |
| `_ds/wilderness-design-system-*/` | Design tokens and component bundle used by both pages |
| `assets/*.mp4` | Three generated video loops used by the local-assets story page |
| `data/density.json`, `data/density.js` | Precomputed KDE grids, per-bin centroids, and raw sighting points. `data/density.js` sets `window.__DENSITY__`; both HTML pages load it via a `<script>` tag and fall back to `fetch('data/density.json')` if that global isn't present |
| `data/source_density_urocissa_ornata.json` | 12 MB source records the KDE grids were computed from (not read by the pages at runtime) |

The Python pipeline that produced the data (`01_fetch_gbif.py`, `02_clean_and_eda.py`,
`03_kde_model.py`) is **not part of this repo** — only its JSON output
(`data/density.json` / `data/density.js`) is checked in here. The section below documents
how that upstream pipeline worked and why, for provenance.

For the design system, motion specs, and instructions on recreating these pages in a
production codebase, see [`HANDOFF.md`](HANDOFF.md). For notes on how the `.dc.html`
runtime works and how to verify a change, see [`CLAUDE.md`](CLAUDE.md).

## Viewing locally

```bash
python3 -m http.server
```

Then open `http://localhost:8000/`. Opening the files directly via `file://` breaks the
`fetch('data/density.json')` fallback path.

## How the data was produced

Upstream pipeline: GBIF fetch → clean → KDE model, run as three sequential scripts against
the GBIF API, none of which live in this repo. The reasoning behind the pipeline's
constants, kept here because it explains numbers that show up on the pages:

### Ingest — GBIF fetch

- Pulled all Sri Lanka (`country=LK`) records for *Urocissa ornata* via paginated GBIF
  search (300/page, offset-based).
- `hasGeospatialIssue` filter deliberately **not** set. It was tried and removed after
  checking what GBIF's `issues` column actually contained on this dataset: 91%
  `CONTINENT_DERIVED_FROM_COORDINATES`, which is informational, not a quality problem —
  the genuinely dangerous flags like `COORDINATE_INVALID` don't occur here at all.
  Filtering server-side would have traded visibility for no measurable benefit on this
  species. This finding doesn't generalize automatically to other species.
- `occurrenceID` and `recordedBy` were kept in the export because the clean step's dedup
  logic depends on them.

### Clean — filtering and deduplication

- Standard filters: valid coordinates inside a Sri Lanka bounding box, has a `year`,
  coordinate uncertainty ≤10 km (missing uncertainty is kept, not dropped — common for
  older museum records).
- Dedup is on `occurrenceID`, not lat/lon/date. An earlier approach (dedup on
  lat/lon/year/institution) destroyed 62% of the dataset, because eBird hotspots share one
  fixed coordinate across every checklist submitted there — many genuinely distinct
  observers/days collapsed into one record. `occurrenceID` tracks the actual observation
  event instead. The ~10 rows missing `occurrenceID` (all from one publisher,
  `NABU|naturgucker`) are kept as-is and never merged with each other, since missing
  identity metadata means "unknown," not "same." Checked by hand that none of those 10 are
  true duplicates.
- Cutoff year is 2024. eBird publishes to GBIF in periodic bulk snapshots, not
  continuously, and this dataset's eBird snapshot cuts off in 2024 — 2025/2026 records
  collapse to near-zero because the dominant source hadn't synced yet, not because
  sightings stopped. Confirmed via the `institutionCode` breakdown: `CLO` (eBird) supplies
  500+ records/year through 2024, then zero after, while `iNaturalist` keeps its normal
  pace. This cutoff moves if the dataset is re-fetched later.

### Model — kernel density estimation

- One pooled bandwidth, selected once via 5-fold cross-validation on log-likelihood
  against all points together, then reused unchanged across every bin — not fit per bin.
  Two reasons: early bins have as few as ~22 points, so 5-fold CV on that leaves ~4
  validation points per fold, too noisy to trust; and KDE's CV-optimal bandwidth
  mechanically shrinks with sample size (~n^(-1/5)), so per-bin CV would make data-rich
  recent bins look artificially "tighter" for a reason that has nothing to do with the
  bird. Since the whole point is comparing bins against each other, they need the same
  smoothing assumption.
- Bandwidth search was log-spaced, 0.2–60 km, not linear 5–60 km. A linear grid had
  returned its own floor value (5 km) — a sign the optimizer wanted to go narrower but
  wasn't allowed to. The true optimum turned out to be **0.8972 km** (rounds to 0.90),
  reflecting this species' genuinely tight local clustering (a habitat specialist with
  dense records around Sinharaja).
- Grid extent is data-driven (occurrence bounding box + 15 km margin), not the whole
  country — a country-wide box would waste almost all grid resolution on empty area this
  species is never recorded in.
- Grid resolution is 300×300, chosen so cell size (~0.5 km) stays below the 0.90 km
  bandwidth. A coarser grid than the bandwidth under-resolves the KDE surface regardless of
  how correct the bandwidth math is — this relationship needs rechecking if the grid
  resolution, bounding box, or species (with a different optimal bandwidth) changes.
- Coastline masking was investigated, not implemented. Cropping to the data extent already
  put ~98% of the grid box on land (checked against a real, if coarse, Sri Lanka boundary
  polygon). The boundary data available without a live GBIF-side fetch was too coarse
  (~15 vertices for the whole island) to trust for a real per-cell mask — using it risked
  clipping actual land as ocean, a worse and harder-to-spot error than the small sliver it
  would fix.

**Resulting dataset:** 3,620 records, 7 five-year bins (1990–2024), bin sizes
22 / 29 / 70 / 140 / 341 / 1,009 / 2,009.

## Known limitations (unresolved, stated plainly rather than hidden)

**Sampling effort bias.** Record counts grew ~90x from the 1990s to the 2020s, driven by
eBird adoption, not the bird's actual abundance. This was checked against the possibility
that it's a few power users — it isn't: unique observer counts scale roughly with total
records (63 → 187 → 518 → 676 across four periods), and the top 5 observers never account
for more than ~15% of any bin. This means the density surface partly reflects *where and
when people search* as well as *where the bird is*. No full fix is applied — one would
require a background "effort surface" (a KDE fit on all bird sightings in Sri Lanka, same
bins, used as a denominator), which needs a broader GBIF fetch outside this project's
current scope.

**Not a climate/range-shift claim.** Nothing in this pipeline supports attributing
centroid movement to climate, habitat change, or actual range expansion/contraction. Bins
are independent snapshots with no interpolation between them; the honest headline is a
stable core around Sinharaja and a record set that grew up around it.

## Species history

The project started with two mammal candidates (Sri Lankan elephant, Sri Lankan leopard) —
both too sparse across time for a real multi-bin KDE comparison (elephant: 56 records, 86%
concentrated in two decades). It moved to the Sri Lanka blue magpie (*Urocissa ornata*), an
endemic montane-forest specialist, for better eBird-sourced GBIF coverage and a more
biologically meaningful question (elevation/habitat fragmentation pressure is real for this
species) — though see the sampling-effort caveat above before reading too much into any of
it.
