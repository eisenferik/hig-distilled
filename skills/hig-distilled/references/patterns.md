# UX Patterns

Apply these cross-cutting behavior rules to flows, states, dismissal, defaults, feedback, and
edge cases. Preserve the Apple-specific exceptions and transferable web rules. See
`references/inputs-and-interaction.md`, `references/components.md`, and
`references/accessibility.md` for related controls and accessibility.

## Contents

Core (high-frequency)
- [Modality](#modality)
- [Feedback](#feedback)
- [Loading & perceived performance](#loading--perceived-performance)
- [Launching](#launching)
- [Onboarding](#onboarding)
- [Searching](#searching)
- [Settings](#settings)
- [Entering data](#entering-data)
- [Accounts & sign-in](#accounts--sign-in)
- [Notifications & permissions](#notifications--permissions)
- [Undo & reversibility](#undo--reversibility)
- [Ratings & reviews](#ratings--reviews)
- [Offering help](#offering-help)
- [File management](#file-management)

Media & system (briefer)
- [Charting data](#charting-data)
- [Collaboration & sharing](#collaboration--sharing)
- [Drag & drop](#drag--drop)
- [Full screen](#full-screen)
- [Multitasking](#multitasking)
- [Audio, haptics, video](#audio-haptics-video)
- [Printing](#printing) · [Live viewing](#live-viewing) · [Workouts](#workouts)


---

## Modality

A mode that blocks the parent view and requires an explicit action to leave. Use it deliberately — it interrupts.

**Use a modal only for a clear payoff:** (1) surface critical info that must be seen or acted on; (2) confirm or modify the most recent action; (3) run a distinct, narrowly-scoped task without losing prior context; (4) an immersive/focus experience. If none apply, keep the work inline.

**Rules**
- Keep the task simple, short, self-contained — one linear path. A modal that hides its parent taxes memory; a multi-level "app inside an app" makes people lose the thread.
- Give an obvious, convention-matching exit. Touch (iOS/iPadOS/watchOS): a top-toolbar button *and* swipe-down. macOS/tvOS: a button in the content. On web, mirror both: a visible close control plus `Esc`, over a backdrop, with focus trapped inside.
- Guard user content: if dismissing could discard unsaved input, confirm first — whether the dismiss came from a gesture or a button.
- Title the modal (or add a line of description) so people keep their place.
- Don't stack modals; dismiss one before showing the next. The single exception is an alert, which may appear above everything — but never show two alerts at once.

Component vocabulary: alerts, sheets, popovers, action sheets/confirmation dialogs, activity views, separate windows, full-screen modals — see `references/components.md`.

## Feedback

Tell people what's happening, what they can do, and the result of what they did — and match delivery intensity to the information's stakes.

**Rules**
- Scale intensity to significance. Ambient/passive feedback for low-stakes status (people glance when they care); interruptive alerts only for high-stakes cases like possible data loss.
- Put status *in context*, near the thing it describes, so people get it without acting or navigating away (e.g. last-updated time and unread count sitting in the mailbox toolbar).
- Success is the default expectation — usually only *failure* needs surfacing. Reserve explicit success confirmations for consequential actions (a payment going through).
- When a command can't run, show it and say *why*, rather than silently doing nothing.
- Warn only for *unexpected + irreversible* loss. Don't confirm expected outcomes (no dialog for dragging a file to the trash).
- Deliver the same message through redundant channels — color **and** text (**and** sound/haptics) — so it lands when the device is silent, the user looks away, or uses VoiceOver. Never color-only.

**Cross-platform:** Haptics are Apple-hardware; the transferable rule is *redundant, multi-sensory feedback*. On glanceable/low-attention surfaces (watchOS, a background tab), prefer "we'll notify you when it's done" over an open-ended spinner that implies the user must keep watching.

## Loading & perceived performance

"The best loading experience finishes before people notice it." Hide latency rather than decorating it.

**Rules**
- Show *something* immediately. A blank screen reads as broken — render placeholder text/graphics/skeletons, then swap in real content progressively as it arrives.
- Keep the app usable while loading. Load in the background so people can read menus, browse, or start elsewhere (a game loads the next level while the player reads hints).
- Pick the indicator by what you know: **determinate** progress when duration is known, **indeterminate** when it isn't.
- For unavoidably long waits, give people something worthwhile — tips, hints, feature intros — sized so it neither flashes by nor loops.
- Prefetch large assets in the background (post-install, during updates) to speed first launch.

**Cross-platform:** The web analog is skeleton screens + background prefetch + progressive hydration. Match indicator use to the surface's attention budget: on watchOS, aim for instant content and avoid spinners, but a 1–2s indicator still beats a blank screen.

## Launching

The startup experience: from tap to first usable screen. Onboarding, if any, comes *after* this.

**Rules**
- Launch feels instant. The system shows a launch screen the instant the app opens and swaps it for the real first screen — the trick that sells "fast."
- Make the launch screen nearly identical to the first real screen so the swap doesn't flash. If the app opens on a solid color, the launch screen is just that color. No text (it can't localize). It must match the current orientation and light/dark mode.
- Restore state on relaunch — granularly: scroll position, selection, window location, in-progress input. Never make people retrace steps.
- The launch screen is not branding, an About box, or a splash screen. A splash screen (a branding graphic) is separate: show it at the *start of onboarding*, or right after launch if there's no onboarding.

**Cross-platform:** iOS/iPadOS/tvOS require launch screens; macOS/visionOS/watchOS don't. Web analog: a first-paint skeleton that matches final layout (respecting theme/orientation) plus restoring scroll/route/session state.

## Onboarding

Optional, skippable help getting started — never a gate.

**Rules**
- The ideal onboarding is none: people learn by using the app. When you need it, make it fast, optional, and skippable.
- Teach by doing — let people safely perform the action they're learning; retention beats reading.
- Prefer a set of *just-in-time contextual tips* near the relevant UI over one big upfront tour, so people focus on one action while making real progress.
- Ship sensible defaults so most people can start immediately; postpone nonessential setup and customization.
- Request a permission during onboarding *only* if the app can't function without that data — then explain why and the benefit. Otherwise ask in-context, when the feature is first used.
- If a separate tutorial exists, don't re-present it after a skip; keep it findable later (help/settings).
- Don't block first use on large downloads; bundle enough to start. Keep licensing/agreements out of onboarding. Don't ask for ratings or purchases before people have engaged.

## Searching

How people find content in-app, in a document, and system-wide. Primary control is a search field.

**Rules**
- If search matters, give it a primary spot — a dedicated tab or the main toolbar.
- Prefer one searchable location for the whole app; sections may additionally offer *local* search that filters the current view.
- Always make the active *scope* clear — via placeholder text, a scope bar, or a title ("searching this mailbox").
- Cut typing: show recent searches before input, and predictive completions/corrections while typing, personalized from past searches.
- Support scoping/filtering by attributes (date, type, size) via scope bars and tokens.
- Treat search history as private: think before showing it where others can see, and always offer a way to clear it.

**Cross-platform:** Spotlight/Quick Look/Core Spotlight are Apple-system-specific; system-wide indexing maps loosely to site-wide/global search. Everything else (prominent single entry, visible scope, recents + predictions, clearable history) transfers directly.

## Settings

Ways to customize the app. Split by *how often* an option changes and *where* it applies.

**Rules**
- Ship strong defaults suiting the largest audience, so most people never open settings. Auto-detect (connected controller, current appearance) instead of asking.
- Minimize the count — too many options bury the one people want and make the app feel unapproachable.
- Put rarely-changed, app-wide options in a dedicated settings area (key mappings, account options, window config). Opening it suspends the current task, so reserve it for infrequent changes.
- Keep *task-specific* options inline, on the screen they affect (show/hide a pane, sort, filter) — discoverable and in-context.
- Don't duplicate platform/OS/browser-level settings (accessibility, scrolling, auth) — duplicates confuse scope.
- Restore the last-viewed section when settings reopens.

**Apple-specific:** the system Settings app, the App menu's Settings item, `⌘,` to open (Esc in games), pane-based macOS settings windows. The transferable split — rarely-changed-global vs. task-specific-inline — is the real rule.

## Entering data

Reduce effort and mistakes: pre-gather what you can and support every relevant input method.

**Rules**
- Pull data from the system automatically (settings, or permissioned location/calendar) instead of asking.
- Prefill reasonable defaults to cut decisions.
- Prefer selection over typing — pickers/menus/lists are faster and less error-prone than a keyboard, even when a keyboard is available.
- Accept paste and drag-and-drop as first-class entry methods.
- Validate *inline, as people type*, with immediate feedback — fix errors in the moment, not after a long submit. Use number formatters for numeric fields.
- Make requirements obvious: keep Next/Continue disabled until required fields are filled.
- Mask secrets: obscure each character in a secure field. Never prepopulate a password field — always require re-entry or biometric/keychain auth.

**Cross-platform:** Secure-digit entry (tvOS) and AirPlay auto-blur of secure fields (visionOS) are Apple-specific; macOS expansion tooltips reveal truncated text on hover (pointer-only). The core rules — prefill, live inline validation, prefer selection, accept paste/drag, mask secrets — are universal.

## Accounts & sign-in

**Rules**
- Require an account only if core functionality needs it; otherwise let people use the app account-free.
- Defer sign-in as long as possible — let people feel the value first (browse the shop, sign in only at checkout). Forced early sign-in drives abandonment.
- Prefer Sign in with Apple; else passkeys (username only, no password to create); if passwords remain, add two-factor.
- Name the auth button after the actual method ("Sign In with Face ID"), and only reference methods available on the current device — check capabilities first.
- Don't add an in-app biometric toggle (biometrics are a system-level setting; an app toggle is redundant). Avoid the word "passcode" for account auth — people tie it to unlocking the device.
- **Account deletion** is mandatory if you allow account creation: let people *delete* (not just deactivate), in-app or via an easy-to-find link, equally simple on web and in-app. Say when deletion completes and confirm when done. Clarify that an auto-renewable subscription keeps billing through the store until canceled, independent of account deletion.

**Cross-platform:** Sign in with Apple, passkeys, iCloud Keychain, Face/Touch ID are Apple mechanisms. Transferable: defer auth, minimize required accounts, name the method precisely, offer *genuine* deletion. Platform specifics — tvOS: minimize typing on the remote, prefer sign-in from another device; watchOS: autofill via iCloud Keychain; TV-provider apps route sign-out to system Settings.

## Notifications & permissions

Send notifications people trust; map urgency honestly.

**Rules**
- Get explicit permission before sending anything. The system lets people silence or change it later.
- Respect Focus and delivery scheduling — people choose who breaks through and whether alerts arrive immediately or batched into a summary. Even a delayed *alert* still delivers the notification itself on arrival.
- Assign the honest interruption level:

  | Level | Overrides scheduled delivery | Breaks Focus | Overrides Ring/Silent |
  |---|:---:|:---:|:---:|
  | Passive (view at leisure) | No | No | No |
  | Active (default) | No | No | No |
  | Time Sensitive | Yes | Yes | No |
  | Critical (health/safety, rare) | Yes | Yes | Yes |

  Critical needs a special entitlement. Use Time Sensitive only for something happening now or within the hour — the system periodically re-prompts people to reassess whether your use is justified.
- Separate transactional from marketing. Gate marketing behind explicit opt-in with an in-app control to change it. Marketing must never use Time Sensitive.

**Cross-platform:** Focus, interruption levels, Ring/Silent, SiriKit are Apple mechanisms. Transferable: permission first, urgency maps to real importance, respect quiet/do-not-disturb states, and separate + opt-in-gate marketing. watchOS surfaces per-notification controls (Mute 1 Hour, Turn off Time Sensitive) via swipe.

## Undo & reversibility

Let people recover from mistakes and explore safely.

**Rules**
- Make outcomes predictable, then *show* them. People often undo repeatedly until they see a change — so if the affected content is offscreen, scroll to reveal it, or they'll assume nothing happened and keep going.
- Label the target: "Undo Typing", "Redo Bold" — never a bare "Undo".
- Allow deep, multi-step undo back to a logical checkpoint (open or last save); don't cap it arbitrarily. Consider a batch "undo all changes since opening."
- Use the invocation methods people already know; add dedicated buttons only when truly necessary, and use the standard undo/redo symbols if you do.

**Cross-platform:** Shake-to-undo and three-finger-swipe are iOS idioms (absent on tvOS/watchOS); `⌘Z` / `⇧⌘Z` and the Edit menu are Mac conventions. Never redefine these standard gestures. The transferable principle: reliable, multi-step, predictable reverse-and-restore with clear labels and visible confirmation of *what* changed.

## Ratings & reviews

Ask only after the person has experienced value.

**Rules**
- Trigger off engagement signals — launch count, features explored, tasks/levels completed — never on first launch or during onboarding (no opinion formed yet; you invite negative reviews).
- Ask at a natural break, never mid-task or mid-gameplay.
- Prefer the system prompt: it's consistent, single-tap, checks for prior feedback, lets people opt out globally, and self-limits to **3 displays per app per 365 days**. Space your own requests ≥1–2 weeks apart, re-prompting only after further engagement.

**Cross-platform:** the store prompt and summary-rating reset are Apple-store-specific; the behavior — request feedback only post-value, at a non-disruptive moment, strict frequency caps, easy dismissal — applies to any review/NPS/feedback prompt.

## Offering help

Contextual, in-the-moment guidance — tied to what the person is doing now and easy to dismiss.

**Rules**
- Match help to task complexity: 1–2 step task → a brief inline view; complex multistep → a tutorial.
- **Tips** suit simple, easy-to-describe features (if it needs more than three actions, it's too complex for a tip). Keep them 1–2 action-oriented sentences, no promo content. Use eligibility rules so tips only reach people who'd benefit (don't tip a feature already used), and cap frequency (e.g. once per 24h) across multiple tips. Types: popover, inline (annotation-style pointing at an element, or hint-style). Prefer the *filled* symbol variant.
- **Tooltips / help tags** (macOS, visionOS): ~60–75 chars, sentence case, no ending punctuation, start with a verb ("Restore default settings"). Describe *only* the indicated control and what it does *in your app* — don't explain how standard components work, and don't repeat the control's name.
- Use platform-appropriate language and imagery: don't say "click" on iPhone or "tap" on Mac; don't show a game controller to a Siri Remote user. Prefer animation/graphics over long text for novel controls.

**Cross-platform:** TipKit, help tags, and hover/gaze triggers are Apple-specific; tooltips are inherently pointer/gaze (less relevant to pure touch). Transferable: just-in-time, short, dismissible, precisely targeted, audience-gated frequency.

## File management

For document-based apps: create, open, save, preview, sync.

**Rules**
- Provide familiar affordances: New/Open commands and always an Add (+) button. Custom browsers should respect the platform's file model (Finder/Files) — open to the most relevant location but let people navigate everywhere.
- **Autosave by default** — save periodically while editing and on close/app-switch, so people trust their work is preserved. Only force explicit saves if the user has turned autosave off; then show unsaved state and prompt on close.
- Signal unsaved state clearly (macOS: a dot on the close button and next to the doc name) — but *only when autosave is off*; a dot with autosave on is misleading. New docs default to "Untitled".
- Hide file extensions by default, let people reveal them, and reflect that choice consistently across all open/save UI.
- Support **Quick Look**: preview files the app can't open, in place, without leaving. In file-provider extension UIs, filter to context-appropriate types (only PDFs in a PDF editor), give a clear destination chooser for export/move, and don't add a second toolbar (the modal already has one).

**Cross-platform:** watchOS/tvOS have no file-browsing UI. Finder/Files/Quick Look/document launcher are Apple frameworks. Transferable: autosave silently + signal unsaved clearly, reuse the familiar file model, preview in place, filter pickers to relevant types, offer a clear save/export destination.

---

## Charting data

Present data so people spot trends, current state, and comparisons.

- Match chart *size* to function: large enough to read labels and support interaction; small/glanceable for a single value or a preview that expands into a fuller view.
- Support progressive disclosure — let people opt into more detail rather than showing everything at once. Teach novel chart types by animating them in.
- Keep charts that share purpose or data consistent (type, colors, marks, annotations); deviate only to signal a real difference. Prefer common types (bar, line). Don't use a chart when a scrollable/sortable list or table serves better.
- Always add accessibility labels describing values *and* interactive accessibility elements — a visible summary headline does not replace them. See `references/accessibility.md`. This guidance is platform-agnostic; it transfers to web directly.

## Collaboration & sharing

- One obvious, persistent share entry point (a toolbar Share button). Show a Collaboration button next to it the moment collaboration begins; it identifies who's sharing.
- Start sharing from the system share sheet, which handles method + permissions. Support "send copy" vs. collaborate.
- Keep permission choices minimal and grouped; write short summaries ("Only invited people can edit") that expand into a few options.
- Collaboration events (edits, membership changes, @mentions) can post notifications carrying a deep link back to the right view.

**Cross-platform:** share sheet, CloudKit/iCloud, Messages/FaceTime, SharePlay are Apple-specific; not on tvOS. Transferable: single obvious share point, a persistent "shared / who's here" affordance, concise grouped permissions, deep links in collaboration notifications.

## Drag & drop

People try it everywhere — support it broadly, and always offer an alternative (menu copy/move, accessibility drag).

- **Drag image** appears once movement exceeds ~3 pt; render it translucent so the destination stays visible beneath.
- **Destination feedback:** insertion point or view highlight when droppable; a "not allowed" cue (`circle.slash`) when not. Show cues only while hovering the target.
- **Move vs. copy:** same container = move; different container = copy; across apps = always copy. Hold Option to force copy (checked at drop time).
- **Spring loading:** hovering a control while dragging activates it (e.g. switching a Calendar view). **Auto-scroll** a scrolling destination while dragging over it.
- Offer multiple representations highest→lowest fidelity (PDF → PNG → JPEG); the destination picks the richest it supports. On failure, the item animates back or evaporates. After drop: keep dropped content selected; deselect at the source.
- Prefer undo; confirm irreversible drops. Show progress + a placeholder for slow/large transfers.

**Cross-platform:** not on tvOS/watchOS. visionOS pinch-and-hold, Universal Control, force-click spring loading are Apple-specific. The threshold, translucent image, droppable/not-allowed feedback, move-vs-copy + Option-to-copy, multi-fidelity payloads, undo, and post-drop selection all map to web DnD.

## Full screen

Expand to fill the screen and hide chrome for distraction-free focus (games, media, deep work).

- Adjust layout for the larger canvas by subtly shifting proportions — same items, no jarring rearrange. Don't programmatically resize the window.
- Let people choose when to *enter and exit*; don't auto-exit on app switch or when an activity ends. Pause games/slideshows on leave; resume where they left off on return.
- Keep essential controls reachable — reveal hidden toolbars via tap, swipe-down, or moving the cursor to the top edge.

**Cross-platform:** not on tvOS/visionOS/watchOS (already immersive). Maps directly to the web Fullscreen API: user-controlled enter/exit, reveal-on-gesture chrome, resume state.

## Multitasking

Nearly every app must support it, and you never know when it starts.

- Always be ready to save and restore context. Pause attention-demanding activity (games, media) on switch-away; resume seamlessly on return.
- **Audio focus:** pause indefinitely for primary audio (music, podcasts); briefly duck or pause for short interruptions (GPS prompts), then restore. Finish user-initiated background work (downloads, exports) before suspending. Notify only on important completions.
- Adapt to *any* window size — apps get no signal about the chosen split/window config, so design responsively and don't assume a fixed size.

**Cross-platform:** not on watchOS. PiP, iPadOS/visionOS window management, and visionOS gaze-to-activate are Apple-specific — notably, *don't* pause video on look-away in visionOS/macOS multi-window. Transferable: save/restore anytime, pause/resume on focus change, respect audio focus, finish background work, be responsive.

## Audio, haptics, video

**Audio** — behave as people expect around silence, volume, and routing.
- Pick the session category matching real use — don't stop another app's music if you don't need to. (Solo-ambient silences others; ambient mixes; playback ignores the silent switch and plays in background; record/play-and-record for capture and calls.)
- Never override overall system/user volume — adjust only relative internal levels. Reroute transparently on headphone connect; pause immediately on disconnect. Respond to media-key/Control-Center controls only when actively playing or in an audio context; never repurpose standard controls. Resume intelligently after interruptions (a media app checks whether the interruption was resumable; a game can just resume). Never rely on sound alone.

**Haptics** (Apple-hardware: Taptic Engine, Force Touch, Digital Crown, Pencil Pro).
- Use system patterns per their documented meaning — a "failure" pattern must never signal success. Build a consistent, causal haptic↔action mapping so people learn it. Match intensity/sharpness to the animation and sync with sound. Prefer short haptics for discrete events; keep them optional (mutable) and the app fully usable without them. Standard families: Notification (Success/Warning/Error), Impact (Light/Medium/Heavy/Rigid/Soft), Selection. Web transfer is limited (Vibration API); the principles — reinforce cause→effect, stay consistent and short, complement not replace, allow off — still carry.

**Video** — prefer the system player and its familiar controls; a custom player should mirror them.
- Default mode by aspect ratio: aspect-fill (may crop) for wide 2:1–2.40:1; aspect-fit (letterbox/pillarbox) for standard and ultrawide. Always display at the true aspect ratio — never bake letterbox padding into the frame (it breaks scaling and PiP).
- Show a loading screen (black, centered spinner, no surrounding content) only if loading exceeds ~2s. Start playback as soon as enough loads; keep loading the rest in the background. Support expected input (Space to play/pause). Don't mix audio across mode switches (PiP mute vs. background music).

**Cross-platform:** silent switch, session categories, Spatial Audio, PiP, Siri Remote, and the watchOS encoding specs are Apple-specific. The transferable video rules: native controls (or faithful mimicry), preserve true aspect ratio, minimal fast-appearing loading state, don't mix audio across modes.

## Printing

- Put print in the predictable spot (macOS File menu; iOS/iPadOS toolbar → action sheet). Show it only when printing is possible — dim/remove it when there's nothing to print or no printer.
- Use the system print UI for standard options (page range, copies, double-sided); don't reimplement what the system provides. Add app-specific options only where they add value, and hide advanced ones behind an "Advanced Options" disclosure. Make option interdependencies clear.
- **Cross-platform:** not on tvOS/watchOS. Web analog: lean on the browser print dialog, hide it when unavailable, tuck advanced options behind disclosure.

## Live viewing

Apps centered on live streams (vs. on-demand): prioritize the fastest possible path to playback.

- Minimize launch→playback: live content in the first tab; ideally one tap (a "Watch Now" button that vanishes into full-screen playback) or zero. Show progress of in-progress live content so people know where they'll land. Give instant visual feedback on channel change (covers stream load).
- Mark live content unmistakably (badge/sash, a "Live" row). Keep playback the primary action; record/restart/favorite are secondary in a consistent app-wide order. A browse-while-watching footer should invoke/dismiss symmetrically (swipe up to show, down to hide). Concepts transfer to any streaming web/TV app.

## Workouts

Fitness experiences, chiefly watchOS (Apple-hardware: wrist-raise persistence, Activity rings, HealthKit sensors).

- Glanceable, high-contrast metrics for people in motion; put the most important data (time, calories, distance) where it's easiest to read. Use a distinct visual appearance to signal an active session.
- Always-available, easy-to-tap pause/resume/stop with clear start/stop feedback. Explain missing sensor data (water blocks heart rate while swimming) and still record what you can. End with a summary confirming completion and showing recorded data. Discard extremely brief sessions automatically or ask.
- Use Activity rings only for their documented meaning and colors. Transferable: glanceable in-motion metrics, always-available session controls with clear feedback, explain unavailable data, completion summary.


