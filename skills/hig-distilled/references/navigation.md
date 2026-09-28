# Navigation

Choose the simplest structure that fits the content, express it through an adaptive container,
and preserve location, back behavior, and state. Keep every screen orienting and every Back
action trustworthy.

## Contents

- [Four questions every screen must answer](#questions)
- [Navigation models](#models)
- [Choosing a model](#choosing)
- [Containers](#containers)
- [Liquid Glass bars](#bars)
- [Adaptive tab bar and sidebar](#adaptive)
- [Search as navigation](#search)
- [Paths and tokens](#paths)
- [Back and state restoration](#back)
- [Navigation and modality](#modality)

<a id="questions"></a>
## Four questions every screen must answer

Treat these as an acceptance test:

1. **Where am I?** Show a clear title and selected state in persistent navigation.
2. **How did I get here?** Make the path visible or obvious through a breadcrumb, Back label,
   highlighted tab, or equivalent orientation cue.
3. **How do I go back or out?** Provide an expected escape without dead ends or surprise jumps.
4. **How do I reach the primary task in one step?** Keep the screen's purpose directly
   actionable rather than buried in a menu.

<a id="models"></a>
## Navigation models

| Model | Content shape | Movement | Signature control |
|---|---|---|---|
| **Flat** | A few unordered peer sections | Jump between top-level areas | Tab bar, sidebar, segmented switcher |
| **Hierarchical** | Nested levels | Drill in and back one level at a time | Push navigation, reliable Back, breadcrumb |
| **Content-driven** | Document, map, canvas, or media is the space | Move within content while chrome recedes | Minimal, consistently revealed overlays |

Flat navigation is legible and one-tap but does not scale beyond a handful of destinations.
Hierarchical navigation scales but adds distance and disorientation risk, so keep depth shallow
and Back dependable. Content-driven navigation maximizes immersion but requires learnable,
consistent reveal controls.

Compose models when useful: use flat navigation at the top and a hierarchy inside each tab.
Give each tab its own stack so switching away and back restores the previous location.

<a id="choosing"></a>
## Choosing a model

- Start flat when the app fits in 3–5 peer sections.
- Use hierarchy for genuinely nested content. Keep sidebars to two levels before moving into a
  split view; aim for depth 2–3 before content and provide search, recents, or breadcrumbs to
  avoid repeated drilling.
- Use content-driven navigation only when content deserves the full screen, and keep navigation
  cheaply recoverable.
- Do not invent a fourth model for novelty; familiar structures preserve learned behavior.

<a id="containers"></a>
## Containers

The model is the structure; the container is the control that expresses it.

| Container | Best for | Capacity | Constraint |
|---|---|---|---|
| **Tab bar** | Flat top-level switching on compact/touch layouts | About five or fewer | Persistent and thumb-reachable; navigation only |
| **Sidebar** | Flat or shallow hierarchy on wide layouts | Many, groupable | Requires width and height; adapt when narrow |
| **Top bar / toolbar** | Title, Back, and 1–3 contextual actions | Few actions | Do not use for many peer destinations |
| **Path / breadcrumb** | Deep hierarchy where ancestry matters | Reflects depth | Excessive for one or two levels |
| **Menu / overflow** | Secondary, infrequent actions | Spillover only | Hiding primary destinations here is a defect |

- Keep primary destinations persistent. Do not add a “More” tab; reduce or regroup beyond five.
- Use a tab bar for navigation and a toolbar for actions.
- Pair tab labels with filled SF Symbols or coherent icons; reserve badges for critical counts.
- Use a sidebar for richer grouping only when space supports it. Let people customize order and
  do not place critical actions at the bottom edge of a macOS sidebar.
- Read `components.md` for control-specific behavior and platform geometry.

<a id="bars"></a>
## Liquid Glass bars

Place primary navigation on a distinct, legible functional layer above scrolling content.

- A bar may minimize while scrolling but must reappear cheaply, such as at the top or on tab tap.
- Extend content beneath translucent chrome and pad for the bar instead of boxing content off.
- Keep bar treatment distinct from colorful content. Prefer monochrome or restrained chrome so
  labels remain legible over every background.
- Treat fixed geometry and gaze behavior as platform-specific; read `components.md`. Read
  `apple-visual-system.md` for materials, contrast, and reduced-transparency behavior.

<a id="adaptive"></a>
## Adaptive tab bar and sidebar

Keep one navigation model and destination source of truth while changing only its container.

- Render compact destinations as bottom tabs and regular-width destinations as a sidebar or
  rail. Apple's `sidebarAdaptable` also adapts automatically to rotation and resizing.
- Preserve destination names, order, selection, and per-destination state across containers.
- Do not degrade a wide-screen sidebar into a mobile hamburger that hides every primary
  destination. Adapt the container without demoting the information architecture.

<a id="search"></a>
## Search as navigation

Treat search as a primary navigation model when the destination space is large.

- Give important search a dedicated tab or main-toolbar position. Prefer one global search
  location, with optional local filtering inside sections.
- Always show scope. Default scope to the broadest useful option and let people narrow.
- Show recents before typing and predictive results while typing; treat history as private and
  clearable.
- Choose search-as-you-type for inexpensive queries and submit-on-enter for expensive or
  rate-limited queries.
- Design a nonblank no-results state that repeats the query or scope, suggests corrections or
  broader terms, and offers a next step.
- Place search for the current layout and input method. Auto-focus a dedicated search area
  unless a virtual keyboard would obscure its results.

Read `components.md` for field anatomy, scope bars, and tokens; read `patterns.md` for search
behavior and privacy. Apple system indexing maps loosely to global site-wide search.

<a id="paths"></a>
## Paths and tokens

- Use a path control or breadcrumb only for genuinely deep, jump-to-any-level structures. Keep
  first and last segments, truncate the middle, and make every visible ancestor navigable.
  For one or two levels, Back is sufficient.
- Use token fields or filter chips when free text becomes structured, removable, reorderable
  criteria such as type, date, or size. Support autocomplete and per-token actions.
- Read `components.md` for tokenization, context menus, and platform-specific mechanics.

<a id="back"></a>
## Back and state restoration

Back promises to return people to the path and state they left.

- Go to the previous screen in the path taken, not an arbitrary Home. Maintain a separate stack
  for each top-level tab or section.
- Restore scroll position and selection when returning from detail; do not send someone who
  opened item 40 back to the top.
- Preserve in-progress forms, search queries, and filters.
- Restore route, scroll, selection, windows, and unsaved drafts across relaunch so reopening
  feels like resuming. On the web, pair restoration with a first paint matching final layout.
- Follow platform Back conventions. Integrate web navigation with browser history and honest,
  linkable URLs; preserve iOS edge-swipe and other system Back behavior.
- After a deep link, notification, or resumed session, rebuild a sensible parent stack and
  orient the person before Back is used.
- Make meaningful destinations addressable through URLs, universal links, or app links.

<a id="modality"></a>
## Navigation and modality

A modal steps outside the navigation stack, blocks its parent, and demands focused dismissal.
Use it only for a self-contained must-finish-now task or required decision.

- Keep normal browsing, detail drilling, and section switching in the navigation stack.
- Do not stack modals. Provide a visible, conventional, safe exit and guard unsaved content.
- If someone would reasonably want to go **Back** rather than **Cancel**, use a pushed screen
  instead of a modal.

Read `patterns.md` for modality and dismissal details and `components.md` for presentation
surfaces.

