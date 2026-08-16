# Purpose and behavior

## Mission

Determine whether the change solves the right problem for the intended user or
system and whether its observable behavior matches the strongest available
intent evidence. Challenge the feature before reviewing implementation details
that depend on it.

## Select when / skip when

Select for new behavior, changed domain rules, ambiguous bug fixes, scope
questions, or conflicts between task text and code. Skip for a genuinely
mechanical change whose behavior and invariant are already clear.

## Evidence to inspect

- User request, issue, PR body, acceptance criteria, design record, and product
  documentation.
- Existing behavior, domain models, user-facing flows, tests, and relevant git
  history.
- Changed code only after reconstructing the intended outcome.

Rank explicit current requirements above stale documentation. Separate
`known`, `assumed`, and `open` facts instead of silently inventing intent.

## Review sequence

1. State the problem, beneficiary, expected outcome, and deliberate non-goals in
   plain language.
2. Identify the source for every material domain rule or acceptance condition.
3. Trace the primary success path and the most important failure/edge outcomes
   at a behavioral level.
4. Compare the actual diff with those outcomes. Look for missing scenarios,
   extra scope, changed semantics, and behavior that solves a different
   problem.
5. Check whether tests encode the intended behavior rather than merely the
   implementation.
6. Mark intent gaps that prevent a reliable conclusion.

## Light / standard / deep

- `light`: confirm the stated task and primary observable outcome against the
  diff and direct tests.
- `standard`: also inspect domain rules, adjacent flows, failure outcomes, and
  existing behavior.
- `deep`: reconcile conflicting sources, inspect history and end-to-end flows,
  and test competing interpretations of the requirement.

## Finding threshold

Report only a concrete mismatch such as wrong user outcome, missing essential
scenario, unsupported domain rule, accidental scope expansion, or a test suite
that locks in behavior contrary to the requirement. Explain whose outcome
breaks and how.

## Do not report

- Implementation style, abstractions, naming, or local performance.
- Speculation about hypothetical future product directions.
- A missing requirement as a defect when it is only an open question; record an
  evidence gap instead.

## Challenge signals

Reopen Scout or an earlier decision when the observed behavior contradicts the
task, when two authoritative sources disagree, or when the feature's primary
outcome cannot be established. A load-bearing purpose finding should stop
downstream lenses.

Use the shared worker result contract from `SKILL.md`.
