# Guidebook Site — Design Spec

Date: 2026-09-03
Status: Approved

## Purpose

A minimalistic, single-page guest guidebook for the Airbnb rental, published
via GitHub Pages. A guest reads this on their phone during their stay to get
everything they need: check-in/out info, wifi, parking, and nearby
restaurants/activities. No login, no app — just a link.

## Scope

In scope for this spec:
- One static HTML page (`index.html`) with four content sections
- One stylesheet (`style.css`) implementing a warm, minimalistic visual style
- Placeholder content in every section, clearly marked for the owner to
  replace with real details later

Out of scope (explicitly deferred, not part of this build):
- Real content (actual wifi password, address, door code, restaurant picks)
- JS-driven interactions (sticky/active-highlighting nav, image galleries)
- Multi-page structure (may be revisited later if the single page grows too
  long)
- Any backend, form, or dynamic data

## Approach

Single `index.html` + one `style.css`, no JavaScript. Navigation is plain
anchor links (`<a href="#checkin">`) to `<section id="checkin">` etc. —
native browser scroll-to-anchor behavior, no script required.

Rejected alternative: add a small JS-driven sticky nav bar with
scroll-based active-section highlighting. More polished, but adds a moving
part for a page most guests read once per stay. Matches CLAUDE.md's rule to
use JS only where genuinely necessary. Can be added later as its own bounded
change if wanted.

## Structure

**`index.html`** — semantic HTML5:
- `<header>`: property name/tagline + a nav with 4 anchor links (Check-in ·
  Wifi · Parking · Nearby)
- `<main>` containing four `<section>` elements, one per content area below
- `<footer>`: a contact/host line (placeholder)

**`style.css`**:
- CSS custom properties for the palette: off-white background, one warm
  accent color, dark neutral text
- Mobile-first layout, single-column, max-width container centered on wider
  screens
- Card-style blocks for restaurant/activity entries
- No external fonts/frameworks/CDNs — system font stack only, per CLAUDE.md

## Content sections (placeholder content)

1. **Check-in / house rules** (`#checkin`) — arrival time, door code,
   checkout time, 2-3 house rules. Placeholders like `[ARRIVAL_TIME]`,
   `[DOOR_CODE]` clearly marked.
2. **Wifi & essentials** (`#wifi`) — network name + password placeholders,
   note on where to find towels/extra supplies, trash/recycling info.
3. **Parking & getting there** (`#parking`) — parking instructions,
   directions text, a placeholder link for a maps URL (`href="#"` with a
   TODO comment, not a broken external link).
4. **Restaurants & activities** (`#nearby`) — ~3 restaurant cards and ~3
   activity cards, each a short one-line description. Sample/placeholder
   names, clearly not real recommendations.

## Error handling / edge cases

- No real external links exist yet (no real address/maps/restaurant URLs) —
  placeholder links use `href="#"` with an inline HTML comment marking them
  as TODO, so nothing 404s and nothing looks broken to the owner reviewing
  the page.
- All images are deferred (no real photos yet) — sections use text/emoji or
  simple CSS shapes rather than `<img>` tags pointing at missing files, so
  the page never shows broken image icons.

## Testing

Manual verification only, no test framework (static HTML/CSS project):
- Open `index.html` in a browser at a mobile viewport width (~375px) and a
  desktop width (~1200px+); confirm layout doesn't break at either.
- Click every nav anchor link; confirm it scrolls to the right section.
- Visually confirm no broken images, no unstyled/raw HTML showing.
- Confirm placeholder text is clearly marked (e.g. bracketed) so the owner
  can find every spot that needs a real value before publishing.

## Implementation

Per CLAUDE.md, implementation proceeds via `superpowers:writing-plans` next,
executed by subagents per the model-tiering table. This build is small
enough to realistically be one `claude-sonnet-5` "build the page" task, or
split into two parallel Sonnet-tier tasks (HTML structure vs. CSS styling)
if the plan calls for parallelizing.
