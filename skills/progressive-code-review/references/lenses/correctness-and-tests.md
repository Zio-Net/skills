# Correctness and tests

## Mission

Determine whether the changed implementation preserves its invariants across
success, boundary, state-transition, error, and cancellation paths, and whether
the tests can detect the meaningful regressions.

## Select when / skip when

Select for executable logic, changed state, parsing, calculations, error paths,
concurrency, or any behavior with regression risk. Skip for documentation-only
or generated-only scope with no executable semantic change.

## Evidence to inspect

- Changed implementation plus direct and transitive callers/consumers.
- Domain invariants, types, validation, nullability, and state machines.
- Error, timeout, cancellation, retry, and cleanup paths.
- Relevant unit, integration, contract, and regression tests.
- Test assertions, fixtures, mocks, and whether the test fails for the intended
  defect rather than only exercising a line.

## Review sequence

1. Write the invariants and pre/postconditions the changed path must preserve.
2. Trace the primary path with concrete representative values.
3. Exercise null, empty, minimum/maximum, malformed, duplicate, stale, and
   unsupported inputs where applicable.
4. Follow every changed state transition, including repeated and out-of-order
   calls.
5. Trace exceptions, errors, cancellation, resource cleanup, and partial work.
6. Inspect local race or concurrency behavior; route distributed operational
   semantics to production readiness.
7. Map each material risk to a test and verify the assertion would fail for the
   defect.
8. Run the narrowest useful deterministic checks when safe.

## Light / standard / deep

- `light`: trace the direct path, highest-risk boundary, and focused tests.
- `standard`: inspect callers/consumers, state/error paths, edge cases, and
  regression-test quality.
- `deep`: model the full state space or concurrency interaction, inspect
  property/integration evidence, and challenge test doubles and assumptions.

## Finding threshold

Report a concrete input, state, ordering, or failure path that produces an
incorrect result, loses required work, violates an invariant, or escapes the
tests. Explain how to reach it and why current validation does not prevent it.

## Do not report

- Purely hypothetical failure with no reachable path.
- General requests for more tests without naming the missing regression.
- Style, broad simplification, or production scaling concerns owned by other
  lenses.

## Challenge signals

Reopen purpose when tests reveal disputed expected behavior. Reopen
architecture when correctness depends on ambiguous ownership or a broken
contract. Deepen production readiness when retries, delivery, concurrency, or
partial failure cross process boundaries.

Use the shared worker result contract from `SKILL.md`.
