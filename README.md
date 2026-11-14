# TijdBewust — Dagdeel Planner

A lightweight, offline-first Dutch web application for intentional half-hour time-blocking of your free time (**"Bewust omgaan met je vrije tijd"**).

## Overview

`TijdBewust` is built around a self-contained HTML file ([`index.html`](index.html), with [`bewuste_dagdeel_planner.html`](bewuste_dagdeel_planner.html) redirecting to it) with zero build steps, zero external CDN dependencies, and automatic `localStorage` persistence (with optional GitHub Gist cloud sync). It can be opened directly from disk (`file://`) or hosted on GitHub Pages / served over HTTP/HTTPS as an installable offline PWA via [`manifest.webmanifest`](manifest.webmanifest), [`sw.js`](sw.js), and [`icon.svg`](icon.svg).

## Features

- **Date-Keyed Multi-Day Planning**: Quick tabs for **Vandaag** and **Morgen**, previous/next day buttons, and a native date picker (`YYYY-MM-DD` keys in `localStorage`, with automatic migration from legacy `vandaag`/`morgen` data).
- **Configurable Time Window (`Dagdeel`)**: Adjust the visible start (`00:00`–`23:00`, default `08:00`) and end (`01:00`–`24:00`, default `22:00`) hours in 30-minute increments.
- **Click, Touch-Drag & Keyboard Selection**:
  - Click or drag across 30-minute blocks on desktop, or drag along the left time column on touchscreens.
  - Full keyboard navigation on timeslots (`Space`/`Enter` to toggle, `ArrowUp`/`ArrowDown` to move, `Shift+ArrowUp`/`Shift+ArrowDown` to extend selection, `Escape` to clear).
  - Mobile floating action bar and native `<dialog>` bottom-sheet modal.
- **Customizable Activity Templates (`Mijn vaste activiteiten`)**:
  - Pre-populated with 6 defaults (*Lezen*, *Wandelen / Buiten*, *Sporten*, *Creatief project*, *Ontspannen / Rust*, *Opruimen / Klusje*).
  - Add, inline-edit (name & color), reorder (`↑`/`↓`), or delete templates across 6 color themes (*Salie*, *Hemel*, *Zon*, *Koraal*, *Lavendel*, *Neutraal*).
- **One-Off Custom Activities & Notes**: Assign ad-hoc activities with a chosen color and optional short note, or attach/edit notes on any scheduled block.
- **Contiguous Block Grouping & Live Indicator**: Adjacent slots with the same activity display a combined duration pill (e.g., `1,5 uur`), and today's active 30-minute block is highlighted with a live **Nu** badge.
- **Progress Tracking**: Check off completed blocks (`Afvinken` / `Gedaan`) and view a live summary of planned vs. completed blocks and hours.
- **Undo / Redo**: Header history buttons, `Ctrl/Cmd+Z` & `Ctrl/Cmd+Shift+Z` shortcuts, and toast undo actions.
- **Dark Mode**: Follows your system `prefers-color-scheme` by default via CSS `color-scheme` and `light-dark()` tokens, with a header toggle stored in `localStorage`.
- **Multi-Format Export & JSON Backup (`Exporteren`)**:
  - Export the active day as **Tekst (`.txt`)**, **Markdown (`.md`)**, **CSV (`.csv`)**, or **Agenda / iCalendar (`.ics`)**.
  - Download or restore a full JSON backup of all schedules, templates, and time-window settings.
- **Reset (`Resetten`)**: Clear either the active day or all saved days while preserving your activity templates.

## Getting Started

1. Open [`index.html`](index.html) directly in any modern web browser, or visit the GitHub Pages site at `https://pbaarn.github.io/dagplanner/`.
2. *(Optional)* Serve the folder locally to enable PWA installation and the offline service worker:
   ```bash
   python3 -m http.server 8000
   ```
   Then visit `http://localhost:8000/`.

## Data Storage & GitHub Gist Sync

By default, all application state is stored locally in `localStorage`:
- `tijdbewust_state_v1`: Active date, time window (`startHour`, `endHour`), selected colors, activity templates (`templates`), `YYYY-MM-DD`-keyed schedules (`schedules`), and `updatedAt` timestamp.
- `color-scheme`: Optional `"light"` or `"dark"` theme override (removed when matching system default).
- `tijdbewust_gist_config_v1`: Optional GitHub Gist sync configuration (`mode`, `token`, `gistId`, `lastSyncedAt`).

To sync across devices, click **Lokaal** in the header, switch to **GitHub Gist**, and enter a GitHub Personal Access Token with `gist` scope (leaving **Gist ID** blank creates a new private `tijdbewust-data.json` Gist automatically). `localStorage` remains active as a local cache.
