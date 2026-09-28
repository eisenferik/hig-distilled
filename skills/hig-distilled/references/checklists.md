# Checklists

Use the **design review** to evaluate an existing UI and the **pre-ship** pass before completion. Treat each item as an inspection prompt, trace findings to a principle or rule, and rank them by **user impact**, not checklist order.

## How to run a review

1. State the screen or flow's **purpose** in one sentence; inability to do so is the first finding. (*Purpose*)
2. Review each section and assign severity: **blocker** (unusable, inaccessible, or risks data loss) → **major** (confusing, breaks a principle, or harms a real task) → **minor** (polish or inconsistency).
3. Record what is wrong, the governing principle/rule, and a concrete fix.
4. Lead the summary with the top 3 fixes, then report the rest.

## Design-review checklist

### Purpose & content
- [ ] The primary task of this view is obvious within a second or two.
- [ ] Every element earns its place; nothing competes with the primary task.
- [ ] Content (not chrome) gets the most space and attention.
- [ ] Empty, loading, and error states are all designed — not just the happy path.

### Hierarchy & layout
- [ ] Visual hierarchy matches importance (size/weight/color/position/spacing lead the eye correctly).
- [ ] Spacing is consistent and on a rhythm; related things are grouped, unrelated things separated.
- [ ] Alignment is consistent; elements sit on a shared grid.
- [ ] Content stays clear of edges, notches, and system UI (safe margins).
- [ ] Layout reflows/restructures for different sizes rather than just stretching. → `references/apple-visual-system.md`

### Navigation
- [ ] Each screen answers: where am I / how did I get here / how do I go back / how do I reach the primary task.
- [ ] The navigation model fits the content shape (flat / hierarchical / content-driven).
- [ ] Back always works and returns to prior state (scroll position, selection).
- [ ] No primary destination is hidden behind an unnecessary menu. → `references/navigation.md`

### Components & interaction
- [ ] At most one primary action per view; action hierarchy is clear.
- [ ] The right control for the job (toggle vs checkbox vs radio vs segmented vs dropdown).
- [ ] Interactive elements look interactive and have adequate size/spacing.
- [ ] All states exist and are distinct: default, hover/focus, pressed, selected, disabled, loading, error.
- [ ] Modality is justified (focused, self-contained) and dismissal is obvious. → `references/components.md`, `references/patterns.md`

### Feedback & flow
- [ ] Every action gets an immediate, proportionate response.
- [ ] Loading shows honest progress (skeleton/optimistic/determinate) — never a blank screen or endless spinner.
- [ ] Destructive actions are reversible (undo) or clearly confirmed with a specific verb.
- [ ] Errors say what happened and how to fix it, with the recovery action right there. → `references/patterns.md`

### Typography & color
- [ ] A small, consistent type ramp; clear text hierarchy; comfortable line length and height.
- [ ] Color is semantic/role-based and adapts to light/dark; one restrained accent.
- [ ] Meaning is never carried by color alone.
- [ ] Contrast meets WCAG AA (4.5:1 body, 3:1 large/UI). → `references/apple-visual-system.md`

### Apple look (visual craft)
- [ ] System font stack; a small role-based type ramp; body ~17px; no hairline weights.
- [ ] Spacing reuses a consistent set of values; generous whitespace; line length capped.
- [ ] Restrained palette with one accent tint applied only to interactive elements.
- [ ] Color referenced by semantic role; dark mode is a re-mapping (not inversion); elevation via lighter surfaces.
- [ ] Continuous, concentric corners (inner radius = outer − padding); sibling radii match.
- [ ] Signature effects (blur/glass/shadow) only on the floating chrome layer, sparingly.
- [ ] One icon set, one weight, tracking the adjacent text; not mixed outline/filled.
- [ ] Reuses `assets/apple-tokens.css` values wherever the kit has one, rather than ad-hoc hex/px. → `references/apple-visual-system.md`

### Writing
- [ ] Buttons and labels use the verb of the outcome; terminology is consistent.
- [ ] Copy is concise, human, and specific; no jargon, raw error codes, or vague "something went wrong". → `references/writing.md`

### Accessibility (floor — must pass)
- [ ] Contrast AA; color independence.
- [ ] Touch targets ~44px; keyboard-operable with a visible focus indicator; logical focus order.
- [ ] Text scales/reflows; every control has an accessible name + role; images have alt or are decorative.
- [ ] Respects reduced-motion; no info conveyed by motion alone. → `references/accessibility.md`

### Craft & consistency
- [ ] Consistent iconography, spacing tokens, radii, aligned corners, and patterns across the app.
- [ ] Motion is purposeful and quick; transitions preserve context.
- [ ] It feels considered — details are deliberate, not accidental. (*Craft*, *Delight*)

## Pre-ship checklist

Verify **real conditions**, not only the design mockup:

- [ ] **Content extremes:** very long text, empty, huge numbers, missing images, long names — nothing clips, overlaps, or breaks layout.
- [ ] **Localization:** translated text still fits without clipping; right-to-left mirrors correctly; dates/numbers/units format per locale.
- [ ] **Sizes & inputs:** smallest and largest target sizes; touch, pointer, and keyboard all work; orientation/resize handled.
- [ ] **Themes:** light and dark both checked for contrast and legibility (not a naive inversion).
- [ ] **Accessibility pass:** keyboard-only walkthrough; screen-reader smoke test; 200% text/zoom; contrast tooling; reduced-motion on.
- [ ] **States & failures:** offline, slow network, permission denied, server error — each has a designed, recoverable state.
- [ ] **Reversibility:** destructive actions can be undone or are clearly confirmed; no accidental data loss.
- [ ] **Consistency:** matches the rest of the product's patterns, spacing, and terminology.
- [ ] **Performance feel:** interactions respond immediately; perceived performance is handled (optimistic/skeleton) where real work is slow.

## Reporting template

```
## Design review: <screen/flow>
Purpose: <one sentence>

Top fixes:
1. [blocker/major] <finding> — <principle/rule> → <fix>
2. ...

Other findings:
- [minor] <finding> → <fix>

What's working well:
- <keep these>
```


