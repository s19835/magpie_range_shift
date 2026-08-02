# Wilderness Design System

Wilderness is a wildlife storytelling brand: long-form field journals, migration and conservation reporting, told through cinematic photography and a quiet, literary voice. The system supports a marketing/storytelling website and a long-form article reading experience, in both a true light theme and a true dark theme.

**Source material:** one inspiration image, `uploads/Storytelling Landing Page Design.jpeg` — a template-showcase mockup (brand shown in it, "Norvale," is the template's own demo content, not Wilderness) illustrating the desired *composition*: a dark canvas holding a light inset "card," serif display type mixed with sans UI labels, floating photo insets, a frosted-glass progress chip, and pull-quote storytelling beats. No Figma file, codebase, or logo was provided — this system was built from that single reference plus the brief, then generalized into full brand foundations, components, and two UI kits.

## Content fundamentals
- Voice: first-person, journal-like, present-tense observation ("The plain slowly emptied at dusk"). Never marketing-speak, never exclamation points.
- Casing: sentence case everywhere except nav/eyebrow labels, which are set in small, wide-tracked uppercase (e.g. "MIGRATION · CHAPTER 01").
- Pronouns: third-person narrator describing the subject/wildlife; occasional first-person field-note asides ("we had followed three generations of this route").
- No emoji, ever. No unicode-icon-as-bullet tricks.
- Numbers used sparingly and always concrete/sensory (distances, minutes, days), never marketing stats ("10x", "50% faster").
- Pull quotes carry the emotional turn of each piece — short, aphoristic, present tense.

## Visual foundations
- **Color:** near-black ink (`--ink-950 #0d1117`) and warm cream (`--cream-100 #f4f1ea`) are the two surfaces; a single warm gold (`--gold-500 #c9a86a`) is the one accent used for CTAs, focus rings and active states, plus a muted brown and moss for secondary/nature accents. Max two accent colors on screen at once.
- **Type:** `Manrope` (geometric sans) for all UI chrome, nav, labels, buttons; `Ibarra Real Nova` (warm serif, true italics) for headlines, drop caps, pull-quotes. *Substitutes for the reference's Satoshi/Sentient — see Font note below.*
- **Imagery:** full-bleed wildlife/landscape photography is the star; every photo gets a bottom protection-gradient scrim when it carries text. Warm, slightly desaturated, documentary — never oversaturated or heavily filtered.
- **Cards:** light theme cards are pure white on cream with a soft shadow (`--shadow-sm`) and 12px radius; dark theme cards are a slightly-lifted ink tone with the same radius. No colored borders, no left-accent-bar cards.
- **Glass chips:** floating metadata (reading progress, stats) uses a frosted dark chip — `rgba(13,17,23,.55)` + `blur(16px)` — regardless of page theme, since it always floats over photography.
- **Radius:** 6/12/20/28px scale (sm/md/lg/xl) plus a pill for tags/switches. Nothing sharp, nothing overly rounded.
- **Shadow:** soft and low-contrast on light surfaces; a stronger `--shadow-float` (large, soft, black) for anything floating over photography, echoing the reference's layered photo insets.
- **Motion:** slow and deliberate — 150/260/420ms, `cubic-bezier(.4,0,.2,1)`. Hovers lift 1px; nothing bounces or scales aggressively.
- **Hover/press:** hover = 1px lift or a color shift toward the accent; press has no separate treatment defined yet (flag below).
- **Layout:** the marketing hero's dark-canvas + light-inset-card composition is a fixed brand motif carried from the reference, independent of the light/dark theme toggle, which instead governs the rest of the page (archive, footer, reader).
- **Transparency/blur:** reserved for the glass progress/stat chips and modal scrims — not used decoratively elsewhere.

## Iconography
No icon system, icon font, or SVG set was supplied. The system currently uses no icons at all — controls are labeled with text (e.g. "Begin the Story →") or a single inline glyph (☰ for the mobile menu, → for forward actions). If a real icon set is added later, Lucide (stroke-based, matches the brand's restrained line weight) is the recommended default; document the substitution here when it's chosen.

## Intentional additions
No component-inventory source (Figma/codebase) was supplied, so a standard primitive set was authored, sized to a storytelling site's needs, plus three brand-specific additions:
- **ProgressIndicator** — the frosted reading/migration-progress chip from the reference image.
- **QuoteBlock** — pull-quote treatment; storytelling quotes are load-bearing in this brand's content.
- **MediaFrame** — the rounded photo frame + bottom scrim used for every inset/hero photo.
- **ThemeToggle** — light/dark mode is a named requirement of the brief, not just a nice-to-have.

## Caveats — please help me iterate
- **Font substitution:** the reference's fonts (Satoshi, Sentient) aren't available to me as licensed files, so I substituted the closest free pairing — Manrope + Ibarra Real Nova (Google Fonts). If you have the real Satoshi/Sentient font files, attach them and I'll swap `tokens/fonts.css` to self-hosted `@font-face` rules.
- **No logo supplied.** Every mark is a typeset "WILDERNESS" wordmark. If there's a real logo, attach it and I'll wire it into `assets/` and the wordmark card/thumbnail.
- **No press/active state defined** for buttons/cards beyond the 1px hover lift — tell me if you want a distinct press treatment (darken? scale down?).
- **Placeholder photography only** — every image is an editable drop-in `<image-slot>`; nothing here is a generated or stock image. Send real field photography whenever you have it.
- **"Higgsfield-style" motion/parallax elements** were interpreted as the floating photo-inset + glass-chip motif already in the hero; if you meant something more specific (actual scroll-parallax, video loops, AI-generated background motion), tell me more and I'll build it.

## Index
- `styles.css` — root stylesheet, imports everything below.
- `tokens/` — `colors.css`, `typography.css`, `spacing.css`, `effects.css` (radius/shadow/motion), `fonts.css` (Google Fonts import).
- `guidelines/` — 12 foundation specimen cards (Colors, Type, Spacing, Foundations, Brand groups in the Design System tab).
- `components/` — 15 React primitives across `forms/` (Button, Input, Select, Checkbox, Switch), `feedback/` (Badge, Tag, Tooltip, ProgressIndicator), `navigation/` (Tabs, ThemeToggle), `content/` (Card, QuoteBlock, MediaFrame), `overlay/` (Dialog).
- `ui_kits/marketing-site/` — homepage recreation (Hero, StoryGrid, Footer), interactive theme toggle + unlock dialog.
- `ui_kits/reader/` — long-form article reading experience with live scroll progress.
- `image-slot.js` — shared drag-and-drop image placeholder used throughout.
- `SKILL.md` — portable skill file for using this system in Claude Code.
