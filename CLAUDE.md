# CLAUDE.md

Instructions for Claude Code working in this repository.

## Project

This repo is a static **guest guidebook** for an Airbnb rental, published via
**GitHub Pages**. It gives guests everything they need for their stay:
accommodation details, house rules, parking instructions, nearby restaurants,
local activities, check-in/check-out info, wifi, emergency contacts, etc.

The audience is a guest on their phone, often with a weak connection — the
site must be fast, readable, and usable without any app or login.

## Tech stack — vanilla only, no build step

- Plain **HTML + CSS**. Use plain **JavaScript** only where genuinely
  necessary (e.g. a mobile nav toggle, an image gallery/lightbox) — never as
  a default.
- **No frameworks** (React, Vue, Tailwind, Bootstrap, etc.), **no npm, no
  bundler, no build step**. GitHub Pages serves the repo's files as-is, and
  this project stays that simple on purpose.
- No backend, no database. If something needs to be dynamic, prefer static
  data (e.g. a small JSON/JS file) over a service.

## Workflow — use superpowers skills

For any non-trivial page, section, or feature work in this repo, follow the
superpowers workflow rather than jumping straight to code:

1. **`superpowers:brainstorming`** — always start here, even for small
   changes. It classifies the work (spike / bounded / architectural) and
   gets explicit approval on the design before anything is built.
2. **`superpowers:writing-plans`** — for anything multi-step, turn the
   approved design into a written implementation plan before touching code.
3. **`superpowers:subagent-driven-development`** /
   **`superpowers:dispatching-parallel-agents`** — use subagents to execute
   the plan, especially when independent chunks can proceed in parallel
   (e.g. building separate guidebook sections, or content vs. styling).
   Pick the subagent model per the tiering table below.
4. **`superpowers:requesting-code-review`** — before considering any work
   done.
5. **`superpowers:verification-before-completion`** — before claiming
   anything is fixed, complete, or ready to merge (actually open the page /
   check the diff, don't just assert it).

Do not skip straight to implementation on the assumption that "it's just
HTML/CSS" — the brainstorming skill's approval gate applies here too, scaled
to a short in-chat design for small changes.

## Subagent model tiering

When dispatching subagents, pick the model based on task complexity — don't
default to the biggest model for everything:

| Complexity | Model                     | Example tasks                                                        |
|------------|----------------------------|-----------------------------------------------------------------------|
| Low        | `claude-haiku-4-5-20251001` | Copy edits, alt text, link/typo sweeps, file moves/renames            |
| Medium     | `claude-sonnet-5`           | Build a page section, style a component, a responsive pass            |
| High       | `claude-opus-5`             | Design system decisions, information architecture, cross-page refactors |

## Quality bar

- **Mobile-responsive** — this is read on phones first.
- **No dead links** — verify links to maps, restaurant sites, etc.
- **Lightweight** — no heavy assets; compress/resize images before adding them.
- **Basic accessibility** — semantic HTML, alt text on images, sensible
  heading structure.

## Git workflow

- Work happens on feature branches, not directly on `main`. Open a PR to
  merge into `main`; the user merges.
- **Never mention Claude/AI authorship in commits** — no `Co-Authored-By:
  Claude`, no "Generated with Claude Code" trailers, no mention of Claude in
  commit messages. Write commit messages as if authored normally by the
  repo owner.
