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

Call a change forward-only only when migration or rollback evidence proves it;
persisted data or a large migration is not enough by itself.

## Selection signals

Probe every lens, then apply three gates:

1. **Changed surface:** the reviewed artifact causally changes behavior,
   contracts, data, trust, interaction, or runtime operations owned by this
   lens. Mentioning a subject is not changing its surface.
2. **Observed signal:** evidence already inspected shows a suspicious behavior,
   contradiction, uncovered material path, or important gap that the bounded
   probe cannot resolve. Merely imagining a plausible failure is not enough.
3. **Unique value:** the lens asks a distinct review question. Inspecting the
   same files in another lens does not make the question redundant, and the
   answer could change the verdict or implementation direction.

Classify the result as:

- `skipped` when a gate fails;
- `light` / `checked` when the highest-risk direct path was inspected and no
  concrete warning or material unresolved gap remains;
- `standard` or `deep` only when the probe records the warning or gap, why the
  light check could not decide it, and which review decision remains open.

A `checked` reason names the direct path inspected and why no separate review is
needed. A selected reason names the evidence that prevented the same closure.
Generic curiosity, PR breadth, or a list of topics still worth examining does
not justify promotion.

`light` / `checked` requires affirmative closure evidence, not merely the
absence of an observed warning. The coordinator may close it directly only
when the relevant surface is small and fully inspected during the initial
analysis. Otherwise run a fresh bounded closure probe or promote the lens to
`standard`. Metadata and another author's summary are context, not sufficient
closure evidence by themselves.

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
actual scope. Do not skip simplicity merely because architecture inspects the
same code: substantial human-authored changes normally receive at least a light
simplicity probe. Two to four dedicated lenses is typical; checked light probes
do not count. Breadth by itself is not a reason to add a dedicated lens.

## Depth

- `light`: inspect the diff, task, applicable repository rules, and the
  highest-risk direct path. It may cover a small, clear lens surface inside a
  large PR and does not require a dedicated worker.
- `standard`: also inspect adjacent callers/consumers, contracts, tests, and
  relevant failure paths. This is the normal default.
- `deep`: trace the behavior end to end, inspect history/docs/runtime evidence,
  and test competing assumptions. Use for ambiguous, broad, hard-to-reverse, or
  high-impact changes.

Choose depth from the risk and evidence specific to that lens. PR size,
cross-service scope, a migration, or a public contract is not by itself a reason
to make every related lens deep. A `deep` reason must name the ambiguity,
high-impact failure, difficult reversal, or runtime evidence that requires the
extra work. Depth applies to the lens's deciding question, not every listed
subtopic; inspect tests only as far as needed to judge the risky behavior. Do
not ask a generic "how detailed?" question; render the recommendation and let
the user override it.

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

## Optional evidence delegation

The coordinator performs the initial analysis. When a candidate `checked` lens
is not small enough to inspect fully there, a fresh worker performs a bounded
closure probe and reports the paths inspected plus `clear` or `promote`. The
coordinator still classifies lenses and owns the user-facing result.

When scope is unresolved, return coherent change groups and one scope question
instead.

After the table, state whether confirmation is required.
Do not return findings, fixes, or a merge verdict.
