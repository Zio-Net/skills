# Architecture, data, and contracts

## Mission

Verify that responsibilities, dependencies, state ownership, and external or
internal contracts remain coherent across the complete changed path.

## Select when / skip when

Select when the change crosses components/services, moves ownership, persists
data, changes API/events/configuration, adds a migration, or alters deployment
order or compatibility. Skip for strictly local implementation whose
boundaries and contracts are unchanged.

## Evidence to inspect

- Applicable architecture instructions and decision records.
- Callers, consumers, dependency direction, module/service boundaries, and
  runtime topology.
- Domain/data models, source-of-truth rules, read/write paths, retention, and
  migrations.
- API schemas, event payloads, configuration, versioning, compatibility tests,
  and rollout order.
- Target-ref versions of these files when reviewing another branch.

## Review sequence

1. Map the changed responsibility from entry point through its consumers and
   durable state.
2. Identify the owner and source of truth for each important decision or datum.
3. Check dependency direction and whether the new coupling belongs at that
   boundary.
4. Trace create/read/update/delete and failure lifecycle for changed data.
5. Compare producer and consumer contracts, including versions, defaults,
   optionality, and error semantics.
6. Test backward/forward compatibility and deployment or migration ordering.
7. Compare the implementation with required architecture documentation.

## Light / standard / deep

- `light`: inspect changed boundaries, direct callers/consumers, and applicable
  repo architecture rules.
- `standard`: trace adjacent contracts, state lifecycle, compatibility, tests,
  and deployment order.
- `deep`: follow the end-to-end topology across services and versions, inspect
  migrations/history, and model mixed-version rollout and rollback.

## Finding threshold

Report a concrete boundary violation, split or ambiguous ownership, broken
producer/consumer contract, unsafe compatibility assumption, data lifecycle
gap, or impossible deployment/migration order. Show the affected path and
impact.

## Do not report

- New abstractions merely for aesthetic layering or possible future reuse.
- Broad architecture cleanup outside the changed path.
- Local correctness or formatting issues better handled by later lenses.

## Challenge signals

Reopen purpose when the architecture reveals a different effective behavior.
Reorder or deepen security, correctness, or production lenses when a newly
discovered boundary, durable state, public contract, or rollout constraint
changes their risk.

Use the shared worker result contract from `SKILL.md`.
