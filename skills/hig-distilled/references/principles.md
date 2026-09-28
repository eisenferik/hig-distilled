# The eight design principles

Use these principles to explain a UI decision, diagnose what feels wrong, and resolve tension
between concrete rules. They describe the **why** behind the sibling visual, navigation,
component, and pattern references. Treat them as competing pressures, not independent boxes to
maximize. When they genuinely conflict, **Purpose** breaks the tie.

## Contents

- [Purpose](#purpose)
- [Agency](#agency)
- [Responsibility](#responsibility)
- [Familiarity](#familiarity)
- [Flexibility](#flexibility)
- [Simplicity](#simplicity)
- [Craft](#craft)
- [Delight](#delight)
- [Resolving conflicts](#conflicts)

<a id="purpose"></a>
## Purpose

Help someone accomplish one primary thing. Name that task, then make it the most prominent and
least obstructed path. Treat anything that does not serve it as a candidate for removal.

- State the view's primary task in one sentence before designing. Split a view that cannot be
  explained this way.
- Rank elements by how directly they serve the task; give the top-ranked element the strongest
  position, size, and contrast.
- Prefer removing an unjustified feature to burying it. Make every surviving feature earn its
  place against the primary task.

Avoid screens that showcase everything and prioritize nothing, “quick” actions that take many
steps, and secondary controls or promotions that compete with the core job.

<a id="agency"></a>
## Agency

Let people choose their path, understand the current state, and recover from mistakes. Control
and reversibility beat rigid hand-holding.

- Make consequential actions reversible with Undo, trash, or drafts instead of relying on
  confirmation dialogs people habitually dismiss.
- Show state, progress, and action results immediately so people never have to guess.
- Offer multiple inexpensive routes to important tasks, including relevant keyboard, pointer,
  touch, menu, and shortcut paths.

Avoid forced linear flows for order-independent work, irreversible actions protected only by
“Are you sure?”, and auto-advancing interfaces that outrun the person.

<a id="responsibility"></a>
## Responsibility

Act in the person's genuine interest. Be honest, request only what is needed, and never trade
trust for a short-term metric.

- Request data and permissions when needed, explain why plainly, and degrade gracefully when
  access is denied.
- Give decline, cancel, and unsubscribe choices equal visual and interaction weight to choices
  that benefit the business.
- Use defaults you could defend to the person directly; make consequential defaults explicit
  rather than preselected.

Avoid confirmshaming, a dominant “Accept all” beside a hidden alternative, easy signup with a
buried exit, and contextless permission requests.

<a id="familiarity"></a>
## Familiarity

Build on conventions people already understand so skills transfer between apps. Maintain
consistency within the product as well as with the platform.

- Use standard, platform-idiomatic controls for standard jobs.
- Keep a control's meaning, label, and placement stable; do not reuse an icon for two meanings.
- When deviation creates real value, make the new pattern self-explanatory on first use and
  account for its learning cost.

Avoid reinvented date pickers, checkboxes, scrollbars, or navigation with worse ergonomics;
unlabeled invented icons; and actions that move unpredictably between screens.

<a id="flexibility"></a>
## Flexibility

Adapt to different screen sizes, input methods, languages, content, and abilities. Design for
the range rather than an imagined median user.

- Reflow across widths and orientations; test the smallest and largest realistic layouts.
- Support touch, pointer, keyboard, voice, and assistive technology where the platform allows.
  Do not make a capability exclusive to one input method.
- Let content expand for translation, large text, long values, and variable item counts.

Avoid fixed-height rows that clip enlarged text, hover-only affordances, keyboard traps,
unreachable controls, and layouts that fail when labels are translated.

<a id="simplicity"></a>
## Simplicity

Make the interface clear and direct by removing the unnecessary and establishing an obvious
hierarchy. Do not confuse simplicity with sparse minimalism: it concerns ease of understanding,
not the fewest controls or pixels.

- Remove steps, options, and words that do not change the outcome.
- Establish one clear visual hierarchy per screen.
- Keep necessary complexity and reveal it progressively—defaults first, depth on demand—rather
  than deleting capabilities people rely on.

Avoid stripping useful controls for appearance, flattening affordances until nothing looks
interactive, equal-weight walls of content, and jargon where plain language works.

<a id="craft"></a>
## Craft

Treat spacing, alignment, states, copy, and timing as evidence of care and trustworthiness.
Polish is not decoration; it is deliberate execution of the whole experience.

- Use a consistent grid and spacing scale. Keep nested rounded shapes concentric and radii
  harmonious; see `apple-visual-system.md`.
- Design every state: happy, empty, loading, error, partial, offline, and long-content.
- Treat labels, error messages, animation curves, and micro-timing as first-class design work.

Avoid off-grid spacing, mismatched radii, unstyled non-happy states, janky or overlong motion,
and placeholder copy in production.

<a id="delight"></a>
## Delight

Evoke the right feeling at the right moment with a defining beat that fits the task. Delight is
earned at meaningful moments, not applied everywhere as decoration.

- Reserve character for transitions such as first success, completion, or a milestone, where
  it reinforces rather than interrupts.
- Match tone to stakes; payments and medical results are not occasions for confetti.
- Keep delightful touches fast, skippable, occasional, and compatible with reduced motion.

Avoid animation on every interaction, celebration during serious or routine work, and effects
that block the next action or ignore accessibility settings.

<a id="conflicts"></a>
## Resolving conflicts

Good design negotiates these pressures for the specific context. Do not maximize one principle
in isolation.

### Simplicity vs. Flexibility: hide, do not remove

Layer necessary capability: let defaults serve the common case and reveal depth through an
advanced section, overflow menu, or settings pane. Removing a needed option merely relocates
complexity into workarounds. Cut only what no real workflow depends on.

### Delight vs. Simplicity: do not obstruct the task

Place delight beside the path, such as after completion, rather than repeatedly in front of the
primary action. Keep it fast, occasional, skippable, and reduced-motion aware. Remove it when it
makes the core job slower or less clear.

### Familiarity vs. novelty: innovate on substance

Spend novelty where it creates value—in capability, content, or workflow—while leaving solved
controls conventional. Craft the familiar control well instead of replacing it. When breaking a
convention is necessary, make the pattern teach itself on first use.

### Agency vs. Responsibility: freedom inside protective guardrails

Prefer Undo, trash, and drafts so people retain control while mistakes remain cheap. Reserve
hard blocks and friction for genuinely irreversible or dangerous actions such as permanent
deletion, sending money, or sharing private data. Guardrails should catch harm without
micromanaging ordinary choices.

### Purpose vs. everything: use the tie-breaker

When other tensions do not decide the issue, choose the option that best serves this view's
primary task for this person. A principle that degrades the core job in the name of consistency,
cleanliness, novelty, or charm is being misapplied.

