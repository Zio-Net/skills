# Simplicity and code health

## Mission

Find behavior-preserving simplifications that reduce the cost of understanding,
testing, and safely changing the reviewed code without expanding into a broad
refactor.

## Select when / skip when

Select after functional lenses for human-authored code or documentation with
meaningful complexity. Skip for generated files, lockfile-only changes, or a
scope too small to simplify usefully.

## Evidence to inspect

- The changed code and the smallest necessary adjacent context.
- Representative human-authored hotspots, separated from generated or
  mechanical change volume.
- Repository conventions, existing utilities/patterns, tests, and applicable
  documentation requirements.
- Control flow, abstraction boundaries, duplication, naming, comments, and
  dead or redundant paths.

## Review sequence

1. State the behavior that must remain unchanged.
2. Identify unnecessary branching, nesting, indirection, wrappers, and
   abstraction layers introduced by the change.
3. Check whether duplicated logic should use an existing local mechanism,
   without inventing a premature general framework.
4. Remove concepts that carry no distinct invariant or responsibility.
5. Check naming, comments, and documentation for accurate intent rather than
   narration of syntax.
6. Prefer the smallest local simplification whose safety can be demonstrated by
   existing or focused tests.

## Light / standard / deep

- `light`: sample representative human-authored changes for duplication,
  nesting, indirection, and convention mismatches. Layered file organization
  alone is not sufficient evidence. If a large or cross-layer change cannot be
  cleared with concrete evidence, use `standard`.
- `standard`: inspect the containing module and existing patterns for safe
  consolidation or removal.
- `deep`: use only when complexity spans the reviewed feature; trace whether
  abstractions encode real responsibilities before recommending a bounded
  redesign.

## Finding threshold

Report when the change adds demonstrably unnecessary complexity, duplication,
dead behavior, misleading naming/comments, or required documentation
inconsistency, and when a bounded safer direction exists. Most findings in this
lens should be `P2` or `P3` unless complexity directly masks a severe defect.

## Do not report

- Repository-wide cleanup, naming churn, or rewriting stable neighboring code.
- Personal style preferences already accepted by repo conventions.
- A large abstraction justified only by possible future reuse.
- Simplification that changes behavior or weakens explicit contracts.

## Challenge signals

Reopen correctness when complexity hides a reachable behavior defect. Reopen
architecture when apparently redundant structure actually represents disputed
ownership, or when simplification would cross a real boundary. Do not silently
collapse a confirmed tradeoff.

Use the shared worker result contract from `SKILL.md`.
