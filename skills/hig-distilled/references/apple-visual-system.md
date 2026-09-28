# Apple visual system

Apply Apple's concrete visual language—type, semantic color, spacing, shape, materials,
icons, and motion—to Apple platforms and faithful web approximations. Use
`../assets/apple-tokens.css` as a starting point. Read `inputs-and-interaction.md`,
`components.md`, and `accessibility.md` for behavior and accessibility details.

## Contents

- [Typography](#typography)
- [Color](#color)
- [Spacing, grid, and layout](#layout)
- [Shape and concentricity](#shape)
- [Materials and depth](#materials)
- [Iconography](#icons)
- [Motion](#motion)
- [Pre-ship visual checks](#checks)

<a id="typography"></a>
## Typography

Use San Francisco as the sans-serif workhorse: SF Pro, SF Compact for watchOS, SF Mono for
code and numerics, and SF Rounded for a softer tone. Use New York for editorial serif text.
These are variable fonts with optical sizing; SF also supports Condensed and Expanded widths.
Match SF Symbols to the adjacent SF weight.

Use named text-style roles rather than raw point sizes so Dynamic Type can rescale the system.
iOS default Large sizes are **point size / leading**:

| Role | iOS pt/leading | Weight | Job |
|---|---|---|---|
| Large Title | 34 / 41 | Regular; Bold for emphasis | Screen identity, once per screen |
| Title 1 | 28 / 34 | Regular | Major section header |
| Title 2 | 22 / 28 | Regular | Section header |
| Title 3 | 20 / 25 | Regular | Subsection header |
| Headline | 17 / 22 | **Semibold** | Emphasized item in a group |
| Body | 17 / 22 | Regular | Default reading text |
| Callout | 16 / 21 | Regular | Supporting text near body |
| Subhead | 15 / 20 | Regular | Grouped-list group text |
| Footnote | 13 / 18 | Regular | Secondary metadata |
| Caption 1 | 12 / 16 | Regular | Timestamps and labels |
| Caption 2 | 11 / 13 | Regular | Densest metadata |

- Use the smaller macOS scale—approximately Large Title 26/32 through Body 13/16—and match
  native control fonts. macOS has no Dynamic Type.
- Respect default/minimum readable sizes: iOS 17/11pt, macOS 13/10pt, tvOS 29/23pt,
  visionOS 17/12pt, and watchOS 16/12pt.
- Let tracking adjust with point size: positive when tiny, near zero around 12pt, and slightly
  negative through the teens. Reproduce it manually only in static mockups.
- Use Regular, Medium, Semibold, and Bold. Avoid Ultralight, Thin, and Light at small sizes or
  in bright environments; size up if a light weight is essential.
- Build hierarchy with both size and weight. Near-identical size steps without weight contrast
  read as mistakes.

### Dynamic Type

Keep hierarchy constant as text grows to the largest accessibility sizes. At AX5, Body is
about 28pt and Large Title about 44pt. Let labels wrap instead of clipping; stack formerly
inline layouts and drop columns when needed. Not every element must scale equally—for example,
tab titles need not follow body text. Scale meaningful icons with text. Test with Larger
Accessibility Text enabled.

### Web mapping

Use the system stack so Apple devices render SF and other platforms fall back gracefully:

```css
font-family: -apple-system, BlinkMacSystemFont, system-ui, "Segoe UI", Roboto, sans-serif;
```

Build a role-based `rem` scale on the browser default, approximately 16px, so Body resolves
to approximately 17px:

| Role | size / line-height | weight |
|---|---|---|
| Large Title | 2.125rem / 1.21 | 400 |
| Title 1 | 1.75rem / 1.21 | 400 |
| Title 2 | 1.375rem / 1.27 | 400 |
| Title 3 | 1.25rem / 1.25 | 400 |
| Headline | 1.0625rem / 1.29 | 600 |
| Body | 1.0625rem / 1.29 | 400 |
| Callout | 1rem / 1.31 | 400 |
| Subhead | 0.9375rem / 1.33 | 400 |
| Footnote | 0.8125rem / 1.38 | 400 |
| Caption 1 | 0.75rem / 1.33 | 400 |
| Caption 2 | 0.6875rem / 1.18 | 400 |

Use `rem`, unitless line height, and reflowing layouts. Do not place enlarged or translated
text in fixed-height boxes. Use `clamp()` for fluid display headings. The full token values are
in `../assets/apple-tokens.css`.

<a id="color"></a>
## Color

Apple supplies Red, Orange, Yellow, Green, Mint, Teal, Cyan, Blue, Indigo, Purple, Pink, and
Brown in light, dark, increased-contrast light, and increased-contrast dark variants, plus six
system grays. Apple publishes swatches rather than stable hex values; reference semantic roles
instead of hard-coding release-dependent colors.

| Family | Roles | Meaning |
|---|---|---|
| Background | `systemBackground` → secondary → tertiary, plus grouped variants | Primary view, groups within it, then nested groups |
| Foreground | `label` → secondary → tertiary → quaternary; `placeholderText`, translucent and opaque separators, `link` | Descending emphasis and specialized content roles |
| Fill | `fill` → secondary → tertiary | Tinted backgrounds for small controls and grouped shapes |

macOS exposes additional AppKit roles including label tiers, control colors, selected-content
background, separators, and window background. Use those roles rather than visual lookalikes.

### Accent and meaning

- Choose one interaction tint for buttons, selection, and active controls. On macOS, respect
  the user's system accent except where a meaning-bearing sidebar icon requires its own color.
- Keep one color tied to one meaning. Do not use the interaction tint on noninteractive text.
- Never rely on color alone; add text, a glyph, or a shape for state, selection, and status.
- Account for cultural meaning, including locale-specific danger, luck, and market colors.
- Do not repurpose semantic roles, such as using a separator color for text.

### Dark mode

Remap semantic roles rather than inverting pixels. Use dimmer backgrounds and brighter
foregrounds while preserving hierarchy. iOS and iPadOS distinguish base and elevated
background sets; a popover, sheet, or multitasking surface advances by switching to the
lighter elevated set in dark mode. Meet 4.5:1 contrast and aim for 7:1 on small text. Supply
light, dark, and both increased-contrast variants for every custom color, even when an app
targets one appearance. Respect the system appearance instead of adding an app-specific
light/dark toggle.

### Liquid Glass tint

Glass has no inherent color; it samples content behind it. Tint it sparingly for status or a
primary action. For a prominent button, tint the background rather than its label. Prefer
monochrome bars over rich or busy media, and do not tint several controls at once.

### Web mapping

Define every theme-dependent value as a semantic custom property and reference it everywhere:

```css
:root { --label:#000; --bg:#fff; --bg-secondary:#f2f2f7; --separator:#3c3c4349; }
@media (prefers-color-scheme: dark) { :root { --label:#fff; --bg:#000; --bg-secondary:#1c1c1e; } }
```

Follow `prefers-color-scheme`. Use a lighter surface rather than a larger shadow for dark-mode
elevation, and dim large saturated fills to avoid glare. Treat the hex values above and in the
token kit as web approximations, not stable Apple API values.

<a id="layout"></a>
## Spacing, grid, and layout

Reuse a small spacing scale and treat space between controls as deliberately as control size.
Group related items with space first, then use a background, material, or separator if needed.
Use published component metrics where applicable—for example, 14pt Lock Screen margins for
Live Activities and at least 16pt clear space around visionOS controls.

Constrain long text with the platform readable-width guide rather than stretching it across a
wide window. Leave essential information room and keep controls comfortably hittable.

### Safe areas

Extend backgrounds and full-bleed artwork to physical edges, but inset interactive controls
from bars and hardware such as Dynamic Island and camera housings. Preserve tvOS overscan with
60pt top/bottom and 80pt side insets. On visionOS, keep interactive centers at least 60pt apart
and at least 16pt of clear space between controls.

On the web, combine `viewport-fit=cover` with
`env(safe-area-inset-top/right/bottom/left)`, constrain reading text near 60–75ch, and keep
comfortable hit targets near 44px.

### Adaptivity

Treat each dimension as regular or compact. iPhones are compact-width/regular-height in
portrait; large iPhones can become regular-width in landscape; iPads are regular/regular.
Design the largest and smallest layouts first. Adapt rather than stretch: reflow columns,
change a sidebar to a tab bar, and hide tertiary inspectors first. Delay compact mode until the
full layout no longer fits to keep the UI stable.

On the web, map these concepts to container or media queries and responsive component
breakpoints rather than device names.

### Reading order, localization, and assets

- Flow top-to-bottom and leading-to-trailing; place the most important content top-leading.
- Mirror layouts for RTL locales with logical CSS properties. Fit artwork to new aspect ratios
  without distorting it or losing essential content through careless cropping or letterboxing.
- Supply @2x/@3x raster assets for iOS, @1x/@2x for macOS and tvOS, and @2x for iPad and watch.
  Prefer SVG/PDF for icons and flat art; PNG for lossless UI graphics; JPEG, HEIC, or WebP for
  photos. Embed a color profile and place vector control points on whole values.
- On the web, use `srcset`, `<picture>`, SVG, and CSS `image-set()`.

<a id="shape"></a>
## Shape and concentricity

Use continuous superellipse or squircle corners rather than treating every corner as a simple
circular arc. Keep nested rounded surfaces concentric so their curves remain parallel:

> **inner radius = outer radius − padding**

A radius-20 card with 12 units of padding therefore gives its inner element radius 8. Incorrect
curves pinch or drift and read as unfinished. Keep sibling radii consistent and scale radii
proportionally with surface size so controls do not become accidental pills and sheets do not
look unnaturally hard. Let the system mask app-icon corners; do not pre-mask them.

On the web, derive inner radii, such as
`--radius-inner: calc(var(--radius-lg) - var(--space-1))`. Native CSS radii are circular;
approximate continuous curves with a slightly larger radius or use an SVG/`clip-path`
superellipse where the distinction matters. Reuse a small radius scale.

<a id="materials"></a>
## Materials and depth

Use materials to separate the floating functional layer from content. Liquid Glass belongs on
navigation, bars, sidebars, and controls above scrolling content. Use standard materials for
grouping within content. Do not place Liquid Glass in the content layer, except transiently on
an active control to communicate interaction.

### Glass variants

| Variant | Use when | Behavior |
|---|---|---|
| Regular, default | A background could hurt legibility or the surface contains substantial text | Blur and luminosity adjustment preserve foreground readability |
| Clear | Rich photo or video should remain visible | High translucency; add a 35% dark dimming layer behind bright content, but skip it for already-dark content or controls with their own dimming |

Choose standard material thickness by contrast need, not apparent color:

| Thickness | Translucency | Use |
|---|---|---|
| `ultraThin` | Highest | Maximum background context and minimal text |
| `thin` | High | Light overlays |
| `regular` | Balanced | Default for most surfaces |
| `thick` | Lowest | Fine text requiring strong contrast |

Use system-defined vibrant colors on materials. Apply label vibrancy from `label` through
`quaternaryLabel`, but avoid quaternary labels on thin and ultraThin materials. Fill and
separator vibrancy work on every material.

### Depth and elevation

Use a scroll-edge effect—blur plus reduced content opacity near a bar—instead of a hard line
between content and chrome. Small glass bars can adapt light/dark to the content beneath while
keeping symbols monochrome; larger sidebars should be more opaque for legibility. Test Reduce
Transparency and Increase Contrast, both of which can collapse glass to a solid surface.

| Tier | Surface | Light mode | Dark mode |
|---|---|---|---|
| Base content | Flush cards and list rows | Separator only | No shadow |
| Raised | Resting card or grouped section | Soft, low, diffuse shadow | Slightly lighter fill |
| Floating chrome | Toolbar, tab bar, sticky header | Scroll-edge effect or hairline | Translucent glass |
| Overlay | Popover, sheet, menu, alert | Larger soft shadow plus scrim | Elevated background plus scrim |

Use a small shadow ramp rather than per-component shadows. In dark mode, use a lighter surface
instead of a larger shadow.

### Web mapping

```css
.chrome {
  background: var(--material-chrome-bg);
  backdrop-filter: var(--material-blur);
  -webkit-backdrop-filter: var(--material-blur);
}
@media (prefers-reduced-transparency: reduce) {
  .chrome { background: var(--bg); backdrop-filter: none; }
}
```

Keep blur on functional chrome, add an approximately 35% dark scrim under text over bright
media, and select thickness by contrast need. Treat Liquid Glass APIs and platform material
names as Apple-specific; transfer the distinct, legible floating-layer behavior.

<a id="icons"></a>
## Iconography

SF Symbols align with SF across nine weights and three cap-height-relative scales. Choose among
four rendering modes: Monochrome, Hierarchical opacity, Palette with explicit layer colors,
and intrinsic Multicolor. Use variable color to express a 0–100% quantity or change—not depth,
which belongs to Hierarchical rendering. Use outline/fill/slash/enclosed variants for state:
fill for selection or emphasis, slash for off or unavailable, and enclosed for small-size
legibility.

For custom glyphs:

- Draw in black and clear so the system can tint the glyph.
- Keep size, detail, stroke weight, and perspective consistent; match adjacent text weight.
- Optically center asymmetric shapes rather than trusting geometric centering.
- Ship vector SVG/PDF, provide accessible labels, mirror directional or textual glyphs in RTL,
  and do not depict Apple hardware.

On the web, choose one vector icon set, size it in `em`, and color it with `currentColor`.
Choose outline or filled as the default; do not mix styles or weights. SF Symbols, SymbolEffect,
and macOS document mechanics are platform-specific; consistent text-matched vectors transfer.

<a id="motion"></a>
## Motion

- Match direction to the triggering gesture; contradictory motion disorients.
- Keep motion brief and precise. Avoid custom animation on frequent standard interactions.
- Make animation interruptible; never force people to wait through it.
- Pair motion with text, haptics, audio, or another signal when meaning is essential.
- Map symbol motion to meaning: Bounce for confirmation; Pulse (opacity) or Breathe (opacity
  plus size) for ongoing activity; Rotate or Draw On/Off for progress; Replace or Magic
  Replace for state change.
- Target 30–60fps. Avoid large peripheral or full-viewport motion and sustained oscillation
  near 0.2Hz, which can cause vestibular discomfort.

On the web, honor `prefers-reduced-motion`, keep transitions near 200–300ms, animate
`transform` and `opacity`, preserve gesture direction, and never rely on motion alone:

```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: .001ms !important; transition-duration: .001ms !important; }
}
```

<a id="checks"></a>
## Pre-ship visual checks

- Use named type roles, a restrained weight ramp, and no clipping at accessibility sizes.
- Reference semantic color roles everywhere; remap dark mode and signal it without color alone.
- Use one interaction tint and keep it off noninteractive content.
- Reuse spacing and radius scales; keep nested corners concentric.
- Adapt layouts across extremes rather than stretching one composition.
- Keep Liquid Glass on functional chrome and preserve contrast over every background.
- Use one text-matched icon family and style.
- Keep motion brief, directional, optional, and interruptible.


