# AGENTS.md

Static single-page website for **Qiskit Fall Fest 2026 x |QET⟩ IIT Hyderabad** — a student-run quantum computing festival (Oct 9–11, 2026). The entire site is `index.html` (~675 lines): markup, inline CSS, inline Tailwind config, and inline JS all live in that one file. Everything else in the repo is brand assets.

## Running / Testing

- No build system, package manager, or test suite exists. Don't look for one; don't add one unless asked.
- Run locally by opening `index.html` in a browser, or `python3 -m http.server` and visiting `http://localhost:8000`.
- External dependencies load from CDNs (Tailwind, Lenis, Google Fonts), so network access is needed for full rendering. Verify changes in **both dark and light themes** (toggle in the nav) and at mobile widths — the layout has separate mobile structures in several places.

## Architecture

One page, no routing. Sections are semantic `<section id="...">` blocks in order: `top` (hero), `about`, `decade`, `schedule`, `speakers`, `hackathon`, `team`, `register`, footer. Nav links in the header, mobile menu, and footer must all stay in sync when sections change.

JS at the bottom of the file handles:

- **Theme toggle** — `data-theme` on `<html>`, persisted to `localStorage`, default dark. The theme is set by an inline script in `<head>` *before paint* to avoid a flash; keep that script first.
- **Lenis smooth scrolling** — page-wide momentum scroll plus eased anchor navigation with a `-80px` offset for the fixed header. Anchor clicks are intercepted by JS; bare `#` hrefs are skipped. Falls back to native smooth scroll under `prefers-reduced-motion`.
- **Schedule tabs** — Fri/Sat/Sun tab buttons toggle `panel-fri` / `panel-sat` / `panel-sun`; the `panels` object maps tab keys to panel IDs.
- **Speaker grid + per-day spotlights** — rendered from the `speakers` array into `#speakerGrid`, and filtered by `days` into each `[data-day-speakers]` box in the schedule panels.
- **Decade timeline** — rendered from the `decade` array; separate desktop (horizontal) and mobile (vertical) templates.

## Key patterns and gotchas

- **Theming via CSS variables, not Tailwind classes.** Colors like `bg`, `panel`, `ink`, `muted` are Tailwind color names mapped to `--bg`, `--panel`, etc. in the inline `tailwind.config`. Changing a theme means editing the CSS variables in the `[data-theme]` blocks at the top — never hard-code light/dark colors.
- **The pink accent `#ff4f9e` is hard-coded in several JS and CSS spots** (tab underline, timeline dots, `color-mix` backgrounds) in addition to the `accent` Tailwind color. Changing the accent requires updating all of them.
- **Theme-aware logos**: `only-dark` / `only-light` classes hide images per theme (IBM Quantum, Qiskit, and QET club logos each have two variants). Any new theme-sensitive imagery needs both variants and both classes.
- **Schedule rows are styled by JS, not markup.** An `<li data-item>` containing `<span data-time>`, `<span data-title>`, `<span data-tag>` gets its grid/hover styling applied at runtime. Add rows with this exact shape. `data-feature` on the `<li>` highlights it in pink (used for the hackathon).
- **Speaker data has two homes that must agree.** Names in the schedule `<li data-title>` should match the `speakers` array entries; the `days` field in that array controls which schedule tabs show the speaker spotlight.
- **Placeholder assets**: speaker photos come from `i.pravatar.cc` (numbered placeholders), and several links (`Discord`, GitHub, speaker websites, registration button) are intentional `#` placeholders — the register button is explicitly disabled with a "Registrations are yet to be opened" note. Don't "fix" these; they're pending real data.
- **Asset paths contain spaces and must be URL-encoded** (e.g. `materials-resources/00_Deliverables/Illustration%20Exports/...`). Preserve the `%20` encoding when adding references.

## Assets

- `materials-resources/` — official Qiskit Fall Fest 2026 brand kit (stickers, hero illustrations, badge, blog images, PowerPoint template, LICENSE). Treat as a brand guideline source, not working files; the site references a handful of these directly.
- `IBM_Quantum/`, `Pictogram/` — IBM Quantum and Qiskit logos in multiple color variants (pick the `_rev`/`_white` variants for dark theme, `_pos`/`black`/`purple` for light).
- `QET_Dark.png` / `QET_Light.png` — club logo variants, referenced from the repo root.

## Commit style

History uses short, imperative, content-focused messages (e.g. "Mark registrations as not yet open", "Schedule: two-track columns after lunch; rename Online Also to Hybrid"). Follow that pattern.
