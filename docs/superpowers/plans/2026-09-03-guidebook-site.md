# Guidebook Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a minimalistic single-page guest guidebook (`index.html` + `style.css`) with placeholder content, per the approved design spec.

**Architecture:** One static HTML page with a header/nav and four anchor-linked `<section>`s (check-in, wifi, parking, nearby), styled by one CSS file. No JavaScript, no build step, no dependencies.

**Tech Stack:** Vanilla HTML5 + CSS3 only. No frameworks, no npm, no CDNs, no external fonts (system font stack).

**Spec:** `docs/superpowers/specs/2026-09-03-guidebook-site-design.md`

**Testing note:** This is a static content site with no test framework. "Test" steps below are (a) structural `grep` checks that the required markup exists, and (b) a real-browser QA pass in Task 3 using the `browse` skill — not unit tests. There is no red/green cycle for Tasks 1-2; write the file, then verify its structure.

## Global Constraints

- Vanilla HTML/CSS only — no JavaScript, no frameworks, no npm, no build step (CLAUDE.md).
- No external CDNs or web fonts — system font stack only (CLAUDE.md, spec).
- All placeholder values are bracketed, e.g. `[DOOR_CODE]` (spec, Content sections).
- Placeholder links use `href="#"` with an inline HTML comment marking them TODO — never a real or dead external URL (spec, Error handling).
- No `<img>` tags for missing photos — use text/emoji/CSS instead (spec, Error handling).
- Mobile-first layout; must not break at ~375px or ~1200px+ widths (spec, Testing).
- Work happens on branch `feature/guidebook-site` — do not commit to `main` (CLAUDE.md, Git workflow).
- Commits must never mention Claude/AI authorship — no `Co-Authored-By: Claude`, no "Generated with Claude Code" (CLAUDE.md, Git workflow).
- Repo root: `/Users/alexandresokolow/asokolow/airbnb`. `index.html` and `style.css` live at the repo root (GitHub Pages serves root of `main`).

---

### Task 1: HTML structure and content

**Files:**
- Create: `index.html`

**Interfaces:**
- Consumes: nothing (first task).
- Produces: `index.html` with a `<link rel="stylesheet" href="style.css">` in `<head>` (Task 2 creates the file this points to), and these ids for Task 2's CSS to target: `#checkin`, `#wifi`, `#parking`, `#nearby`; these classes: `.site-header`, `.tagline`, `.section-nav`, `.guide-section`, `.info-list`, `.card-grid`, `.card`, `.site-footer`.

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
  <header class="site-header">
    <h1>[PROPERTY_NAME]</h1>
    <p class="tagline">Everything you need for your stay</p>
    <nav class="section-nav" aria-label="Guide sections">
      <a href="#checkin">Check-in</a>
      <a href="#wifi">Wifi</a>
      <a href="#parking">Parking</a>
      <a href="#nearby">Nearby</a>
    </nav>
  </header>

  <main>
    <section id="checkin" class="guide-section">
      <h2>Check-in &amp; House Rules</h2>
      <dl class="info-list">
        <dt>Check-in</dt>
        <dd>After [ARRIVAL_TIME] — door code: <strong>[DOOR_CODE]</strong></dd>
        <dt>Check-out</dt>
        <dd>Before [CHECKOUT_TIME]</dd>
      </dl>
      <h3>House rules</h3>
      <ul>
        <li>No smoking indoors</li>
        <li>Quiet hours after [QUIET_HOURS_START]</li>
        <li>No parties or events</li>
      </ul>
    </section>

    <section id="wifi" class="guide-section">
      <h2>Wifi &amp; Essentials</h2>
      <dl class="info-list">
        <dt>Network</dt>
        <dd>[WIFI_NETWORK_NAME]</dd>
        <dt>Password</dt>
        <dd>[WIFI_PASSWORD]</dd>
      </dl>
      <p>Extra towels and linens are in [TOWEL_LOCATION]. Trash and recycling bins are [TRASH_LOCATION] — pickup is [TRASH_DAY].</p>
    </section>

    <section id="parking" class="guide-section">
      <h2>Parking &amp; Getting There</h2>
      <p>[PARKING_INSTRUCTIONS — e.g. "Park in the driveway; street parking is also available after 6pm."]</p>
      <p><a href="#"><!-- TODO: replace with real maps link -->View on map</a></p>
    </section>

    <section id="nearby" class="guide-section">
      <h2>Restaurants &amp; Activities</h2>
      <h3>Restaurants</h3>
      <div class="card-grid">
        <article class="card">
          <h4>[RESTAURANT_1_NAME]</h4>
          <p>[One-line description — cuisine, why we like it]</p>
        </article>
        <article class="card">
          <h4>[RESTAURANT_2_NAME]</h4>
          <p>[One-line description]</p>
        </article>
        <article class="card">
          <h4>[RESTAURANT_3_NAME]</h4>
          <p>[One-line description]</p>
        </article>
      </div>
      <h3>Activities</h3>
      <div class="card-grid">
        <article class="card">
          <h4>[ACTIVITY_1_NAME]</h4>
          <p>[One-line description]</p>
        </article>
        <article class="card">
          <h4>[ACTIVITY_2_NAME]</h4>
          <p>[One-line description]</p>
        </article>
        <article class="card">
          <h4>[ACTIVITY_3_NAME]</h4>
          <p>[One-line description]</p>
        </article>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <p>Questions during your stay? Text [HOST_NAME] at [HOST_PHONE]</p>
  </footer>
</body>
</html>
```

- [ ] **Step 2: Verify structure**

Run (from repo root):
```bash
grep -c 'id="checkin"' index.html
grep -c 'id="wifi"' index.html
grep -c 'id="parking"' index.html
grep -c 'id="nearby"' index.html
grep -c '<img' index.html
grep -c 'href="style.css"' index.html
```
Expected: first four commands each print `1`; `<img` count prints `0` (no image tags per spec); the stylesheet link count prints `1`.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add guidebook page structure and placeholder content"
```

---

### Task 2: CSS styling

**Files:**
- Create: `style.css`

**Interfaces:**
- Consumes: the ids/classes produced by Task 1 (`#checkin`, `#wifi`, `#parking`, `#nearby`, `.site-header`, `.tagline`, `.section-nav`, `.guide-section`, `.info-list`, `.card-grid`, `.card`, `.site-footer`).
- Produces: `style.css`, linked from `index.html` (already wired in Task 1).

- [ ] **Step 1: Write `style.css`**

```css
:root {
  --color-bg: #faf6f1;
  --color-surface: #ffffff;
  --color-text: #2b2420;
  --color-text-muted: #6b6058;
  --color-accent: #c1613b;
  --color-border: #e8ddd0;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
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

.site-header {
  padding: var(--space-4) var(--space-2) var(--space-3);
  text-align: center;
  border-bottom: 1px solid var(--color-border);
}

.site-header h1 {
  margin: 0 0 var(--space-1);
  font-size: 1.75rem;
}

.tagline {
  margin: 0 0 var(--space-3);
  color: var(--color-text-muted);
}

.section-nav {
  display: flex;
  justify-content: center;
  gap: var(--space-3);
  flex-wrap: wrap;
}

.section-nav a {
  color: var(--color-accent);
  text-decoration: none;
  font-weight: 600;
}

.section-nav a:hover,
.section-nav a:focus {
  text-decoration: underline;
}

main {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 var(--space-2);
}

.guide-section {
  padding: var(--space-4) 0;
  border-bottom: 1px solid var(--color-border);
}

.guide-section:last-child {
  border-bottom: none;
}

.guide-section h2 {
  margin-top: 0;
  font-size: 1.375rem;
}

.guide-section h3 {
  font-size: 1.05rem;
  color: var(--color-text-muted);
  margin-bottom: var(--space-1);
}

.info-list {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: var(--space-1) var(--space-2);
  margin: var(--space-2) 0;
}

.info-list dt {
  font-weight: 600;
  color: var(--color-text-muted);
}

.info-list dd {
  margin: 0;
}

.card-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-2);
  margin: var(--space-2) 0 var(--space-3);
}

@media (min-width: 480px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius);
  padding: var(--space-2);
}

.card h4 {
  margin: 0 0 var(--space-1);
}

.card p {
  margin: 0;
  color: var(--color-text-muted);
  font-size: 0.9rem;
}

.site-footer {
  text-align: center;
  padding: var(--space-3) var(--space-2) var(--space-4);
  color: var(--color-text-muted);
  font-size: 0.9rem;
}
```

- [ ] **Step 2: Verify structure**

Run (from repo root):
```bash
grep -c ':root' style.css
grep -c '\-\-color-accent' style.css
grep -c '\.card-grid' style.css
grep -c '@media' style.css
grep -c 'http' style.css
```
Expected: first four commands each print `1` or more; the `http` count prints `0` (confirms no external CDN/font URL was introduced).

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "Add warm minimalistic styling for the guidebook page"
```

---

### Task 3: Responsive and link QA pass

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
- Single-column layout, no horizontal scrollbar/overflow.
- Header, nav, and all four section headings (`Check-in & House Rules`, `Wifi & Essentials`, `Parking & Getting There`, `Restaurants & Activities`) are present when scrolling top to bottom.
- Restaurant/activity cards stack in a single column.
- No broken-image icons anywhere (there should be no `<img>` tags at all).
- All placeholder values are visibly bracketed (e.g. `[DOOR_CODE]`, `[WIFI_PASSWORD]`) — nothing reads like real content that could be mistaken for a live value.

- [ ] **Step 3: Screenshot at desktop width**

Use the `browse` skill to open the same URL at a 1280×800 viewport and take a screenshot.

Check:
- Content is centered with visible margins (not stretched full-width).
- Restaurant/activity cards display 2 per row within each `.card-grid`.

- [ ] **Step 4: Verify anchor navigation**

Using the `browse` skill, click each of the four nav links (Check-in, Wifi, Parking, Nearby) in turn and confirm the page scrolls to the matching section and the URL fragment updates to `#checkin`, `#wifi`, `#parking`, `#nearby` respectively.

- [ ] **Step 5: Fix any issues found**

If any check in Steps 2-4 fails, edit `index.html` or `style.css` to fix it, then repeat the relevant check until it passes. If nothing failed, skip this step.

- [ ] **Step 6: Stop the local server**

```bash
kill %1
```
(Or stop whatever process is serving port 8000.)

- [ ] **Step 7: Commit (only if Step 5 made changes)**

```bash
git add index.html style.css
git commit -m "Fix responsive/navigation issues found in QA pass"
```
If Step 5 made no changes, skip this commit — there is nothing to record.

---
