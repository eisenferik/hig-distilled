# Components — choosing and using controls

Choose controls by behavior and data shape, then apply Apple interaction details and map them
to accessible web/HTML. Treat a wrong component as a usability bug. Preserve the concrete
sizes, counts, timings, platform exceptions, and transferable web rules below.

## Contents

- [Buttons & actions](#buttons--actions)
- [Menus & action surfaces](#menus--action-surfaces)
- [Navigation & search bars](#navigation--search-bars)
- [Presentation surfaces](#presentation-surfaces)
- [Selection & input controls](#selection--input-controls)
- [Lists, tables & collections](#lists-tables--collections)
- [Status & indicators](#status--indicators)

---

## Buttons & actions

A button triggers one action *now*; a link navigates. Keep the roles distinct in look and
behavior — assistive tech announces the role, so a "link" that mutates data breaks it.

**Hierarchy — spend emphasis deliberately.** A button is defined by *style* (size/color/shape),
*content* (symbol, label, or both), and *role* (semantic meaning). Roles: Normal (no meaning),
Primary (default; accent-colored; fires on Return), Cancel, Destructive (system red).

| Tier | Looks like | Use when |
|---|---|---|
| **Primary / prominent** | Solid accent fill | The single most likely action in the view |
| **Secondary** | Tinted or bordered | Common alternatives that shouldn't compete |
| **Tertiary / plain** | Text-only, borderless | Low-stakes or frequent minor actions, toolbar items |
| **Destructive** | Red text/fill, confirmed | Delete/reset — pair color with a clear verb, never color alone |

- **One primary per view** (Apple: keep prominent accent buttons to 1–2). Two equal filled buttons dilute both.
- **Signal the preferred option with *style, not size*.** Among same-sized buttons, emphasis — not a bigger footprint — marks the default.
- **Label with the outcome verb**, title-style caps: "Add to Cart", "Delete 3 Files" — not "OK"/"Submit"/"Yes". A specific verb lets people act without reading surrounding text. See `references/writing.md`.
- **Append `…`** when the action opens another view for more input (e.g. a macOS push button that opens a window) — never on a button that acts immediately.
- **Hit target ≥ 44×44 pt** (60×60 pt visionOS) even when the label is short; pad generously. Web analog: 44 px min tap target.
- **Apple button heights:** Mini 28 / Small 32 / Regular 44 / Large 52 / Extra-large 64 pt. visionOS: centers ≥60 pt apart (regular size then clears ≥16 pt); buttons ≥60 pt also need +4 pt padding so the hover effect can't overlap.
- **Always give a visible press state** — a custom button with no pressed feedback feels dead. macOS/visionOS add a hover tooltip; visionOS uses audio, not haptics.
- **Show pending state.** For a non-instant action, swap the label and show an inline spinner ("Checkout" → "Checking out…") and block a second submit.
- **Icon-only still needs a name** — an accessible label plus (on pointer) a tooltip. Associate familiar actions with familiar SF Symbols; reserve bare glyphs for universally understood ones (close, search, share).
- **Shape** (visionOS): circle = icon-only; capsule = text+icon; rounded-rect/capsule = text-only. On the web, shape follows the design system, but keep the icon-only/round convention for compact toggles.
- **Keep subtypes in the window body.** macOS push/square/gradient/help(`?`)/image buttons don't belong in the toolbar or status bar — use a toolbar item there. A help `?` button is circular and one per window.

- Separate destructive choices spatially from the safe default; confirm irreversible actions and
  prefer Undo where possible. Never assign the Primary role to a destructive action, and keep
  label/background contrast clearly distinguishable.
- Follow the host platform's confirm/cancel order consistently; Apple places confirmation
  trailing/right.

---

## Menus & action surfaces

All menus share one discipline: verb/verb-phrase items, title-style caps, no articles, `…`
when more input follows, groups of related items split by separators (~3 groups max), one
submenu level, destructive items last and red. Every action reachable in a menu should also
be reachable elsewhere. Keyboard shortcuts belong in menu-bar menus, never context menus.

**Which surface:**

| Surface | Use for | Web analog |
|---|---|---|
| **Menu** | A revealed list of commands/states | `role="menu"` / dropdown |
| **Pop-up button** | Choose one *value* from a set; button shows current pick | `<select>` |
| **Pull-down button** | Choose an *action* tied to the button (Add, Sort, More) | kebab / overflow menu |
| **Context menu** (macOS: contextual) | Hidden per-item actions on demand | right-click / long-press menu |
| **Edit menu** | Act on a text/object selection (Copy, Look Up) | selection toolbar |
| **Action sheet** | Choices tied to a just-taken action | see Presentation |
| **Toolbar** | Persistent frequent controls | toolbar |

- **Pop-up vs pull-down** is the key mental model:
  - *Pop-up button* **picks a value** — flat list, no submenus; choosing closes it and updates the button label. Give a useful default and enough context (intro label or descriptive button text) to predict the options without opening.
  - *Pull-down button* **runs an action** — worth opening at ~3+ items; use plain buttons/toggles for 1–2. A destructive item that cannot be undone confirms in a separate location (action sheet on iOS, popover on iPadOS); an undoable one acts immediately and offers Undo.
- **Menus:** unavailable items dim but the menu still opens. Toggled state uses one changeable label (Show Map ↔ Hide Map, add a verb if ambiguous like "Turn HDR On") or a checkmark.
- **Menu layouts (iOS):** Small (top row of 4 icon-only) / Medium (3 icon+label) / Large (all listed, default). Long dynamic menus (History, Bookmarks) may scroll. Split anything long; never nest submenus deeper than one level or ~5 items.
- **Context menus** are revealed by touch/pinch-and-hold, Control-click, or secondary click. Order (and open direction) may flip toward where the finger/pointer opened, so put frequent items first.
- **Hide** unavailable items in a context menu (don't dim) — the opposite of the menu bar. Never show keyboard shortcuts here. iOS/iPadOS: give a row a context menu *or* an edit menu, not both; mark destructive items and list them last.
- **Edit menus** appear on touch-and-hold/double-tap (iOS), pinch-and-hold, or secondary click; iOS shows a horizontal bar, iPadOS opens the context style for pointer input — support both. The system auto-detects data types (address → "Get Directions"). Distinguish Delete (like the key) from Cut (copies then deletes); there's no confirmation, so support undo/redo.
- **Toolbar regions:** **leading** (back, sidebar toggle, title, document menu — not customizable), **center** (common controls, overflow-collapsing), **trailing** (persistent important items, one `.prominent` tinted primary like Done, optional search, More menu).
- **Toolbar behavior:** aim ≤3 groups; the system auto-adds an overflow "More" menu when items don't fit — define overflow order, don't roll your own. iOS large title collapses to a standard title on scroll and returns at the top. Prefer recognizable symbols over text; separate text buttons with fixed space so they don't read as one. On macOS every toolbar item must also be a menu-bar command.
- **Menu bar** (macOS/iPadOS, 24 pt) is the authoritative command surface — order AppName, File, Edit, Format, View, [app-specific], Window, Help.
  - Put every app command here even if available elsewhere (discoverability, shortcuts, Full Keyboard Access); disable — don't hide — unavailable items so people learn what's possible.
  - Dynamic items change on one modifier (Option: Close → Close All) but must never be the only path to a task.
  - iPadOS bar is hidden until revealed (pointer to top edge or swipe down) — ensure every function is reachable in-UI.
- **Liquid Glass:** toolbars and bars ride the floating glass layer above content. Reduce custom backgrounds/tints and let the content layer inform color; use the scroll-edge effect for separation (see `references/apple-visual-system.md`). Standard components get corner radii concentric with the bar; match custom ones.

**Apple-specific surfaces** (transferable idea in parens): **share sheet / activity view** — one recognizable share entry point; dismisses when a task starts, report long-task status in-app (Web Share API); **Home Screen quick actions** — ≤4 high-value, outcome-titled, predictable shortcuts (PWA shortcuts/jump lists); **Dock menus** (jump lists) and **ornaments** (visionOS floating control panel = sticky toolbar) duplicate in-app commands at the launcher/edge.

---

## Navigation & search bars

Current HIG lists five: tab bars, sidebars, search fields, path controls, token fields — all now
in the floating **Liquid Glass** layer above content. General navigation IA lives in
`references/navigation.md`.

**Tab bar ↔ sidebar (the adaptive pattern).**
- Tab bar = persistent primary nav to **≤5** top-level destinations, each preserving its own state. Sidebar = the same for richer hierarchies (**2 levels max**), when there's room.
- Use the `sidebarAdaptable` style so one information architecture renders as a sidebar in regular width and collapses to a tab bar when compact — the transferable responsive rule.
- Placement is hardware-specific: iOS tab bar floats at the **bottom** on translucent glass; iPadOS sits near the **top** and can convert to a sidebar via a button; visionOS is always **vertical**, leading-edge, expanding on eye gaze; tvOS bar is fixed 68 pt high, 46 pt from top. Not on watchOS.

**Tab-bar behavior:**
- Navigation only, never actions (use a toolbar for controls acting on the current view). Keep it visible across sections (a modal may cover it).
- Short single-word labels; a badge (red oval, white number/`!`) only for critical info; prefer *filled* SF Symbols.
- **Liquid Glass minimize:** on scroll-down the bar can minimize and pull an accessory (e.g. a Music MiniPlayer) inline, restoring on tab-tap or scroll-to-top.
- Avoid overflow/"More" tabs — reduce the count instead. Don't disable or hide tab buttons for empty sections; explain the empty state instead.
- iPadOS tabs are user-customizable; keep a default of ≤5 for compact↔regular continuity.

**Sidebar behavior:**
- Show/hide via edge-swipe (iPadOS) or a button / View-menu command (macOS); it can auto-collapse as the window resizes.
- Content extends beneath it via horizontal scroll or the **background extension effect** (`backgroundExtensionEffect()`, mirrors adjacent content to look stretched under the glass).
- Icons default to the app accent color (honor the *system* accent on macOS); sparing fixed colors (Mail's yellow VIP) can clarify meaning.
- Let people customize contents/order; keep it discoverable (don't hide by default); avoid critical items at the macOS bottom edge (people hide it). Use disclosure controls to group; jump to a split view past two levels.
- Web analog: a collapsible left rail that becomes a bottom tab bar at narrow widths.

**Search fields.**
- Anatomy: leading Search icon, placeholder (convey scope), trailing Clear button; optional **scope bar** (segmented category filter — default broad, let people narrow) and **tokens** (chip-ified terms, selectable/editable as a unit, paired with suggestions).
- Prefer **search-as-you-type**: show recents before typing and predictive suggestions during.
- iOS entry points: as a **tab** (standard = a search landing page for discovery; button appearance = focuses the field and shows the keyboard immediately), in a **toolbar** (bottom preferred for reach, or top as a button), or **inline** to filter one view (pin to top on scroll).
- Auto-focus in a dedicated search area — except iPad with only a virtual keyboard (the keyboard would cover the view). Keep the experience consistent across an app's iPad and Mac versions.
- Web analog: `<input type="search">` with instant results, recents, and scope/token filters positioned near the content it searches.

**Path controls & token fields (both macOS-only):**
- **Path control** = a breadcrumb trail (root → parents → selected item); hides middle segments when long, click any ancestor to navigate, place in the window body not the frame. Web analog: a breadcrumb with middle-segment truncation.
- **Token field** = free text that becomes reorderable, individually-actionable chips on comma/Return, with an autocomplete list (tunable delay) and a per-token context menu (Mail recipients). Web analog: a tags/chips input; tokens as search filters appear cross-platform via search fields.

---

## Presentation surfaces

Choosing a container is choosing a modality — how hard it interrupts and how it dismisses.
See `references/patterns.md` (Modality) for the underlying decision.

| Surface | Modality | Reach for when |
|---|---|---|
| **Sheet** | Modal (iOS can be nonmodal) | A short, scoped task tied to the current context |
| **Popover** | Transient | A few related options/controls, anchored to a trigger |
| **Alert** | Blocking | Critical, actionable, time-sensitive info — sparingly |
| **Action sheet** | Modal | Choices tied to a just-taken action (esp. destructive) |
| **Panel** (macOS) | Floating | An inspector for the current selection |
| **Page control** | Inline | Position in a small ordered set of peer pages |
| **Window** | — | The app surface itself (iPadOS/macOS/visionOS) |

**Sheets** slide in for a scoped task tied to the current context.
- iOS/iPadOS **detents** (rest heights): **large** (full), **medium** (~half). Adding medium enables progressive disclosure; specifying medium-only prevents full expansion. iPadOS also has fixed page/form styles centered on a dimmed background.
- A **grabber** (top indicator) lets people drag or tap to cycle detents (and works with VoiceOver). The sheet expands as content scrolls.
- **Swipe down to dismiss** is expected — if there are unsaved changes on swipe, confirm with an action sheet.
- Modal everywhere except iOS/iPadOS, which can be **nonmodal** (affect the parent without dismissing, e.g. Notes formatting).
- Buttons: Cancel/Close (leading), Done (trailing), Back (a step, not a dismiss). Pair Done with a Cancel or Back so completion isn't the only exit; don't show all three together.
- One sheet at a time. For long/complex flows prefer a full-screen modal or a new window. Web analog: a modal dialog or bottom-sheet with snap points.

**Popovers** are transient overlays with an arrow pointing at the trigger; sized to contents (animate any resize so it doesn't read as a replacement).
- Dismiss on outside tap or item selection — add Close/Done only when it adds clarity (save vs discard) or multi-select needs the popover to stay open.
- Always **save** on auto-close; discard only on an explicit Cancel (an outside tap can be accidental).
- **Only one at a time**, never stacked; nothing displays over a popover except an alert. Not for warnings (people miss them — use an alert).
- iOS/iPadOS: avoid in *compact* widths — use a full-screen sheet. macOS popovers can be detachable into a panel. Web analog: a popover/dropdown that closes on outside click.

**Alerts** are blocking modals for critical, actionable, time-sensitive info.
- Content: title (keep to **≤ 2 lines**), optional text, **≤ 3 buttons**. Default button (trailing/top) fires on Return — omit a default if people must actually read the alert.
- Include Cancel whenever there's a destructive action; use verb-based labels ("View All", "Reply") and reserve "OK" for purely informational alerts.
- Avoid alerts that aren't actionable, alerts at launch, and alerts for common undoable deletes. Web analog: `<dialog>`, used rarely.

**Action sheets** present a short choice set anchored to a just-taken action.
- Destructive style sits at the **top** (most noticeable); Cancel sits at the **bottom** — the safe out.
- Present only one action sheet at a time. Don't use one as a plain menu or make it scroll
  because either increases mis-taps. watchOS caps it at 4 buttons total; SwiftUI
  `confirmationDialog` adds Cancel automatically. Not in visionOS.

**Page controls** = ≤ ~10 dots for an ordered flat set of peers; solid dot = current; centered near the bottom.
- Tap the current dot's leading/trailing side for prev/next; touch-and-drag to *scrub* continuously. Animate transitions on tap, not while scrubbing (scrubbing is fast — animating each step lags).
- Styles: Automatic (background only during interaction), Prominent (always shown), Minimal (position-only, no scrub feedback — don't scrub with it).
- Not for hierarchy (use a split view) or >10 items (use a grid). Not on macOS.

**Panels** (macOS only) float above windows as an inspector that auto-updates with the selection.
- Usually omit the minimize button; refer to it by title ("Show Fonts"), not the word "panel". Bring all panels forward when the app activates; hide them when inactive.
- HUD variant is dark/translucent for media apps. On other platforms use a modal/split-view/sheet for the same supplementary content.

**Windows** (iPadOS/macOS/visionOS) use system chrome and state cues.
- Never replicate the frame/controls (imperfect matches feel broken and lose automatic state updates); adapt fluidly to size; open new windows deliberately, not by default.
- macOS states: Main/Key (colored controls, materials effect) vs Inactive (gray, materials dropped). Don't put critical actions at the bottom edge.
- visionOS windows default 1280×720 pt on unmodifiable glass; set min/max sizes; use a volume for rich 3D. Not on iOS/tvOS/watchOS. See `references/patterns.md` for modality.

---

## Selection & input controls

Match the control to the *shape* of the data — binary vs short list vs long list vs free text —
and give exact-value entry a text companion when ranges are wide.

| Data shape | Control | Web analog |
|---|---|---|
| Two opposing states, immediate effect | **Toggle / switch** | `role="switch"` |
| Hierarchical / multi-select with a master | **Checkbox** (macOS) + tri-state | `<input type=checkbox>` |
| 2–5 mutually exclusive | **Radio group** (macOS) / **segmented control** | radios / segmented tabs |
| Short mutually-exclusive set (visible) | **Segmented control** | button group |
| One value from a set (space-tight) | **Pop-up button** | `<select>` |
| Medium–long list of values | **Picker** | scrolling/menu picker |
| Free text, mostly-known values | **Combo box** (macOS) | autocomplete input |
| Small ± numeric nudge | **Stepper** (+ field) | number input + spin |
| Continuous value in a range | **Slider** | `<input type=range>` |
| Small text input | **Text field** | `<input>` |

- **Toggles** flip state *immediately* (no separate confirm).
  - Make the state difference obvious via fill/background/checkmark — **never color alone** (accessibility).
  - iOS: use the switch style **only in a list row** (the row supplies context, no label needed); outside a list use a toggle-styled button.
  - Checkbox tri-state (macOS): a master shows a dash when its children differ. Radio groups suit 2–5 mutually exclusive options.
  - macOS: put switches/checkboxes/radios in the window body, never the toolbar; don't swap an existing checkbox for a switch.
- **Segmented controls**: 2+ equal-width segments, ~5–7 max (≤5 on iPhone).
  - One mode only — all-selection *or* all-momentary-action, never mixed. Text *or* images, not both; keep segment sizes consistent.
  - iOS: switch closely related subviews (use a tab bar for separate app *sections*). macOS: view-switching in toolbars/inspectors (a tab view for main-window sections).
- **Pickers**: scrollable constrained-value lists; value order is locale-driven, so don't hard-code it. Show in context (bottom or popover), never on a separate screen.
  - iOS date-picker styles: Compact (button → modal calendar), Inline, Wheels, Automatic; modes Date / Time / Date-and-time / Countdown (≤23h59m).
  - Coarsen minute granularity (e.g. 15-min) when full precision isn't needed. Use a picker for medium–long lists; a pull-down for short; a list/table for very large.
- **Sliders**: filled track, min leading/bottom → max trailing/top; give live preview as the thumb moves.
  - Pair with a text field + stepper for exact values over wide ranges. macOS adds tick marks (label only min/max) and a circular style.
  - iOS: don't use for audio volume (use a volume view). Prefer horizontal, especially visionOS.
- **Steppers** show no value themselves — always pair with a visible value, and add direct numeric entry for big jumps (macOS Shift-click = larger step).
- **Text fields**: single-line; size the field to the expected amount as a visual cue. Stack related fields with consistent widths.
  - Placeholder shows only when empty — pair with a persistent label when the purpose must stay visible. Use a **secure** field for passwords; a **number formatter** to constrain/format (locale-dependent).
  - Validation timing is contextual: email → on leaving the field; username/password → before leaving. iOS: trailing Clear button, contextual keyboard type.
  - For long/editable text use a **text view** (`<textarea>`); make useful read-only text (errors, IPs, serial numbers) selectable.
- **Virtual keyboards** (touch platforms): pick the keyboard *type* matching the content (email keyboard adds `@` / `.com`); customize the Return key when it clarifies the task (Search); provide an obvious keyboard switch (Globe key) and don't duplicate system keys. Custom keyboard extensions work everywhere except secure and phone-number fields.
- **Specialized inputs** with direct web analogs: **color wells** (swatch → picker, live current color, shared palette), **image wells** (macOS drag-drop image drop-zone with copy/paste/clear + default), **combo boxes** (macOS editable text + suggestion list — free entry allowed but not added to the list; use a colon-terminated intro label), **digit-entry views** (tvOS full-screen masked PIN pad).

---

## Lists, tables & collections

- **List vs collection vs table:** list/table for **text** (easier to scan in a scrollable column); collection/**grid** for images and widely varying sizes; table for **multi-column** comparison; outline/column for **hierarchy**.
- **Row anatomy** (the reusable pattern): **leading** media (small image/symbol) + **label** (primary + secondary text) + **trailing** accessory.
- **The two trailing accessories people conflate:** the **info button** ⓘ (detail disclosure) *shows more info, does not navigate*; the **disclosure indicator** › (chevron) *drills into the next level, does not show detail*. Keep them distinct. Don't pair a trailing A–Z index with trailing row controls — they collide.
- **Selection & editing.** iOS/iPadOS require an explicit **edit mode** to select rows; support reorder even when add/remove isn't allowed.
- **Selection feedback signals purpose:** a navigation table **persistently highlights** the selected row (shows the current path); an options table highlights **briefly** then adds a **checkmark**. Web analog: persistent highlight for master-detail, checkmark for multi-select.
- **Collection gestures:** tap to select, touch-and-hold to edit, swipe to scroll; animate insert/delete/reorder as feedback; don't reflow layout mid-interaction. Do not nest two scroll views that move in the same orientation.
- **Styles & density.** iOS grouped style (headers/footers/space separate groups); macOS bordered style with **alternating row backgrounds** to track values across wide tables. Keep row text succinct.
- **macOS table behavior:** click a heading to sort, click again to reverse; let people resize columns; prefer a centered ellipsis (keeps start+end) over end-truncation in narrow tables. Multi-column headings: nouns/short noun phrases, title-style caps, no ending punctuation.
- **Split views (master–detail).** Adjacent panes (sidebar + detail, optional third) showing multiple hierarchy levels at once; **persistently highlight** the current selection in each leading pane.
  - macOS: draggable dividers (thin = **1 pt**, preferred), sensible min/max sizes, collapsible panes with multiple restore paths.
  - iOS/iPadOS: use only in **regular** width — collapse to a single-column drill-down when compact (the critical responsive rule). visionOS: prefer a split view over a new window for supplementary info.
- **Disclosure controls.** A leading **triangle** points inward when collapsed, down when expanded (for a view/list); a **disclosure button** points down→up (next to a control). Keep advanced options hidden by default with a descriptive label ("Advanced Options"); one expander per view. Web analog: `<details>/<summary>`, keeping the right-when-collapsed / down-when-open rotation.
- **Deep hierarchy (macOS):** **outline views** (tree-table) and **column views** (Miller columns) — expose hierarchy in the first column, persist expansion state, Option-click expands all; degrade to a split view / drill-down list on touch or narrow widths.
- **Containers & text:** **boxes** group related content with a border/background + optional title (keep them small relative to the container; use padding over nested boxes — web: a card/`<fieldset>`). **Labels** carry a 4-level neutral text ramp — primary → secondary → tertiary → quaternary — use it for importance instead of inventing grays; make useful label text selectable.
- **Tab views** (macOS) switch **≤6** mutually-exclusive self-contained panes via a top tab strip (iOS/iPadOS use a segmented control instead; watchOS renders them as swipeable page controls).

---

## Status & indicators

- **Progress vs activity.** Use a **determinate** progress bar/ring (fills leading→trailing / clockwise) when duration is known; an **indeterminate** spinner (activity indicator) when it isn't. Keep it moving — a frozen indicator reads as stalled; if it truly stalls, explain why. Switch indeterminate→determinate once you know the duration, but **never** switch bar↔circular shape (jarring). Even out the pace (no 90%-fast-then-crawl). Offer **Cancel** when interruption is safe, **Pause** when it has side effects, and warn before a cancel that loses progress. iOS pull-to-refresh is a supplement, not a replacement for periodic auto-refresh. Fully transferable.
- **Gauges** show one value in a range on a circular/linear path; styles **standard** (indicator marks the value) and **capacity** (fill stops at value). Use **continuous** fill for large ranges, **discrete** segments only for small ones; a gradient can encode meaning (red→blue = hot→cold). Label the current value and both endpoints. Web analog: meters/dials/capacity bars.
- **Rating indicators**: whole symbols only (round, never partial), evenly spaced, editable inline. Keep the star unless a custom symbol's meaning is unmistakable. Universal on web.
- **Badges** = small filled ovals with an unread count; keep current (clearing to zero removes related Notification Center items); never use badges for non-notification data.

**Apple-only system experiences** (recognizable by their fixed treatment — do not replicate the proprietary look on the web; the transferable principle is noted):

- **Activity rings** — Apple Fitness Move/Exercise/Stand triple ring with locked RGB colors on black; never restyle. *Principle: a fixed, never-restyled data glyph preserves trust.*
- **Widgets** — glanceable Home/Lock-Screen cards. Standard margin **16 pt** (tight groupings 11 pt), text **≥ 11 pt**, container-relative corner radius, deep-link to the exact screen (don't just relaunch), placeholder content while loading, animate updates ≤ **2 s**. *Principle: glanceable dashboard cards, single purpose, meaning without color.*
- **Live Activities** — real-time tracker for an event with a start/end (<~8 h). Presentations: compact / minimal / expanded / Lock Screen; Dynamic Island corner radius **44 pt**. Update only on change, deep-link on tap, set a custom dismissal (15–30 min), animations ≤ 2 s. *Principle: a live status card that ends when done.*
- **Notifications** — consented, glanceable, ≤ **4** action buttons; suppress when the app is foreground (update quietly instead); title-case title, sentence-case body; don't duplicate, don't use for errors (use an alert). *Principle: consent, preview privacy, foreground suppression, don't spam — all transfer to web push.*
- **Controls / App Shortcuts / Snippets / Complications / Top Shelf / Watch faces** are Apple-surface-specific. Transferable ideas only: standalone symbols that reflect live state and redact when locked (Controls); expose top tasks with single-parameter simplicity (App Shortcuts); confirm-vs-result cards with descriptive primary labels (Snippets); dense glanceable tiles with per-tile deep links (Complications); a focus-driven hero carousel (Top Shelf).




