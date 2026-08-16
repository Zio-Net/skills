# Experience and accessibility

## Mission

Verify that the changed experience lets the intended user complete the task,
understand system state, recover from failure, and use the flow with applicable
accessibility, responsive, localization, and RTL needs.

## Select when / skip when

Select for user-visible behavior, UI, interaction, navigation, loading/error
states, content, localization, accessibility, or frontend/backend behavior that
changes what a user experiences. Skip for internal changes with no credible
user or developer experience effect.

## Evidence to inspect

- Task/design evidence, existing product patterns, and changed UI components.
- Data loading and mutation flow, backend responses, validation, and recovery.
- Loading, empty, success, error, offline, timeout, and permission states.
- Keyboard/focus order, semantic roles/names, screen-reader behavior, contrast,
  touch targets, responsive layouts, localization, and RTL.
- Component/integration tests, screenshots, or browser evidence when available.

## Review sequence

1. State the user's goal and trace the shortest successful path.
2. Enumerate meaningful states and transitions, especially waiting, empty,
   failure, retry, cancellation, and partial success.
3. Check whether system status and next actions are understandable without
   hidden knowledge.
4. Verify frontend behavior matches backend contracts and permissions.
5. Inspect keyboard, focus, semantics, announcements, visual affordances, and
   touch/responsive behavior.
6. Check text expansion, localization, date/number directionality, and RTL
   layout where applicable.
7. Prefer direct rendered/browser evidence for claims that static code cannot
   establish.

## Light / standard / deep

- `light`: inspect the changed state and primary path against established
  patterns.
- `standard`: cover all meaningful states, frontend/backend consistency,
  keyboard/focus, responsive behavior, and tests.
- `deep`: exercise the end-to-end flow across viewport, input, localization,
  accessibility, slow/failure, and permission variants.

## Finding threshold

Report when a user cannot complete or understand the task, receives misleading
state, cannot recover, or faces a concrete accessibility/responsive/RTL
barrier. Tie the finding to a user, state, and observable impact.

## Do not report

- Personal aesthetic preferences without a requirement or measurable UX
  impact.
- Broad redesign proposals unrelated to the changed flow.
- Pixel-level claims without rendered evidence.

## Challenge signals

Reopen purpose when the user goal differs from the stated feature. Reopen
architecture/contracts when the required experience cannot be represented by
the current API or state model. Move this lens before correctness when its
finding changes the interaction design.

Use the shared worker result contract from `SKILL.md`.
