# AGENTS.md

## Purpose
This file is a quick map for understanding what this app is and where to look next.

## Start Here (1-3 minutes)
- `README.md` : project overview, MVP scope, and non-goals.
- `docs/ARCHITECTURE.md` : the core design (SVG.js / Interact.js / svg-pan-zoom separation).
- `docs/DEVELOPER_NOTES.md` : constraints and how to run/debug.

## How the App Works (5-10 minutes)
- `index.html` : the entire app (UI + logic) lives here.
  - File load, select, drag, attribute edit, undo/redo, serialize/download.
  - `<g>` is the minimal editable unit.

## Current Status
- `docs/ISSUE_LIST.md` : implementation status and done conditions.
- `docs/TODO.md` : open decisions and next focus.

## Sample Data
- `test/data/` : SVG samples for testing behavior.

## Notes
- The app is a single HTML file; no build or server is required.
- Keep the SVG structure intact; prefer editing referenced originals over `<use>` instances.
- GitHub Pages is published from the `gh-pages` branch at `/(root)` with `index.html`.
