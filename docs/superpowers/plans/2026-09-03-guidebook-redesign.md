# Guidebook Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the existing `index.html` and `style.css` with a card/accordion-based redesign (hero, welcome header, quick-facts strip, "The house" accordion, "Around the corner" tile grid, host-contact block) with 8 inline SVG icons, per the approved redesign spec.

**Architecture:** Same two static files as before, fully rewritten. No JavaScript — the accordion rows use native `<details>/<summary>`. Icons are inline SVG embedded directly in the HTML, not an icon font.

**Tech Stack:** Vanilla HTML5 + CSS3 only. No frameworks, no npm, no CDNs, no external fonts (system font stack + system monospace stack).

**Spec:** `docs/superpowers/specs/2026-09-03-guidebook-redesign-design.md`

**Testing note:** Same as the prior plan — no test framework for this static site. "Test" steps are `grep` structural checks (Tasks 1-2) and a real-browser QA pass (Task 3), not unit tests.

## Global Constraints

- Vanilla HTML/CSS only — no JavaScript, no frameworks, no npm, no build step (CLAUDE.md).
- Accordion rows (`<details>/<summary>`) provide expand/collapse natively — no script (spec, Approach).
- Icons are 8 hand-authored inline `<svg>` elements — no icon font, no CDN (spec, Approach).
- No `<img>` tags anywhere — hero and tile photos are CSS-only placeholder boxes (spec, Error handling).
- All placeholder values are bracketed, e.g. `[DOOR_CODE]` (spec, Structure).
- The host "Message" link uses `href="#"` with an inline HTML comment marking it TODO — never a real or guessed URL (spec, Error handling).
- Mobile-first layout: `.quick-facts` and `.tile-grid` are single-column below 480px, multi-column at `@media (min-width: 480px)` (spec, Structure/Testing).
- New CSS custom properties: `--surface-2` and `--font-mono`, added alongside the existing token set (`--color-bg`, `--color-surface`, `--color-text`, `--color-text-muted`, `--color-accent: #a8492a`, `--color-border`, spacing scale, `--radius`, `--max-width`) — reuse the existing values verbatim, do not rename them (spec, Visual tokens).
- Work happens on branch `feature/guidebook-site` — do not commit to `main` (CLAUDE.md, Git workflow).
- Commits must never mention Claude/AI authorship (CLAUDE.md, Git workflow).
- Repo root: `/Users/alexandresokolow/asokolow/airbnb`. `index.html` and `style.css` already exist at the repo root from the prior build — this plan replaces their full content.

---

### Task 1: HTML structure, content, and inline SVG icons

**Files:**
- Modify: `index.html` (full content replacement)

**Interfaces:**
- Consumes: nothing (first task).
- Produces: `index.html` with a `<link rel="stylesheet" href="style.css">` in `<head>` (Task 2 creates the file this points to), and these classes/ids for Task 2's CSS to target: `.hero`, `.hero-label`, `.welcome`, `.eyebrow`, `.address`, `.icon`, `.quick-facts`, `.quick-fact-card`, `.quick-fact-label`, `.quick-fact-value`, `.quick-fact-sub`, `.mono`, `#house`, `#nearby`, `.guide-section`, `.house-row`, `.tile-grid`, `.tile-photo`, `.tile-name`, `.tile-meta`, `.host-contact`, `.host-avatar`, `.host-info`, `.host-name`, `.host-response`, `.host-message`.

- [ ] **Step 1: Write `index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome — Your Stay Guide</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main>
    <div class="hero">
      <span class="hero-label">Cover photo</span>
    </div>

    <header class="welcome">
      <p class="eyebrow">Welcome to</p>
      <h1>[PROPERTY_NAME]</h1>
      <p class="address">
        <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <path d="M12 21s-7-6.2-7-11a7 7 0 0 1 14 0c0 4.8-7 11-7 11z" />
          <circle cx="12" cy="10" r="2.5" />
        </svg>
        [PROPERTY_ADDRESS]
      </p>
    </header>

    <section class="quick-facts" aria-label="Quick facts">
      <div class="quick-fact-card">
        <p class="quick-fact-label">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M2 8.5C8 3 16 3 22 8.5" />
            <path d="M5.5 12C9.5 8.5 14.5 8.5 18.5 12" />
            <path d="M9 15.5C10.9 13.8 13.1 13.8 15 15.5" />
            <circle cx="12" cy="19" r="1.25" fill="currentColor" stroke="none" />
          </svg>
          Wifi
        </p>
        <p class="quick-fact-value mono">[WIFI_NETWORK_NAME]</p>
        <p class="quick-fact-sub mono">[WIFI_PASSWORD]</p>
      </div>
      <div class="quick-fact-card">
        <p class="quick-fact-label">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M15 4h3a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2h-3" />
            <path d="M3 12h11" />
            <path d="M10 8l4 4-4 4" />
          </svg>
          Check-in
        </p>
        <p class="quick-fact-value">[ARRIVAL_TIME]</p>
      </div>
      <div class="quick-fact-card">
        <p class="quick-fact-label">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M9 4H6a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h3" />
            <path d="M10 12h11" />
            <path d="M17 8l4 4-4 4" />
          </svg>
          Check-out
        </p>
        <p class="quick-fact-value">[CHECKOUT_TIME]</p>
      </div>
    </section>

    <section id="house" class="guide-section">
      <h2>The house</h2>

      <details class="house-row">
        <summary>
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <circle cx="6.5" cy="16.5" r="3.5" />
            <path d="M9.5 13.5L19 4" />
            <path d="M15 8l3 3" />
            <path d="M18 5l2.5 2.5" />
          </svg>
          <span>Getting in</span>
        </summary>
        <p>Keybox left of the [DOOR_LOCATION], code <strong class="mono">[DOOR_CODE]</strong></p>
      </details>

      <details class="house-row">
        <summary>
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M12 14.5V5a2 2 0 0 0-4 0v9.5a4 4 0 1 0 4 0z" />
            <circle cx="10" cy="16" r="1" fill="currentColor" stroke="none" />
          </svg>
          <span>Heating &amp; hot water</span>
        </summary>
        <p>[HEATING_INSTRUCTIONS]</p>
      </details>

      <details class="house-row">
        <summary>
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
            <path d="M4 7h16" />
            <path d="M9 7V4h6v3" />
            <path d="M6 7l1 13a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2l1-13" />
            <path d="M10 11v6" />
            <path d="M14 11v6" />
          </svg>
          <span>Bins &amp; recycling</span>
        </summary>
        <p>Bins are [TRASH_LOCATION] — pickup is [TRASH_DAY]</p>
      </details>

      <details class="house-row">
        <summary>
          <span>House rules</span>
        </summary>
        <p>No smoking indoors. Quiet hours after [QUIET_HOURS_START]. No parties or events.</p>
      </details>
    </section>

    <section id="nearby" class="guide-section">
      <h2>Around the corner</h2>
      <div class="tile-grid">
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[RESTAURANT_1_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[RESTAURANT_2_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[RESTAURANT_3_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[ACTIVITY_1_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[ACTIVITY_2_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">[ACTIVITY_3_NAME]</p>
          <p class="tile-meta">[X min walk]</p>
        </article>
        <article class="tile">
          <div class="tile-photo"></div>
          <p class="tile-name">Parking</p>
          <p class="tile-meta">[Free after TIME]</p>
        </article>
      </div>
    </section>

    <section class="host-contact" aria-label="Host contact">
      <div class="host-avatar">[HOST_INITIALS]</div>
      <div class="host-info">
        <p class="host-name">[HOST_NAME], your host</p>
        <p class="host-response">Usually replies within [RESPONSE_TIME]</p>
      </div>
      <a class="host-message" href="#"><!-- TODO: replace with real messaging link -->
        <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <path d="M4 5h16v11H9l-4 4V5z" />
        </svg>
        Message
      </a>
    </section>
  </main>
</body>
</html>
```

This completely replaces whatever `index.html` currently contains — do not merge with the old content, overwrite the file with exactly the above.

- [ ] **Step 2: Verify structure**

Run (from repo root):
```bash
grep -c '<svg class="icon"' index.html
grep -c 'class="tile"' index.html
grep -c 'class="house-row"' index.html
grep -c '<img' index.html
grep -c 'href="style.css"' index.html
grep -c 'id="house"' index.html
grep -c 'id="nearby"' index.html
```
Expected: `<svg class="icon"` count is `8`; `class="tile"` count is `7`; `class="house-row"` count is `4`; `<img` count is `0`; the remaining three commands each print `1`.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Redesign guidebook page: card/accordion layout with inline SVG icons"
```

---

### Task 2: CSS visual system

**Files:**
- Modify: `style.css` (full content replacement)

**Interfaces:**
- Consumes: the classes/ids produced by Task 1 (listed above).
- Produces: `style.css`, linked from `index.html` (already wired in Task 1).

- [ ] **Step 1: Write `style.css`**

```css
:root {
  --color-bg: #faf6f1;
  --color-surface: #ffffff;
  --surface-2: #f3ece3;
  --color-text: #2b2420;
  --color-text-muted: #6b6058;
  --color-accent: #a8492a;
  --color-border: #e8ddd0;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
  --space-1: 0.5rem;
  --space-2: 1rem;
  --space-3: 1.5rem;
  --space-4: 2.5rem;
  --radius: 0.75rem;
  --max-width: 640px;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: var(--font-sans);
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.5;
}

main {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: var(--space-2);
}

.icon {
  width: 1.1em;
  height: 1.1em;
  vertical-align: -0.15em;
  margin-right: 0.4em;
  flex-shrink: 0;
}

.hero {
  height: 150px;
  background: var(--color-accent);
  border-radius: var(--radius);
  display: flex;
  align-items: flex-end;
  padding: var(--space-2);
  margin-top: var(--space-2);
}

.hero-label {
  font-size: 0.75rem;
  color: var(--color-accent);
  background: var(--color-surface);
  padding: 0.25rem 0.6rem;
  border-radius: 999px;
}

.welcome {
  margin: var(--space-3) 0 0;
}

.eyebrow {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  letter-spacing: 0.04em;
  margin: 0 0 0.25rem;
}

.welcome h1 {
  margin: 0 0 0.4rem;
  font-size: 1.75rem;
}

.address {
  display: flex;
  align-items: center;
  font-size: 0.9rem;
  color: var(--color-text-muted);
  margin: 0;
}

.quick-facts {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-2);
  margin: var(--space-3) 0 var(--space-4);
}

@media (min-width: 480px) {
  .quick-facts {
    grid-template-columns: repeat(3, 1fr);
  }
}

.quick-fact-card {
  background: var(--surface-2);
  border-radius: var(--radius);
  padding: var(--space-2);
}

.quick-fact-label {
  display: flex;
  align-items: center;
  font-size: 0.8rem;
  color: var(--color-text-muted);
  margin: 0 0 0.4rem;
}

.quick-fact-value {
  font-size: 1.05rem;
  font-weight: 600;
  margin: 0;
}

.quick-fact-sub {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  margin: 0.15rem 0 0;
}

.mono {
  font-family: var(--font-mono);
}

.guide-section {
  border-top: 1px solid var(--color-border);
  padding: var(--space-4) 0;
}

.guide-section h2 {
  margin: 0 0 var(--space-2);
  font-size: 1.375rem;
}

.house-row {
  background: var(--surface-2);
  border: 1px solid var(--color-border);
  border-radius: var(--radius);
  padding: 0.9rem 1.1rem;
  margin-bottom: 0.6rem;
}

.house-row summary {
  display: flex;
  align-items: center;
  cursor: pointer;
  list-style: none;
  font-size: 0.95rem;
  font-weight: 600;
}

.house-row summary::-webkit-details-marker {
  display: none;
}

.house-row summary::after {
  content: "";
  width: 0.5em;
  height: 0.5em;
  border-right: 2px solid var(--color-text-muted);
  border-bottom: 2px solid var(--color-text-muted);
  transform: rotate(45deg);
  margin-left: auto;
  transition: transform 0.15s ease;
}

.house-row[open] summary::after {
  transform: rotate(-135deg);
}

.house-row p {
  margin: 0.6rem 0 0;
  font-size: 0.9rem;
  color: var(--color-text-muted);
}

.tile-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-2);
}

@media (min-width: 480px) {
  .tile-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.tile-photo {
  height: 90px;
  background: var(--surface-2);
  border-radius: var(--radius);
}

.tile-name {
  font-size: 0.9rem;
  font-weight: 600;
  margin: 0.5rem 0 0;
}

.tile-meta {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  margin: 0.15rem 0 0;
}

.host-contact {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  border-top: 1px solid var(--color-border);
  margin-top: var(--space-2);
  padding: var(--space-3) 0 var(--space-4);
}

.host-avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--color-accent);
  color: var(--color-surface);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.85rem;
  flex-shrink: 0;
}

.host-info {
  flex: 1;
  min-width: 0;
}

.host-name {
  font-size: 0.95rem;
  font-weight: 600;
  margin: 0;
}

.host-response {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  margin: 0.15rem 0 0;
}

.host-message {
  display: flex;
  align-items: center;
  white-space: nowrap;
  color: var(--color-accent);
  text-decoration: none;
  font-weight: 600;
  font-size: 0.9rem;
}

.host-message:hover,
.host-message:focus {
  text-decoration: underline;
}
```

This completely replaces whatever `style.css` currently contains — do not merge with the old content, overwrite the file with exactly the above.

- [ ] **Step 2: Verify structure**

Run (from repo root):
```bash
grep -c ':root' style.css
grep -c -- '--surface-2:' style.css
grep -c -- '--font-mono:' style.css
grep -c '\.house-row' style.css
grep -c '@media' style.css
grep -c 'http' style.css
```
Expected: `:root` count is `1`; `--surface-2:` count is `1`; `--font-mono:` count is `1`; `.house-row` count is exactly `6` (the base rule plus 5 selectors built on it: `summary`, `summary::-webkit-details-marker`, `summary::after`, `[open] summary::after`, `p`); `@media` count is `2`; `http` count is `0`.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "Redesign guidebook page styling: cards, accordion, tile grid, host block"
```

---

### Task 3: Responsive, accordion, and link QA pass

**Files:**
- Modify: `index.html` and/or `style.css` — only if this task finds an issue to fix.

**Interfaces:**
- Consumes: the finished `index.html` (Task 1) and `style.css` (Task 2).
- Produces: nothing new — this task is verification, with fixes applied in place if needed.

- [ ] **Step 1: Serve the site locally**

```bash
cd /Users/alexandresokolow/asokolow/airbnb && python3 -m http.server 8000
```
Run this in the background (or a separate terminal) — leave it running for the rest of this task.

- [ ] **Step 2: Screenshot at mobile width**

Use the `browse` skill to open `http://localhost:8000/index.html` at a 375×812 viewport and take a screenshot.

Check:
- Hero placeholder box renders with the "Cover photo" chip visible.
- `.quick-facts` cards stack in a single column, no horizontal overflow.
- `.tile-grid` tiles stack in a single column.
- All four "The house" rows are visible (collapsed) with their icons and titles.
- No broken-image icons anywhere (there should be no `<img>` tags at all).
- All placeholder values are visibly bracketed (e.g. `[DOOR_CODE]`, `[WIFI_PASSWORD]`) — nothing reads like real content.
- The wifi password and door code render in a monospace font, visually distinct from the surrounding text.

- [ ] **Step 3: Screenshot at desktop width**

Use the `browse` skill to open the same URL at a 1280×800 viewport and take a screenshot.

Check:
- Content is centered with visible margins (not stretched full-width).
- `.quick-facts` displays 3 cards in one row.
- `.tile-grid` displays 2 tiles per row (7 tiles total → 3 full rows, last row has 1 tile).

- [ ] **Step 4: Verify accordion behavior**

Using the `browse` skill:
- Click each of the four "The house" `<summary>` rows in turn and confirm each expands to show its detail paragraph, and the chevron rotates/changes orientation.
- Click an expanded row's summary again and confirm it collapses.
- Tab to a `<summary>` via keyboard and press Enter (or Space) to confirm it also expands via keyboard, not just mouse click.
- Confirm expanded content is fully visible — not clipped or cut off by a parent element.

- [ ] **Step 5: Verify the host-contact block**

Confirm the host-avatar circle, host name, response-time text, and "Message" link (with its chat-bubble icon) all render, and that the "Message" link points to `#` (a placeholder, not a real URL).

- [ ] **Step 6: Fix any issues found**

If any check in Steps 2-5 fails, edit `index.html` or `style.css` to fix it, then repeat the relevant check until it passes. If nothing failed, skip this step.

- [ ] **Step 7: Stop the local server**

```bash
kill %1
```
(Or stop whatever process is serving port 8000.)

- [ ] **Step 8: Commit (only if Step 6 made changes)**

```bash
git add index.html style.css
git commit -m "Fix responsive/accordion issues found in redesign QA pass"
```
If Step 6 made no changes, skip this commit — there is nothing to record.

---
