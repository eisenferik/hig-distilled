# Official HIG sources & how to fetch them

Use Apple's live HIG for **authoritative, current, platform-specific** metrics, component behavior, and new APIs. These references summarize Foundations, Patterns, Components, and Inputs; live pages change over time and are ground truth when they differ.

## How to fetch HIG content (the reliable way)

HIG pages are client-rendered JavaScript, so plain fetching `/design/human-interface-guidelines/<slug>` returns an almost-empty shell, while browser-rendering many pages hits tab limits and cross-navigation races. **Fetch the DocC JSON instead:**

```
https://developer.apple.com/tutorials/data/design/human-interface-guidelines/<slug>.json
```

- Returns overview, best practices, specifications, platform considerations, and change log as structured JSON via `curl` or WebFetch: HTTP 200, ~40–100 KB per page.
- Requires no JavaScript or browser and is safe to parallelize.
- Examples: `.../design/human-interface-guidelines/sheets.json`, `.../buttons.json`, `.../typography.json`.
- Read prose from `primaryContentSections` / `sections`; read title, platforms, and latest-change `alert-text`/`alert-date` from `metadata`.

> Use `/tutorials/data/design/…`, **not** `/tutorials/data/documentation/…`, which returns 404 for HIG.

For a single-page interactive fallback, use a browser (`preview_start` → `get_page_text`) and read in sections. Prefer JSON beyond a few pages.

## URL pattern (human pages)

```
https://developer.apple.com/design/human-interface-guidelines/<slug>
```
`<slug>` is the kebab-cased title, such as `design-principles`, `app-icons`, `dark-mode`, `sf-symbols`, or `undo-and-redo`. Section indexes: `/foundations`, `/patterns`, `/components`, `/inputs`, `/technologies`, `/getting-started`. Component-group indexes: `/menus-and-actions`, `/navigation-and-search`, `/presentation`, `/selection-and-input`, `/content`, `/layout-and-organization`, `/status`, `/system-experiences`.

## Map of the HIG (read for this skill, mid-2026)

**Design principles** — `/design-principles` (the 8 principles; reintroduced 2026-06-08).

**Foundations (18):** accessibility, app-icons, branding, color, dark-mode, icons,
images, immersive-experiences, inclusion, layout, materials, motion, privacy,
right-to-left, sf-symbols, spatial-layout, typography, writing.
→ **materials** documents the current **Liquid Glass** language.

**Patterns (25):** charting-data, collaboration-and-sharing, drag-and-drop,
entering-data, feedback, file-management, going-full-screen, launching,
live-viewing-apps, loading, managing-accounts, managing-notifications, modality,
multitasking, offering-help, onboarding, playing-audio, playing-haptics,
playing-video, printing, ratings-and-reviews, searching, settings, undo-and-redo,
workouts.

**Components (by group):**
- Menus and actions — buttons, menus, context-menus, edit-menus, the-menu-bar,
  toolbars, pop-up-buttons, pull-down-buttons, activity-views (share sheets),
  home-screen-quick-actions, dock-menus, ornaments
- Navigation and search — tab-bars, sidebars, search-fields, path-controls, token-fields
- Presentation — action-sheets, alerts, page-controls, panels, popovers,
  scroll-views, sheets, windows
- Selection and input — color-wells, combo-boxes, digit-entry-views, image-wells,
  pickers, segmented-controls, sliders, steppers, text-fields, toggles, virtual-keyboards
- Content — charts, image-views, text-views, web-views
- Layout and organization — boxes, collections, column-views, disclosure-controls,
  labels, lists-and-tables, lockups, outline-views, split-views, tab-views
- Status — progress-indicators, activity-rings, gauges, rating-indicators
- System experiences — notifications, widgets, live-activities (incl. Dynamic Island),
  controls, app-shortcuts, snippets, status-bars, complications, top-shelf, watch-faces

**Inputs (13):** action-button, apple-pencil-and-scribble, camera-control,
digital-crown, eyes, focus-and-selection, game-controls, gestures,
gyro-and-accelerometer, keyboards, nearby-interactions, pointing-devices, remotes.

**Technologies** — Apple-specific services such as CarPlay, HealthKit, Wallet, Sign in with Apple, and App Clips. Consult when targeting one; most guidance is integration-specific rather than visual/UX design.

## Staleness notes

- The 8 design principles were **reintroduced 2026-06-08**; Liquid Glass color/material
  guidance updated through late 2025 / 2026. If a live page's change log differs from a
  reference here, trust the live page and tell the user.
- Slugs shift (`gyroscope-and-accelerometer` → `gyro-and-accelerometer`;
  `spatial-interactions` → `nearby-interactions`). If a slug 404s, open the nearest
  section/group index and follow its links.




