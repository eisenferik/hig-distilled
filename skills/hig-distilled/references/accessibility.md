# Accessibility, inclusion, and responsible design

Use this reference to make interfaces intuitive, perceivable without relying on one sense, adaptable to user settings, inclusive across abilities, languages, cultures, and reading directions, and responsible with personal data. It maps concrete rules across Apple accessibility settings, semantic HTML/ARIA, and WCAG. Accessibility operationalizes **Flexibility** and **Responsibility**; apply it throughout design, not as a late audit. See `references/principles.md` and `references/apple-visual-system.md`.

## Contents

- [Minimum bar to ship](#minimum-bar-to-ship)
- [Apple accessibility settings, mapped](#apple-accessibility-settings-mapped)
- [Contrast](#contrast)
- [Color independence](#color-independence)
- [Text scaling and reflow](#text-scaling-and-reflow)
- [Target size and spacing](#target-size-and-spacing)
- [Keyboard, focus, and alternate input](#keyboard-focus-and-alternate-input)
- [Semantics and VoiceOver](#semantics-and-voiceover)
- [Images and media](#images-and-media)
- [Forms](#forms)
- [Motion and animation](#motion-and-animation)
- [Time, interruptions, and cognitive load](#time-interruptions-and-cognitive-load)
- [Screen-reader and testing basics](#screen-reader-and-testing-basics)
- [Inclusive language and representation](#inclusive-language-and-representation)
- [Right-to-left and mirroring](#right-to-left-and-mirroring)
- [Privacy and permissions](#privacy-and-permissions)
- [HIG concept to WCAG mapping](#hig-concept-to-wcag-mapping)

---

## Minimum bar to ship

A screen that fails any item below is not ready, regardless of visual polish.

- **Contrast:** body text ≥ 4.5:1 against its actual background; large text and meaningful UI, icons, and graphics ≥ 3:1. Check light and dark appearances.
- **Not color alone:** repeat every color-coded state or meaning with text, icon, shape, or pattern.
- **Text scales:** preserve content at larger font sizes and ~200% zoom without clipping, overlap, or loss.
- **Touch targets:** make targets ≥ ~44×44px and separate them enough to prevent mis-taps.
- **Keyboard-operable:** make every control reachable and operable, show focus, and prevent traps.
- **Name + role + value:** expose each control's accessible name, correct role, and current state.
- **Focus order:** keep order logical and manage focus when modals, routes, or overlays open and close.
- **Reduced settings honored:** respect reduced motion and transparency; never flash more than three times per second.

## Apple accessibility settings, mapped

Prefer semantic system colors and text styles that adapt automatically over hardcoded special cases.

| Apple setting | User need | Transferable rule | Web / ARIA equivalent |
|---|---|---|---|
| **VoiceOver** | Read and navigate non-visually | Give every element an accessible name, role, and value | Semantic HTML + ARIA; test with NVDA/JAWS/VoiceOver |
| **Dynamic Type / Larger Text** | Scale text comfortably | Use relative units and growing containers | `rem`/`em`, `clamp()`, no fixed heights; browser zoom |
| **Bold Text** | Heavier strokes | Do not encode meaning in hairline weights | Adequate weight; test heavier rendering |
| **Increase Contrast** | Stronger foreground/background separation | Provide a higher-contrast path when marginal | `prefers-contrast: more` |
| **Reduce Transparency** | Opaque instead of translucent surfaces | Never depend on glass alone behind text | Solid fallback under `prefers-reduced-transparency` |
| **Reduce Motion** | Less travel, parallax, and movement | Preserve state change; remove animation | `prefers-reduced-motion: reduce` → fades/instant |
| **Differentiate Without Color** | Non-color cues | Pair hue with text, icon, shape, or pattern | Labels, icons, underlines |
| **Reduce / Dim Flashing Lights** | Avoid seizure/discomfort triggers | Stay under 3 flashes/sec; avoid strobes | Cap rate; no full-screen strobe |
| **Full Keyboard Access** | Keyboard operation | Full operability, visible focus, no traps | Native focusables, `:focus-visible`, roving tabindex |
| **Switch Control** | One- or few-switch operation | Linear order over labeled controls | Correct DOM/tab order |
| **Voice Control** | Activate controls by spoken name | Match visible labels and accessible names | Visible text ↔ `aria-label` |
| **Assistive Access** | A simplified, one-thing-per-screen path | Support simplification; confirm risky actions | Progressive disclosure; explicit confirmation |

Apple's pt values below are device-tuned; the underlying rules transfer across platforms.

## Contrast

**Rule:** Meet WCAG AA: 4.5:1 for normal text and 3:1 for large text (roughly ≥ 24px, or ≥ 19px bold) and meaningful non-text elements such as icons, boundaries, focus rings, and chart strokes. Aim for AAA—7:1 body and 4.5:1 large—when content is dense, small, or read for long periods. Apple specifies 4.5:1 through 17pt and 3:1 at 18pt or for bold text. The 4.5:1 ratio approximates accommodation for 20/40 vision.

- Test computed rendered colors, not token names. For images, gradients, video, or translucency, test the worst pixel and add a scrim or solid backing as needed. Make that backing opaque under **Reduce Transparency**.
- Test light and dark appearances. Disabled controls are exempt, but do not style required readable content as disabled.
- Use semantic system colors so **Increase Contrast** can adapt them; support `prefers-contrast: more` and verify final CSS.

**WCAG:** 1.4.3 (text), 1.4.6 (AAA), 1.4.11 (non-text).

## Color independence

**Rule:** Pair color-coded meaning, state, or grouping with text, icon, shape, pattern, or position. Roughly 1 in 12 men has a color-vision deficiency; grayscale, monochrome contexts, and cultural associations also break color-only signals. Red-green and blue-orange pairings are especially risky.

- Combine an error color with an icon and message, not just a border.
- Label required fields, status dots, chart series, and map legends; add glyphs or textures and let users customize palettes in data-heavy views.
- Give links in body text a non-color cue such as underline or weight.
- Desaturate the screen and confirm every distinction remains.

**WCAG:** 1.4.1. **Apple:** Differentiate Without Color.

## Text scaling and reflow

**Rule:** Respect chosen text size. Scale to at least 200% (140% on watchOS), reflow down to a narrow viewport without horizontal reading scroll, and prevent clipping, overlap, and meaning-destroying truncation.

**Apple custom-type defaults / minimums:** iOS/iPadOS 17 / 11 pt · macOS 13 / 10 pt · tvOS 29 / 23 pt · visionOS 17 / 12 pt · watchOS 16 / 12 pt. Increase thin custom weights beyond these; heavier weights remain more legible when small.

- Use relative/scalable units (`rem`/`em`, Dynamic Type styles), including support for **Bold Text**. Do not use tiny fixed type; 10px body text cannot be made accessible as a visual style.
- Let containers grow: use minimum rather than fixed heights, wrap instead of clip, and never ellipsize essential labels.
- Preserve reading order after reflow; stack a two-column layout rather than hiding its second column.
- Test the largest **Larger Text** setting and browser zoom.

**WCAG:** 1.4.4 (Resize Text), 1.4.10 (Reflow).

## Target size and spacing

**Rule:** Target about 44×44px density-independent minimum for touch, pointer, and stylus, with enough separation to prevent accidental activation.

**Apple defaults / minimums:** iOS/iPadOS 44 / 28 pt · macOS 28 / 20 pt · tvOS 66 / 56 pt · visionOS 60 / 28 pt · watchOS 44 / 28 pt. Allow roughly 12 pt around bezeled controls and ~24 pt around visible edges of unbezeled controls.

- Expand the hit area beyond a small glyph; a 16px icon can have a 44px padded target.
- Give destructive or hard-to-reverse controls extra size and separation.
- Treat 44px as the design target and WCAG's 24px as the absolute floor.

**WCAG:** 2.5.5 (44px AAA), 2.5.8 (24px AA). **Apple HIG:** ~44pt standard minimum.

## Keyboard, focus, and alternate input

**Rule:** Make every pointer or touch action keyboard-operable in logical order, with visible focus and no traps. The same accessibility tree supports **Full Keyboard Access**, **Switch Control**, screen readers, and **Voice Control**; hover- or drag-only controls exclude them.

- Match focus order to visual and reading order. Fix DOM/view order instead of using tabindex tricks.
- Provide “skip to content” past repeated navigation.
- Keep a visible focus ring; restyle rather than remove it.
- On dialog open, move focus inside and intentionally contain it; on close, return it to the trigger. On route change, move it to the new view's heading.
- Match visible labels to accessible names so a spoken command such as “tap Send” activates the intended control.
- Use native focusables, `:focus-visible`, and roving tabindex for composite widgets. Do not override system shortcuts.

**WCAG:** 2.1.1, 2.1.2, 2.4.3, 2.4.7, 2.4.11.

## Semantics and VoiceOver

**Rule:** Encode structure and controls semantically. Use one `h1` per view, logical heading order, landmarks, and an accessible name, role, and value for every control. Prefer native elements to ARIA.

- Use native `<button>`, `<a>`, `<input>`, and checkbox controls for built-in role, keyboard behavior, and state. ARIA fills gaps; incorrect ARIA is worse than none.
- Do not substitute a clickable `div` or `span` for a native control; styling alone provides neither semantics nor keyboard behavior.
- Use heading levels for hierarchy, not font size; style instead of skipping levels. Mark header, nav, main, and footer regions.
- Name icon-only controls (“Close”, “Search”).
- Programmatically expose expanded/collapsed, selected, checked, and busy states so they are announced.
- On Apple platforms, set `accessibilityLabel`, `accessibilityTraits`, and `accessibilityValue`, and deliberately group/order VoiceOver elements.

**WCAG:** 1.3.1, 4.1.2, 2.4.6.

## Images and media

**Rule:** Give meaningful images contextual text alternatives, mark decoration empty, provide media alternatives, and do not autoplay sound.

**Four alternatives:**

- **Captions:** synchronized text for all significant audio, including dialogue and sound; support deaf/hard-of-hearing people and sound-off use.
- **Subtitles:** transcribed or translated dialogue, typically for language.
- **Audio descriptions:** narration of key visuals in natural pauses.
- **Transcripts:** complete, searchable, skimmable text for long-form content.

- Describe an image's purpose, not its pixels. State a chart's takeaway or link to its data.
- Use empty `alt=""` or a decorative flag for decoration.
- Pair audio cues with haptics and visual cues, especially for off-screen action.
- If autoplay is unavoidable, mute it and provide an obvious pause/stop control.
- On Apple platforms, label meaningful images, exclude decorative views from accessibility, and pair cues with the Haptic Engine.

**WCAG:** 1.1.1, 1.2.1–1.2.3, 1.2.2, 1.4.2. **Web:** `alt`, `<track>`.

## Forms

**Rule:** Associate every field with a persistent programmatic label. State required/optional status in text, announce and associate errors, explain recovery, and show instructions before submission.

- Use placeholders only for format hints such as “name@example.com”; they disappear, often fail contrast, and are not reliable labels.
- Mark required fields with text or an announced indicator, not an unlabeled asterisk. State format requirements up front.
- On error, move or associate focus to the field, give a specific instruction such as “Enter a date in the future,” and announce it through a live region.
- Preserve entered data; never clear the form after failure.
- Group radios, address blocks, and related fields under a shared programmatic label.

**WCAG:** 3.3.1, 3.3.2, 3.3.3, 1.3.1, 4.1.2. **Web:** `<label for>`, `aria-describedby`, `aria-live`. On Apple, bind labels to controls and post accessibility notifications on validation. See `references/writing.md`.

## Motion and animation

**Rule:** Honor reduced motion, never encode information in motion alone, and stay below three flashes per second.

- Under **Reduce Motion**, preserve the state change while replacing large transitions with instant changes or cross-fades. Tighten springs, track gestures directly, avoid z-axis/depth motion, replace x/y/z travel with fades, and do not animate into or out of blur.
- Keep essential animation short and skippable.
- Pair animated state changes with persistent visual or announced feedback.
- Honor **Dim Flashing Lights** and avoid rapid red flashes and full-screen strobes.

**WCAG:** 2.3.1, 2.3.3. **Web:** `prefers-reduced-motion`. See `references/apple-visual-system.md` for motion tokens.

## Time, interruptions, and cognitive load

**Rule:** Make time limits adjustable, extendable, or removable; warn before expiry. Do not move focus or content unexpectedly. Prefer explicit dismissal to automatic dismissal.

- Warn before a session or form timeout and offer a simple extension, except for genuine security or real-time constraints.
- Let people pause, stop, or hide auto-updating content; offer pacing or difficulty accommodations when relevant.
- Avoid auto-dismissing elements, auto-advancing carousels, focus-stealing toasts, and reflow while someone reads or types.
- In an **Assistive Access**-style streamlined mode—one interaction per screen and core functionality only—confirm hard-to-undo actions twice.

**WCAG:** 2.2.1, 2.2.2, 3.2.1, 3.2.2.

## Screen-reader and testing basics

Run these before shipping; automated inspection alone is insufficient.

- **Screen reader:** complete the primary task with VoiceOver, NVDA/JAWS, or TalkBack. Confirm sensible name, role, and state; skimmable headings; and no unreachable or “button, button” control.
- **Keyboard only:** reach and operate everything, keep focus visible, complete the flow, and escape every overlay.
- **Zoom / large text:** use the largest **Larger Text** setting and 200% zoom; check clipping, overlap, and horizontal reading scroll.
- **Contrast / automation:** test final rendered colors in light and dark, manually including images, gradients, and glass. Use axe or Apple's **Accessibility Inspector** for quick findings, but treat automation as a floor and test with real assistive technology.

## Inclusive language and representation

Inoffensive is not automatically inclusive; choose language and imagery deliberately.

- Address people as “you/your”; reserve “we/our” for the company. Prefer plain language and define technical terms before use.
- Rewrite unnecessary gendered pronouns. Collect gender only when genuinely required, such as for health or legal needs; then offer nonbinary, self-identify, and decline-to-state options.
- Represent varied races, body types, ages, and abilities. Use non-gendered imagery for generic people; SF Symbols provides nongendered glyphs.
- Use people-first language and ask how a community self-identifies.
- Avoid hard-to-localize idioms, colloquialisms, and humor, including terms with exclusionary origins such as “grandfathered in” and “peanut gallery.”
- Avoid stereotyped roles, narrow definitions of family, and security questions that assume shared experiences such as a first car or college.
- Treat disability as a spectrum that includes temporary and situational impairment, such as a broken arm, bright sun, or a noisy room.

These rules transfer across platforms. Internationalize before localizing; map directional/localized SF Symbols guidance to equivalent icon sets. See `references/writing.md`.

## Right-to-left and mirroring

Use system layout direction—or web `dir="rtl"` and CSS logical properties—for Arabic, Hebrew, and other RTL scripts.

**Mirror:**

- Layout, navigation, reading flow, and text alignment: back points right, next/previous flip, and left alignment becomes right alignment.
- Sliders, progress bars, and their begin/end glyphs.
- Icons that imply text or forward/backward motion.
- Positions of images in meaningful chronological or ranked sequences.

**Do not mirror:**

- Digits within phone numbers, credit cards, or values. Reverse a counting sequence's item order when its control flips, never the numerals themselves.
- Controls that represent a physical direction or actual on-screen location.
- Logos, checkmarks and other universal marks, or arbitrary photos/artwork. Remake direction-critical artwork rather than flipping it when meaning or copyright may change.

**Details:**

- Align a paragraph of 3+ lines according to its own language; keep one alignment across a mixed-script list.
- Arabic and Hebrew have no uppercase and can look small beside all-caps Latin; increase RTL type by ~2 pt when needed.
- RTL locales may use different numeral systems. Choose per locale for number-centric apps; otherwise follow system defaults.

The judgments transfer across frameworks even when implementation differs.

## Privacy and permissions

Request only necessary data and explain it transparently.

**Timing and rationale:**

- Request the smallest, most specific data scope the feature needs, in context when that feature is used. A launch-time request is acceptable only when the resource is essential and the reason obvious, such as location in a maps app.
- Write a brief, complete, active-voice purpose string with a concrete reason: “The app records at night to detect snoring.” Avoid “Microphone access is needed for a better experience” and bare imperatives such as “Turn on microphone access.”
- A custom pre-permission screen may add context, but it must contain exactly one button opening the system prompt, titled “Continue” or “Next”—never “Allow”—with no Cancel or dismiss. Do not imitate, incentivize, coerce, or fake system consent.

**Protecting data:** process on-device where possible; prefer passkeys and platform sign-in to custom authentication and plain-text secrets; add biometrics and two-factor authentication where appropriate.

Apply the same timing, specificity, and anti-coercion rules to web geolocation, camera, notifications, cookie consent, and similar prompts. Map Keychain and platform auth to WebAuthn/passkeys and secure credential storage.

## HIG concept to WCAG mapping

| HIG / design concept | Apple setting | WCAG success criteria | Web mechanism |
|---|---|---|---|
| Legible text and icons over any background | Increase Contrast, Reduce Transparency | 1.4.3, 1.4.6, 1.4.11 | Semantic colors; contrast checkers; `prefers-contrast` |
| Do not rely on color alone | Differentiate Without Color | 1.4.1 | Text + icon/shape; desaturate test |
| Respect font size | Dynamic Type, Larger Text, Bold Text | 1.4.4, 1.4.10 | `rem`/`em`; responsive layout |
| Comfortable targets (~44px) | HIG target sizing | 2.5.5, 2.5.8 | Padded hit areas |
| Full operation and visible focus | Full Keyboard Access, Switch Control, Voice Control | 2.1.1, 2.1.2, 2.4.3, 2.4.7, 2.4.11 | Native focusables; `:focus-visible` |
| Name, role, value | VoiceOver | 4.1.2, 1.3.1, 2.4.6 | Semantic HTML + ARIA |
| Alt text and decorative marking | VoiceOver | 1.1.1 | `alt` / empty alt |
| Captions, transcripts, no autoplay sound | — | 1.2.1–1.2.3, 1.4.2 | `<track>`; muted default + controls |
| Labeled forms and announced errors | VoiceOver | 3.3.1, 3.3.2, 3.3.3 | `<label for>`, `aria-describedby`, `aria-live` |
| Reduced motion and safe flashing | Reduce Motion, Dim Flashing Lights | 2.3.1, 2.3.3 | `prefers-reduced-motion` |
| Adjustable time; stable focus | Assistive Access | 2.2.1, 2.2.2, 3.2.1, 3.2.2 | Timeout warnings; no auto-advance/focus theft |


