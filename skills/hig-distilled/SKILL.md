---
name: hig-distilled
description: >-
  Apply Apple's Human Interface Guidelines to design, build, or review web and
  cross-platform interfaces, covering visual craft, interaction quality, states,
  accessibility, and Apple-style implementation guidance. Use only when the user
  explicitly names `hig-distilled`, asks to use Apple's HIG or Human Interface Guidelines,
  or requests an Apple-, iOS-, or macOS-style interface.
---

# Apple-style interface design and build

Apply Apple's visual craft and interaction quality to web and cross-platform UI without
cloning Apple pixel-for-pixel. Treat the **look** (type, semantic color, spacing, radii,
materials, icons, motion) and the **feel** (states, feedback, modality, agency, undo-first
actions, adaptivity) as equally important. Use concrete values grounded in Apple's HIG,
not generic “clean UI” defaults; treat web implementations as faithful approximations.

This file routes the work and defines done. The concrete values and exceptions live only
in `references/` and `assets/`.

## Invariants

The rules most often missed. They are not sufficient on their own.

- Reference color by semantic role. Dark mode remaps roles; it never inverts colors.
- Nested corners stay concentric: **inner radius = outer radius − padding**.
- Keep Liquid Glass on floating chrome above scrolling content, sparingly and legibly.
- Target ~44×44px controls. Meet WCAG AA contrast. Never use color as the only signal.
- Prefer Undo. Confirm only when loss is irreversible and unexpected.
- Use at most one primary action per view. Label actions with the outcome verb.
- Start web work from `assets/apple-tokens.css`. Copy the needed tokens into the
  deliverable rather than importing a file outside its root.
- When principles conflict, **Purpose** breaks the tie.

## Reference routing

Before designing or writing code, read the sections that match the task. Pick sections from each file's Contents and skip unrelated ones. A screen
usually spans several files—for example, a form reads `patterns.md` → Entering data,
`writing.md` → Forms & fields and Error messages, and `accessibility.md` → Forms. Every
build or restyle also reads `references/apple-visual-system.md` in full and
`references/accessibility.md` → Minimum bar to ship. If your
tool truncates long output, read the section by line range.

| Read | When |
|---|---|
| `assets/apple-tokens.css` | Building web UI; start from its tokens and control recipes |
| `references/apple-visual-system.md` | Applying type, color, spacing, radii, materials, icons, or motion |
| `references/principles.md` | Resolving design trade-offs or reviewing intent |
| `references/components.md` | Choosing or implementing controls, containers, bars, and search-field anatomy |
| `references/patterns.md` | Handling modality, feedback, states, onboarding, search behavior, forms, accounts, sharing, or media |
| `references/navigation.md` | Choosing navigation architecture, adaptive bars/sidebars, search as navigation, paths/tokens, or state restoration |
| `references/inputs-and-interaction.md` | Supporting keyboard, pointer, touch, focus, selection, or gestures |
| `references/writing.md` | Writing labels, buttons, alerts, errors, terminology, localized strings, or per-device copy |
| `references/accessibility.md` | Accessibility, inclusive language, RTL, privacy/permissions, media alternatives, or WCAG/ARIA review |
| `references/checklists.md` | Running a design review or pre-ship check |
| `references/sources.md` | Verifying current or platform-specific HIG details |

## Working style

Inspect the existing product before changing it. Preserve its purpose, identity,
architecture, and conventions unless the user asks otherwise or they conflict with
applicable HIG guidance.

Decide unspecified details yourself. Ask only when a missing decision changes product
purpose, data model, scope, or other high-impact behavior.

## Done

Build: writing code alone is not done. If a preview is available, check the conditions
the change affects—light and dark, narrow and wide widths, relevant states (empty,
loading, error, disabled, pressed), keyboard focus, hit targets, text scaling—and fix
obvious defects.

Review: rank findings by user impact, tie each to a rule or principle with a concrete
fix, and lead with the top fixes (`references/checklists.md` → Reporting template).

## Current HIG and scope

For exact metrics, new APIs, or current platform-specific behavior, use the official DocC JSON
endpoint documented in `references/sources.md` rather than relying on this snapshot.

Treat the token kit as a faithful web approximation. SF Pro and SF Symbols are not freely
embeddable on the web, and Apple exposes many values through platform APIs rather than fixed
web values. This paraphrased distillation is MIT-licensed and does not replace the
official Human Interface Guidelines.
