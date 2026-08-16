# Review Scout lens catalog

Use this file only during Scout. Do not load full lens procedures until their
turn begins.

Scout is mandatory, including under deadline or authority pressure. Users may
override the proposed lens selection, depth, and order, but may not skip Scout.

## Change profile

Classify the scope using observable evidence:

- Change class: feature, bug fix, refactor, dependency, configuration,
  schema/data migration, DevOps/infrastructure, UI, documentation, tests, or
  generated output.
- Semantic size: local, cross-component, cross-service, or system-wide.
- Blast radius: internal-only, public contract, persisted data, user-visible,
  security/privacy, or production-control surface.
- Reversibility: trivial rollback, coordinated rollback, forward-only
  migration, or destructive/unknown.
- Evidence quality: clear task and tests, partial intent, conflicting sources,
  or missing specification.

## Selection signals

| Lens | Select when | Usually skip when |
|---|---|---|
| purpose-and-behavior | Feature behavior, domain rules, scope or intent may be wrong or unclear | Purely mechanical change with clear invariant behavior |
| architecture-data-and-contracts | Boundaries, ownership, persistent state, API/events/config, compatibility or deployment order change | Strictly local implementation with unchanged contracts |
| security-and-privacy | Trust boundary, auth, permissions, tenancy, secrets, sensitive data, input/abuse surface changes | No credible security or privacy surface changed |
| experience-and-accessibility | User-visible flow, UI states, interaction, localization, RTL or accessibility changes | Backend/internal change with no user or developer experience effect |
| correctness-and-tests | Executable logic, state transitions, error paths or regression risk changes | Documentation-only or generated-only scope |
| production-readiness | Async/distributed work, infrastructure, retries, concurrency, load, rollout or recovery matters | Local deterministic code with negligible operational impact |
| simplicity-and-code-health | Human-authored code or documentation can be made clearer without behavior change | Generated files, lockfile-only changes, or a scope too small to simplify meaningfully |

No substantive lens is mandatory. A skipped lens must have a reason tied to the
actual scope.

## Depth

- `light`: inspect the diff, task, applicable repository rules, and the
  highest-risk direct path. Use for small, obvious, reversible changes.
- `standard`: also inspect adjacent callers/consumers, contracts, tests, and
  relevant failure paths. This is the normal default.
- `deep`: trace the behavior end to end, inspect history/docs/runtime evidence,
  and test competing assumptions. Use for ambiguous, broad, hard-to-reverse, or
  high-impact changes.

Increase depth for public contracts, persisted data, security/privacy,
distributed failure semantics, difficult rollback, unclear intent, weak tests,
or cross-service changes. Do not ask a generic "how detailed?" question; render
the recommendation and let the user override it.

## Ordering

Add ordering edges based on invalidation:

1. Put purpose before downstream lenses when intent or domain behavior can
   invalidate the implementation.
2. Put architecture before correctness/production when boundaries, data, or
   contracts shape those checks.
3. Move security or experience earlier when either can force a redesign.
4. For application logic, correctness normally precedes production readiness.
5. For infrastructure, HA, delivery semantics, and rollout changes, production
   readiness may precede correctness.
6. Put simplicity last unless it is the only meaningful lens.

Examples:

- Cross-service feature: purpose -> architecture -> design-shaping security or
  experience -> correctness -> production -> simplicity.
- HA/DevOps change: architecture -> production -> security -> correctness ->
  simplicity.
- Small visual adjustment: experience -> correctness -> simplicity.

## Dedicated Scout threshold

Use a dedicated Scout worker when any of these applies:

- more than one change class or runtime surface;
- cross-component/service boundary;
- public contract, migration, security/privacy, or high-availability impact;
- missing or conflicting intent evidence;
- large diff whose meaningful paths are not obvious.

Otherwise the coordinator performs Scout directly.

## Scout output

Return:

1. Exact scope boundary.
2. Known intent, assumptions, and open questions.
3. Change class, semantic size, blast radius, reversibility, and evidence
   quality.
4. Agenda table with `selected/skipped`, depth, reason, and order.
5. Whether confirmation is required before the first lens.

Do not return findings, fixes, or a merge verdict.
