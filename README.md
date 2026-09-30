# LX Index

A single-file, offline-friendly web tool for making **printable index sheets**: a 200-slot A4 index grid, matching cover pages, registry volumes (A–Z books), and a landscape monthly review table. Everything stays in the browser (localStorage). There's no account, no server and no tracking.

## Views (tabs in the side panel)

| Tab | What it is | Prints as |
|---|---|---|
| **Grid** | 4 × 50 editable index grid (No. 1–200), per-cell bold/italic/underline/size/colour, search, duplicate detection, undo/redo, version history, block copy/cut/paste, Excel/CSV import, Excel export | A4 portrait |
| **Cover** | 10 cover-page styles (Editorial, Official, Architect, …) with editable Arabic/English wording, colours and sizes | A4 portrait |
| **Registry** | Grid snapshots saved as named volumes, auto-paginated, 10 typographic styles, search, CSV export | A4 portrait, multi-page |
| **Review** | Editable RTL reconciliation table (entities × months), transpose, add/remove rows & columns | A4 landscape |

Entry point: `index.html` (no query parameters). The last view you had open is restored on reload.

## Completed in this pass
**Bug fixes**
- The Review sheet (1123px wide) was cut off on laptops and phones. All A4 sheets now zoom to fit the workspace and print at 100%.
- In dark mode the Review sheet went black, and because of `print-color-adjust: exact` it **printed** black too. It now stays paper-white like the other sheets.
- Grid shortcuts leaked into other views. Ctrl+A / C / X / V / B / I / U were being hijacked on the Cover inputs and Review table. They are now grid-only, and native copy of selected text inside a cell works.
- Ctrl+B/I/U inside a grid cell used to insert rich-text tags through the browser. The app's own style toggle now handles them.
- The universal `*{font-family:Inter}` rule overrode the fonts of every cover and registry theme element that didn't set its own font. Fonts now inherit properly.
- Modal hide timers could hide a modal that had just been reopened (race condition). Fixed. Modals also close on backdrop click and Esc, and Enter confirms.
- The confirm action now runs *after* the modal closes, so chained modals no longer get hidden.
- The Excel export button lost its icon after exporting. Fixed.
- Toasts double-escaped text, so names with `&` or quotes showed up as `&amp;`. Fixed.
- Printing from the browser menu skipped the page-size/orientation setup. Fixed.
- CDN scripts are deferred and ExcelJS is pinned to 4.4.0. Import and template buttons now handle the case where the library is still loading.
- "Clear registry" now has Undo. "Clear grid" snapshots a version first.
- Paint mode is cancelled when you leave the Grid view.
- Removed `user-scalable=no` so pinch-zoom works again.

**UI (less "SaaS")**
- Warm paper and ink palette with one deep-green accent, replacing the indigo/pink gradients, glass blur and glow shadows.
- Lora serif for headings and stats. Binder-style tabs instead of pill buttons. Plain-language panel names ("Your data", "Look", "Lines & margins", …).
- Motion is quiet (no bounce, no hover lifts), and `prefers-reduced-motion` is respected.
- Dark, compact toast with an inline Undo action. Soft amber banner when looking at an old version.
- Mobile: a "Tools" pill replaces the gradient floating button. Toast and selection toolbar sit clear of it. Modals scroll.
- Favicon, meta description, theme-color and visible focus rings.

## Data & storage (localStorage keys)
- `lx_data_ul_v4` – grid values `{id: text}`
- `lx_style_ul_v4` – per-cell styles `{id: {b,i,u,s,c}}`
- `lx_settings_ul_v4`, `lx_active_theme_v4` – grid look
- `lx_versions_v4` – last 10 snapshots
- `lx_clip_v4` – last 10 clipboard blocks
- `lx_registry_v4` – `[{book, id, name}]`
- `lx_cover_v4` – cover wording/theme/colours
- `lx_review_v4` – review table model
- `lx_tm` – app theme (auto/light/dark)
- `lx_view_v5` – last open view

The existing v4 keys were left unchanged, so current users keep their data.

## Not implemented / ideas
- Backup/restore of *all* data as one JSON file (useful when moving to another browser)
- Arabic UI strings exist in `STRINGS.ar` but there's no language switch yet
- Undo/redo for the Cover and Review views (Review only has undo for deletes)
- The Review table has no Excel export

## Deploy
Static site: one `index.html` plus CDN libraries (SheetJS, ExcelJS, Google Fonts). Use the **Publish** tab.
