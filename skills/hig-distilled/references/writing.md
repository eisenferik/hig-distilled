# UX writing & microcopy

Treat interface copy as part of the design: every word is a control the reader operates. A label defines an affordance; an error defines a recovery path. Write concise, human, specific copy. Clear beats clever. Read uncertain lines aloud; rewrite anything stiff or hard to say.

## Contents

- [Core stance](#core-stance)
- [Voice & tone](#voice--tone)
- [Labels & buttons](#labels--buttons)
- [Flow & sequence words](#flow--sequence-words)
- [Titles & headings](#titles--headings)
- [Alerts & dialogs](#alerts--dialogs)
- [Forms & fields](#forms--fields)
- [Error messages](#error-messages)
- [Empty states](#empty-states)
- [Settings & controls copy](#settings--controls-copy)
- [Capitalization](#capitalization)
- [Terminology](#terminology)
- [Per-device & delivery channel](#per-device--delivery-channel)
- [Numbers, dates, units & i18n](#numbers-dates-units--i18n)
- [Accessibility of copy](#accessibility-of-copy)

## Core stance

- Write for a person. Address the reader directly and describe outcomes in their terms, not the implementation's.
- Prefer concrete language: "Nothing saved yet," not "No data available."
- Cut words that do not change meaning; short copy scans and translates better.
- Put action before rationale and use active voice: "Choose a plan," not "A plan should be chosen."
- When wit and clarity conflict, choose clarity. Reserve delight (see `references/principles.md`) for moments when nobody is stuck.

## Voice & tone

Keep voice constant; adapt tone to the moment.

- Choose a voice suited to the audience and intended feeling: a bank may be steady and precise; a game playful and energetic.
- Maintain a shared term list so voice stays consistent across screens, releases, and authors.
- Be serious and direct in an error, light in a win, and calm in a routine confirmation.
- Avoid jargon, internal names, and gendered terms.
- Keep personality to low-stakes copy such as empty states, confirmations, and onboarding—not moments involving money, data, or security.

Payment failure: not "Oops! Something's not quite right! 😅" but "Your card was declined. Check the details or try another card."

## Labels & buttons

Use the verb of the outcome. A reader should predict the result without surrounding text.

| Situation | Weak | Strong |
|---|---|---|
| Confirm a purchase | OK | Buy Now |
| Remove several items | Submit | Delete 3 Files |
| Finish a form | Done | Create Account |
| Dismiss without saving | Cancel | Discard Changes |
| Start a primary flow | Let's do it! | Send |

- Match the label to the effect: if a button charges a card, say "Pay," not "Continue."
- Use vague words such as OK, Submit, and Yes only when the outcome is genuinely generic.
- Keep labels scannable, usually one to three words.
- Prefer a plain verb to a cheerful phrase that hides the action.
- Include a count or object when it reduces risk: "Delete 3 Files."
- Use possessive pronouns only when they disambiguate ownership; otherwise prefer "Favorites" to "Your Favorites." Stay consistent.
- Do not pair two easily confused verbs. Give the action and escape distinct consequences.

For "Delete account?", use `[ Delete account ] [ Keep account ]`, not `[ Yes ] [ No ]`.

## Flow & sequence words

Use one stable vocabulary throughout a multi-step flow.

- Reserve one starting verb, such as "Get started," for the entry point.
- Pick "Continue" or "Next" for forward movement; do not alternate them.
- Use a clear terminal word, such as "Done," to signal completion.
- Do not rename the same action between screens.

Prefer "Get started" → "Continue" → "Continue" → "Done" over "Begin" → "Continue" → "Next" → "Finish."

## Titles & headings

- Name the content or place: "Billing history," not "Details."
- Match the destination title to the navigation label that led there.
- Make headings describe the section beneath them for readers who scan headings only.

Prefer nav "Billing" → title "Billing" over nav "Money" → title "Financial Overview Dashboard" (see `references/navigation.md`).

## Alerts & dialogs

An alert must justify the interruption and resolve quickly.

| Part | Job |
|---|---|
| Title | State the situation or exact question |
| Body | Explain the consequence in one sentence; omit it if the title is enough |
| Buttons | Answer the title with verbs; make the safe action the easy default |

- Use inline status or a notification for passing information; reserve a modal alert for a blocking choice that requires an answer.
- Put enough meaning in the title for someone who reads only it to choose correctly. Pair "Discard draft?" with "Discard" and "Keep editing," not "OK" and "Cancel."
- Make the safe, reversible choice the default for focus and Enter. Require deliberate effort for destruction.
- Name destructive actions explicitly ("Delete," "Remove") and never hide irreversible consequences behind a routine-looking button.

Example: title "Delete this project?"; body "Its 12 tasks will be permanently removed. This can't be undone."; buttons "Delete project" and "Cancel"—not "Warning," "Are you sure?", and "OK / Cancel."

## Forms & fields

- Give every field a persistent visible label. A placeholder disappears on focus and does not reliably name the field for screen readers.
- Use placeholder or hint text for format examples—"name@example.com," "MM / YY," "Your name"—not as a label replacement.
- Put validation beside the field it concerns, not only at the top of the form.
- State requirements before failure, such as "At least 8 characters."

Prefer a visible "Email" label, "name@example.com" placeholder, and inline hint to an unlabeled box and an "Invalid" banner at the top.

## Error messages

Answer what happened, why when useful, and how to fix it; then offer recovery.

- Prevent errors where possible with constraints, sensible defaults, and confirmation of destructive actions. If wording cannot fix a widespread error, redesign the interaction.
- Use plain language beside the problem so cause and location arrive together.
- State facts without blame: "That email is already registered," not "You entered an invalid email."
- Give positive instructions: "Use only letters for your name," not "Don't use numbers or symbols."
- Do not use ambiguous system-centered phrasing such as "We're having trouble loading this" when "Unable to load content" is clearer.
- Never make a raw code or exception the explanation. It may appear as a small secondary support reference.
- Match tone to the stakes; a failed payment is not a place for personality.
- Avoid dead ends: always provide a recovery action when one exists.

| Weak | Strong |
|---|---|
| Error 0x8004 | We couldn't reach the server. Check your connection and try again. |
| Invalid name | Use only letters for your name. |
| That password is too short | Choose a password with at least 8 characters. |
| Something went wrong | Your file is too large. Choose one under 25 MB. |

Prefer "That password doesn't match. Try again or reset it" to "Login failed": it names the cause without blame and provides two recovery paths.

## Empty states

Treat an empty state as a first impression, not a blank.

- Explain what belongs there so "empty" reads as new rather than broken.
- Include the action that creates the first item, usually as a button.
- Keep it warm and brief; light personality is appropriate, but essential information is not, because the state disappears once content exists.

Prefer "No invoices yet. When you bill a client, it'll show up here" plus "Create invoice" to "No results."

## Settings & controls copy

Settings are often read individually and out of context.

- Use a clear label and add one explanatory line only when needed.
- Describe the on state and let the reader infer off: "Show read receipts," not "Read receipts on/off."
- Link directly to a setting instead of describing a route through menus: "Open Notification settings."

Prefer "Allow notifications" with "Get alerts when someone replies" to "Notifications" with "Controls whether notifications are enabled or disabled."

## Capitalization

- Sentence case ("Save changes") is the safe default for most UI text and is easier to read, maintain, and translate. Title case ("Save Changes") feels more formal. Apple's rule for **button titles is title-style capitalization**; use it there.
- Choose one convention per element type—alerts, buttons, and so on—and apply it consistently, while honoring component-specific conventions (see Craft in `references/principles.md`).
- Avoid ALL CAPS beyond a short label; it is harder to read, and screen readers may spell it letter by letter.

## Terminology

- Use one term per concept. Do not alternate among "workspace," "project," "board," and "space."
- Prefer users' words to engineers' words: "Trash," not "Soft-delete queue."
- Keep vocabulary stable across UI, docs, errors, tooltips, and empty states. Rename a concept everywhere at once.

Use "Archive" consistently in the list, menu, and confirmation—not "Archive," "Move to storage," and "Deactivate" for one action.

## Per-device & delivery channel

- Match the verb to input: "tap" for touch, "click" for a pointer, and "select" when one phrase must cover both.
- Shorten copy as the screen shrinks and lead with the essential word. Keep private details off shared, room-visible TV screens.
- Match channel to urgency: choose the lightest notification, inline banner, modal alert, or action sheet that still lands (see `references/patterns.md`).

Prefer "Tap Continue" to "Tap or click the button below to continue with your request."

## Numbers, dates, units & i18n

- Localize numbers, dates, currency, and units. "07/08" names different dates in different locales.
- Do not concatenate sentence fragments. Use one complete localized string with named placeholders so translators can change word order and grammar.
- Use locale-aware plural rules, not an appended "(s)"; many languages have more than two plural forms.
- Allow for text expansion. Translations often run substantially longer than English (see `references/apple-visual-system.md`).

Replace `"You have " + n + " item" + (n === 1 ? "" : "s")` with a localized template such as `"You have {count} items"` resolved through the platform's plural rules.

## Accessibility of copy

Copy must stand on its own for assistive technology (see `references/accessibility.md`).

- Make link and button text meaningful out of context and front-load meaning. A screen reader's link list turns "click here" and "read more" into meaningless entries.
- Describe an image's purpose in alt text, not its pixels. State an informative chart's takeaway, mark decoration empty, and describe an icon control's action.
- Give every control a clear accessible name that reads naturally aloud; do not rely on visual position to complete its meaning.
- Avoid ALL CAPS and creative punctuation in labels; assistive technology may read them character by character.

Prefer "Download invoice (PDF, 120 KB)" to "Click here": it remains clear in a link list and communicates file type and size.
