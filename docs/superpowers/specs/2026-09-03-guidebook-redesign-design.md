# Guidebook Site Redesign — Design Spec

Date: 2026-09-03
Status: Approved
Supersedes: `docs/superpowers/specs/2026-09-03-guidebook-site-design.md`

## Purpose

A full visual and structural redesign of the single-page guest guidebook,
inspired by a reference mockup the owner provided
(`airbnb_guest_info_page_minimal_layout.html`, not committed to the repo —
it's a design-tool export used only as inspiration, with undefined CSS
variables and placeholder markup of its own). The previous build (plain
prose sections) is replaced with a more scannable, card/accordion-based
layout, while keeping every piece of content the original spec required.

The mockup's icon-font (`ti ti-*` classes) and JS-implied collapsible rows
are adapted to this project's constraints (CLAUDE.md: vanilla only, no
external CDNs, JS only where genuinely necessary) — see Approach.

## Scope

In scope:
- Full rewrite of `index.html` (structure and content) and `style.css`
  (visual system), replacing the current build.
- 8 hand-authored inline SVG icons.
- Still one static page, no JS, no build step, no dependencies.

Out of scope (unchanged from the original spec):
- Real content (still placeholders, bracketed).
- Any backend, form, or dynamic data.
- Multi-page structure.

## Approach

**Icons:** inline SVG, not an icon font. The mockup's `ti ti-wifi` etc.
classes imply pulling Tabler Icons from a CDN, which conflicts with
CLAUDE.md's "no external CDNs" rule. Instead, 8 small hand-authored inline
`<svg>` icons (24×24 viewBox, stroke-based line style, `currentColor`
stroke so they inherit text color) are embedded directly in `index.html`:
wifi, login (check-in), logout (check-out), key (getting in), heat
(heating/hot water), trash (bins/recycling), pin (address), chat-bubble
(message host). Zero dependencies, fully offline.

**Collapsible rows:** native `<details>/<summary>`, not JS. The mockup's
chevron-affordance rows ("Getting in," "Heating," "Bins") become real
accordions using `<details>`, which is interactive out of the box with no
script — keeps the "JS only where genuinely necessary" rule intact while
making the collapse behavior real rather than decorative.

**Content model:** parking and restaurants/activities merge into one
"Around the corner" tile grid (name + distance/time only, no long
description), matching the mockup exactly, per the owner's explicit choice
over keeping parking as its own detailed section.

**Visual tokens:** extend the existing `style.css` custom properties rather
than renaming them to match the mockup's token names (which were only
ever inspiration, not a contract). Two additions: `--surface-2` (nested
card background, e.g. quick-fact tiles) and `--font-mono` (monospace stack
for the wifi password and door code — values meant to be read/copied
exactly, not prose).

## Structure

**`index.html`** — semantic HTML5, single page, no JS:

1. **Hero** (`.hero`) — a `<div>` placeholder box (no `<img>`): accent
   background, rounded corners, a small "Cover photo" label chip
   positioned at the bottom-left, matching the mockup's cover-photo
   treatment without a real image.
2. **Welcome header** (`.welcome`) — "Welcome to" eyebrow text, `<h1>`
   `[PROPERTY_NAME]`, and an address line with the pin SVG icon:
   `[PROPERTY_ADDRESS]`.
3. **Quick facts** (`.quick-facts`, a 3-tile grid) —
   - Wifi tile: wifi icon, `[WIFI_NETWORK_NAME]`, `[WIFI_PASSWORD]` in
     monospace.
   - Check-in tile: login icon, `[ARRIVAL_TIME]`.
   - Check-out tile: logout icon, `[CHECKOUT_TIME]`.
4. **The house** (`#house`, `<section>`) — `<h2>The house</h2>` followed by
   four `<details class="house-row">` elements, each with a `<summary>`
   (icon + row title) and detail text in a child `<p>`:
   - Getting in — key icon — `Keybox left of the [DOOR_LOCATION], code
     [DOOR_CODE]`
   - Heating & hot water — heat icon — `[HEATING_INSTRUCTIONS]`
   - Bins & recycling — trash icon — `Bins are [TRASH_LOCATION] — pickup is
     [TRASH_DAY]`
   - House rules — no icon (icon slot omitted for this row only) — `No
     smoking indoors. Quiet hours after [QUIET_HOURS_START]. No parties or
     events.`
5. **Around the corner** (`#nearby`, `<section>`) — `<h2>Around the
   corner</h2>` followed by one `.tile-grid` containing ~7
   `<article class="tile">` entries (3 restaurants + 3 activities + 1
   parking), each with a CSS placeholder photo box (no `<img>`), a name
   (`[PLACE_1_NAME]` … ), and a distance/time line (`[X min walk]` /
   `[Free after TIME]` for the parking tile specifically).
6. **Host contact** (`.host-contact`, replaces the old `<footer>`) —
   an initials-avatar circle (`[HOST_INITIALS]`), `[HOST_NAME], your host`,
   a response-time note (`Usually replies within [RESPONSE_TIME]`), and a
   "Message" button/link: chat-bubble icon, `href="#"` with an inline HTML
   comment marking it TODO — same placeholder-link pattern as the map link
   in the previous build. No real messaging backend.

**`style.css`**:
- All existing custom properties retained (`--color-bg`, `--color-surface`,
  `--color-text`, `--color-text-muted`, `--color-accent`, `--color-border`,
  spacing scale, `--radius`, `--max-width`).
- New: `--surface-2` (a step darker/warmer than `--color-surface`, for
  nested tiles/cards sitting on top of the base surface), `--font-mono`
  (system monospace stack: `ui-monospace, SFMono-Regular, Menlo, Consolas,
  monospace`).
- `.hero`, `.welcome`, `.quick-facts` + `.quick-fact-card`, `.house-row`
  (`<details>` styling: default `<summary>` marker hidden via
  `summary { list-style: none }` plus `summary::-webkit-details-marker {
  display: none }`; a CSS-only chevron is drawn with a `::after`
  pseudo-element on `summary` — a small rotated-square/border arrow, no
  extra SVG — that flips via `transform: rotate(90deg)` when the parent
  `<details>` has the `[open]` attribute), `.tile-grid` + `.tile`,
  `.host-contact` + `.host-avatar`.
- Mobile-first: `.quick-facts` and `.tile-grid` are single-column below
  480px, multi-column above it (same breakpoint pattern as the current
  build).

## Error handling / edge cases

- No `<img>` tags anywhere — hero and tile photos are CSS-only placeholder
  boxes, exactly as in the original spec's rule.
- All placeholder text values remain bracketed (`[PROPERTY_NAME]`,
  `[DOOR_CODE]`, etc.).
- The "Message" host link and the (now-removed, folded into a tile) map
  link both use `href="#"` with an inline TODO comment — never a guessed
  real URL.
- `<details>` elements must not be clipped by `overflow: hidden` on a
  parent — verified explicitly in Testing.

## Testing

Manual verification, same method as before (no test framework):
- Open in a browser at mobile (~375px) and desktop (~1200px+) widths;
  confirm quick-facts and tile grids go single-column → multi-column at the
  breakpoint, no horizontal overflow.
- Click/tap and keyboard-activate (Enter/Space while focused) each
  `<details>` row; confirm it expands and collapses, and that expanded
  content is fully visible (not clipped).
- Confirm no broken images (there are none — placeholder boxes only).
- Confirm every placeholder value is visibly bracketed.
- Confirm the wifi password and door code render in the monospace font.

## Implementation

Per CLAUDE.md, implementation proceeds via `superpowers:writing-plans`
next, executed by subagents per the model-tiering table. This redesign
touches both files substantially and adds new SVG-authoring work, so it's
sized for at least two tasks (HTML structure + icons, CSS visual system),
likely three with the QA pass — final task breakdown decided in the plan.
