# Inputs and interaction

Support keyboard, pointer, touch, gestures, focus, assistive technology, and relevant hardware
in parallel. Give every capability more than one sensible input path and degrade gracefully when
one input is absent. Never make hover, drag, a gesture, or specific hardware the only route.
Apply the Flexibility principle with `references/principles.md` and
`references/accessibility.md`.

## Contents

- [Keyboard](#keyboard)
- [Pointer](#pointer)
- [Touch and gestures](#touch-and-gestures)
- [Focus and selection](#focus-and-selection)
- [Apple-specific hardware inputs (brief)](#apple-specific-hardware-inputs-brief)
- [Cross-platform takeaways](#cross-platform-takeaways)

## Keyboard

Full keyboard operability is table stakes: every function reachable and every control operable with the keyboard alone. It's the substrate assistive tech (switch access, voice control, screen readers) rides on, and power users depend on it.

- **Make everything reachable and escapable.** Apple's Full Keyboard Access model means the whole UI is navigable by keyboard. Never trap focus inside a component or modal; keyboard focus must be able to leave by a conventional path. One platform nuance: on iPadOS, add explicit keyboard *navigation* for **content** (text fields and views, sidebars, collection/custom views) but let Full Keyboard Access handle **controls** (buttons, segmented controls, switches) — don't double-implement activation for standard controls.
- **Honor standard shortcuts; invent few.** Keep the platform's conventions intact (⌘C copy, ⌘V paste, ⌘Z undo, ⇧⌘Z redo, ⌘A select all, ⌘F find, ⌘S save, ⌘, settings, ⌘? help). Only redefine a standard shortcut when its action is irrelevant to your app (e.g. reuse ⌘I if you have no text styling). Define custom shortcuts only for your most frequent app-specific commands.
- **Compose shortcuts correctly.** A shortcut is a primary key plus modifiers; list modifiers in canonical order **Control, Option, Shift, Command**. Command is the main modifier; Shift is secondary/complementary; Option is for sparing, power-user features; avoid Control (system-reserved). Don't add Shift to reach a two-character key's upper symbol (⌘? not ⇧⌘/), and don't build a new shortcut by adding a modifier to an unrelated one (don't make ⇧⌘Z mean something other than redo).
- **Use modifiers as people expect** during direct manipulation: ⌘-drag moves as a group, ⇧ drag-resize constrains aspect ratio, holding an arrow key nudges by the smallest unit. The system auto-localizes and RTL-mirrors shortcuts for you.
- **Text entry:** pick the right on-screen keyboard for the field — email, number, URL, phone, etc. — so people get the right keys and validation cues without hunting. This is the single highest-leverage keyboard detail on touch devices.

## Pointer

Treat the pointer (trackpad/mouse) as **additive** — a precision layer on top of touch and keyboard on iPad and Vision Pro, and the primary input alongside keyboard on Mac — never a touch replacement.

- **Give elements the right hover/content effect.** iPadOS morphs the pointer to fit context (a circle by default, an I-beam over text) and offers three content effects: **highlight** (translucent rounded-rect + gentle parallax — default for bar buttons, tab bars, segmented controls; use for small transparent-background elements), **lift** (parallax + scale-up + shadow, pointer fades — default for app icons and Control Center buttons; use for small opaque elements), and **hover** (custom scale/tint/shadow, pointer keeps its shape — use for large elements). Match the effect to the element; don't add gratuitous decorative effects, and never do shadow-without-scale.
- **Precise, generous targets.** The pointer's hit region extends *beyond* the visible bounds: pad ~12 pt around beveled elements and ~24 pt around non-beveled visible edges, and rely on magnetism (the pointer snaps to the likely target's center) for lift/highlight elements and text. Don't leave gaps between adjacent bar-button hit regions, and don't scale elements that have no room to grow (e.g. tight table rows).
- **Cursors communicate.** Use context-appropriate cursors from the standard set — arrow, I-beam over text, pointing hand over links, resize arrows, open/closed hand, crosshair, not-allowed. Keep any custom pointer simple; don't print instructional text on the pointer.
- **Secondary (right) click and modifiers.** Support secondary-click for contextual menus, and keep modifier behavior consistent between touch and pointer (Option-drag duplicates either way).
- **Drag.** Support click-drag to move and band-select (click-drag across items to select a range). Let the pointer reveal auto-hiding controls (a Safari toolbar, video playback controls) on hover — but the same actions must remain reachable without a pointer.

## Touch and gestures

Keep the standard gesture vocabulary conventional and always back it with a visible, tappable equivalent.

- **Standard gestures, standard meanings:** Tap = activate/select; Swipe = reveal/dismiss/scroll; Drag = move; Touch-and-hold = reveal extra controls; Double-tap = zoom; Pinch = magnify; Rotate = rotate. Don't repurpose a familiar gesture for an app-unique action, and don't invent a new gesture for a standard action.
- **Immediate, predictive feedback.** A gesture should respond as it happens with feedback that previews the result, and should visibly indicate when it's unavailable — otherwise the UI looks frozen or broken.
- **Custom gestures must be discoverable, easy, and distinct — and never the only path.** Teach them in context and **mirror them in visible UI**: a swipe-to-go-back still needs a tappable Back button; a swipe-to-delete still needs a reachable Delete control. Shortcut gestures *supplement* standard controls.
- **Comfortable targets.** For touch, frequently-used controls should be at least **44×44 pt**; less-important ones (menus) at least **28×28 pt**. Prevent gaps or overlaps between adjacent hit regions, especially near destructive controls. See `references/accessibility.md` for the full target-size rationale.
- **Don't override system gestures.** Edge swipes, app switchers, and reveal-Home/Control-Center gestures belong to the OS; clashing with them breaks the platform's muscle memory and traps people.

## Focus and selection

Focus visually confirms which element the current input (keyboard, remote, controller, gaze) is targeting. It matters most for keyboard use and for 10-foot / TV interfaces where there's no pointer.

- **Keep a clear, consistent focus indicator.** Use a **focus ring/halo** for text and search fields; use a **highlight** (e.g. white text on an accent-color background) for list rows and collection items. Prefer the platform's tuned focus effects over custom ones.
- **Make the right things focusable per platform.** On tvOS/visionOS, directional focus must reach *every* interactive element, so make everything focusable and supply larger assets for the scaled focused state (tvOS uses a parallax depth effect and up to five states: unfocused, focused, highlighted/pressed, selected, unavailable). On iPadOS/macOS, Full Keyboard Access already handles controls — you only add focus for *content* elements (list items, text/search fields), not buttons, sliders, or toggles.
- **Order and grouping.** Set focus order in reading order (leading→trailing, top→bottom). iPadOS uses focus *groups*: Tab moves between groups (sidebar, grid, list), arrow keys move within a group. Raise an item's priority to make it a group's default (auto-focused) item.
- **Focus usually selects — but not always.** Skip auto-select where it would cause a jarring context shift. Never move focus without user interaction; the one exception is when a discrete directional input (keyboard/remote/controller) removes the focused item, in which case move focus to a nearby survivor rather than dropping it. Preserve selection and scroll position when returning to a list (see `references/navigation.md`).

## Apple-specific hardware inputs (brief)

Each of these is **Apple-hardware-specific**; carry across the underlying idea, not the hardware.

| Input | Hardware | What it does | Transferable idea |
|---|---|---|---|
| **Digital Crown** | Apple Watch, Vision Pro | Primary rotary navigation and 1-D value scrubbing, with haptic detents; watchOS scrolls lists and paginates, visionOS controls volume/immersion | A rotary/scroll input for precise 1-D scrubbing and vertical nav; speed-match the response, give visible feedback, always provide a non-rotary fallback |
| **Apple Pencil + Scribble** | iPad stylus | Precise, pressure/tilt/azimuth-sensitive marking; Scribble converts handwriting to text inline in any text field | A precise pointer should feel instant and directly manipulative, be hand-agnostic (don't hide controls under the hand), and never force a mode switch to write |
| **Remotes (Siri Remote)** | Apple TV | Across-the-room, focus-driven navigation; distinguishes deliberate *press* from incidental *tap* | 10-foot / focus-driven UI where directional input moves a clear focus indicator; ignore inadvertent touches; keep "back" hierarchical |
| **Game controllers** | iOS/tvOS/macOS/visionOS | Gamepad input with a standard nav mapping (A = activate, B = cancel/back, Menu = pause) | Support gamepad + keyboard + pointer + touch simultaneously, remappable, with meaningful iconography and A=confirm/B=cancel conventions; always ship a default-input fallback |
| **Camera Control** | iPhone 16 | Pressure/slide edge control that launches the camera and adjusts capture via an overlay | A hardware/edge affordance for a primary flow with a lightweight adjustable overlay; keep chrome out of the content area; remember last-used state |
| **Action button** | iPhone 16, Watch Ultra | One hardware button bound (in Settings) to a user-chosen action | A single fast path to one high-frequency, user-chosen action; keep its result glanceable and non-destructive |
| **Eyes / gaze** | Vision Pro | Looking targets an element; the system shows a hover effect to confirm | Generous hit spacing and rounded targets aid any imprecise pointing; prefer subtle, low-motion feedback; never rely on knowing a pre-click hover for logic |

Key design constraints worth carrying over even off-device:

- **Pencil/Scribble:** mark the instant the tip touches — no mode or button first; hover can *preview* the mark but must never trigger an action (imprecise); keep the text field stationary and non-autoscrolling while someone writes, and hide distracting autocomplete. Never wire content-modifying or destructive actions to accidental gestures (double-tap, squeeze).
- **Gaze:** allow at least a **16 pt** margin around interactive items or keep centers **≥ 60 pt** apart; rounded shapes are easier to target (eyes drift to corners). The OS never tells your app where someone looks before they tap — a privacy model that generalizes to "don't build logic on a pre-commit hover."
- **Action button:** wire it to essential, frequent, non-destructive actions (people press without looking); let the system teach configuration rather than duplicating Settings' tips.
- **Motion sensors (gyroscope/accelerometer):** use device motion only for a tangible benefit (fitness, gameplay), gate it behind a permission prompt with your own purpose copy, and never make it the sole way to navigate — motion gestures are hard to reproduce and exclude some people.

## Cross-platform takeaways

Build the accessible web equivalent by default.

| Apple concept | Web / cross-platform analog |
|---|---|
| Full Keyboard Access, standard shortcuts | Native focusable elements; honor platform shortcuts (⌘ on Mac / Ctrl on Win/Linux); `accesskey` sparingly; keep custom shortcuts few |
| Right on-screen keyboard per field | `inputmode` and `type` (`email`, `tel`, `url`, `numeric`, `decimal`), `autocomplete`, `enterkeyhint` |
| Pointer content effects (highlight/lift/hover) | `:hover` states scoped to fine pointers via `@media (hover: hover) and (pointer: fine)`; don't attach essential behavior to hover |
| Touch vs. pointer distinction | `@media (pointer: coarse)` for touch sizing; `pointer:fine` for precise; feature-detect rather than sniff UA |
| Comfortable targets (44/28 pt) | Minimum ~44×44 px hit targets with spacing; pad the hit area beyond the visible glyph |
| Secondary click, modifier drags | `contextmenu` event; keep modifier semantics consistent across input types |
| Standard gestures with a visible fallback | Pair swipe/drag with real buttons; treat gestures as enhancement over an accessible baseline |
| Focus indicator + logical order | `:focus-visible` (never remove the outline), DOM order = reading order, roving `tabindex` for composite widgets |
| Never steal focus / respect motion | Manage focus on route/modal change, return it to the trigger; `prefers-reduced-motion`; low peripheral motion |
| Motion sensors as opt-in enhancement | `DeviceMotion`/`DeviceOrientation` behind an explicit permission and a clear benefit, never core navigation |




