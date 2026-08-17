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

Pass all three gates before selecting a lens:

1. **Changed surface:** the reviewed artifact causally changes behavior,
   contracts, data, trust, interaction, or runtime operations owned by this
   lens. Mentioning a subject is not changing its surface.
2. **Concrete scenario:** Scout can name a plausible failure introduced by the
   change and the evidence that would confirm or reject it.
3. **Unique value:** that scenario is not adequately covered by an earlier
   selected lens. Overlap without a distinct decision means `skipped`.

Examples of insufficient signals: a cost document discusses production, a
document has human readers, or a workflow mentions paths, hashes, permissions,
recovery, or configuration. Select production, experience, security, or
architecture only when the changed artifact itself creates the corresponding
runtime, interaction, trust, or contract risk.

Treat agent skills and other executable instructions by the actions they can
cause, not by their Markdown file type. Start with purpose, behavioral
correctness, and simplicity as candidates. Add architecture for actual
worker/state contracts; experience for a distinct human-interaction failure;
security for a concrete actor-input-asset path; and production readiness only
when the artifact changes deployed or externally operated behavior. The fact
that a skill has prompts, checkpoints, or pause/resume steps is not sufficient
by itself.

| Lens | Select when | Usually skip when |
|---|---|---|
| purpose-and-behavior | Feature behavior, domain rules, scope or intent may be wrong or unclear | Purely mechanical change with clear invariant behavior |
| architecture-data-and-contracts | Actual component/worker boundaries, ownership, persistent state, API/events/config, compatibility or deployment order change | Documentation only describes architecture, or local implementation leaves contracts unchanged |
| security-and-privacy | A changed trust boundary, auth, permissions, tenancy, secrets, sensitive data, untrusted input, or file/network operation has a concrete actor-to-impact path | Security-related words or defensive checks appear, but no credible attack or disclosure scenario is introduced |
| experience-and-accessibility | Changed user-visible flow or interaction has a distinct task-completion, recovery, localization, RTL, or accessibility failure | The artifact merely has human readers, or interaction concerns are already covered by purpose/correctness |
| correctness-and-tests | Executable logic or instructions, state transitions, parsing, calculations, commands, error paths, or regression risk changes | Prose/generated documentation has no verifiable behavioral, numerical, or command claim |
| production-readiness | Deployed or externally operated behavior changes async/distributed work, infrastructure, retries, concurrency, load, rollout, observability, or recovery | A document only informs production decisions, or a local agent workflow only persists its own review state |
| simplicity-and-code-health | Human-authored code or documentation can be made clearer without behavior change | Generated files, lockfile-only changes, or a scope too small to simplify meaningfully |

No substantive lens is mandatory. A skipped lens must have a reason tied to the
actual scope. Two to four selected lenses is typical. More than four requires a
different concrete failure scenario and evidence target for every additional
lens; breadth by itself is not a reason.

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

Using a dedicated Scout changes who profiles the scope, not how many lenses
should be selected. When included paths form unrelated change groups and no
task/PR evidence establishes one intent, return the groups and the unresolved
scope question without a lens agenda. Build the agenda only after the user
chooses the review boundary.

## Scout output

Return:

1. Exact scope boundary.
2. Known intent, assumptions, and open questions.
3. Change class, semantic size, blast radius, reversibility, and evidence
   quality.
4. When scope is resolved, an agenda table with `selected/skipped`, depth,
   reason, and order.
5. When scope is unresolved, coherent change groups and one scope question
   instead of an agenda.
6. Whether confirmation is required before the agenda or first lens.

Do not return findings, fixes, or a merge verdict.
