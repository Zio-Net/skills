# Production readiness

## Mission

Verify that the changed system remains reliable, diagnosable, efficient, and
recoverable over time, under concurrency and load, and during partial failure,
deployment, and rollback.

## Select when / skip when

Select for asynchronous/distributed work, infrastructure, external
dependencies, retries, delivery semantics, concurrency, hot paths,
configuration, observability, rollout, or recovery. Skip for local
deterministic changes with negligible operational impact.

## Evidence to inspect

- Runtime topology, queues/events, external calls, storage transactions, and
  process boundaries.
- Retry, idempotency, deduplication, ordering, timeout, cancellation, and
  backpressure behavior.
- Logging, metrics, traces, alerts, correlation, and operator runbooks.
- Resource bounds, hot paths, query/call volume, caches, and cost drivers.
- Configuration defaults and validation, deployment sequencing, feature flags,
  migrations, rollback, and recovery procedures.
- Reliability/load/failure tests and relevant production evidence when
  available.

## Review sequence

1. Map the runtime path and every boundary at which work can be delayed,
   duplicated, reordered, partially committed, or lost.
2. Build a compact failure matrix for dependencies, process termination,
   timeout, cancellation, and retry.
3. Verify durable handoff, idempotency keys, deduplication scope, retry
   classification, and poison-work behavior.
4. Check concurrency, backpressure, resource limits, and amplification under
   load.
5. Determine whether logs/metrics/traces reveal the failure and let an operator
   identify affected work and recovery action.
6. Check configuration defaults, mixed-version deployment, migration order,
   rollback, and recovery.
7. Run focused operational evidence when safe; keep untested runtime claims
   conditional.

## Light / standard / deep

- `light`: inspect the direct runtime path, main dependency failure, and
  rollback/configuration implications.
- `standard`: cover duplicate/order/partial-failure semantics, observability,
  resource bounds, rollout, and recovery.
- `deep`: model failure combinations and mixed versions, inspect load/runtime
  evidence, and trace durable recovery end to end.

## Finding threshold

Report a plausible production scenario that loses or corrupts work, causes
unbounded amplification or cost, prevents recovery/rollback, or leaves a
material failure undetectable. State the trigger, operational impact, and
missing guarantee or signal.

## Do not report

- Generic calls for more logging, metrics, caching, or retries.
- Local correctness defects without an operational dimension.
- Performance claims unsupported by code-path evidence or measurement.

## Challenge signals

Reopen architecture when reliability requires a different transaction,
ownership, or boundary. Reopen correctness when the implementation's local
semantics are wrong before failure handling matters. Reopen scope when safe
rollout requires additional versioning, migration, or recovery work.

Use the shared worker result contract from `SKILL.md`.
