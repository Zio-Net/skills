# Progressive review checkpoint

Copy this structure to
`.review/progressive-code-review/<scope-id>/review.md` and fill every metadata
field that is known.

```yaml
---
schema: progressive-code-review/v1
review_id: ""
scope_id: ""
status: scouting
repo_root: ""
scope_kind: current-branch
base_ref: ""
target_ref: ""
merge_base: ""
include_worktree: true
initial_head: ""
current_head: ""
diff_fingerprint: ""
boundary_fingerprint: ""
included_pathspecs: []
excluded_change_groups: []
pull_request: ""
created_at_utc: ""
updated_at_utc: ""
---
```

## Intent and evidence

Record the stated problem, known acceptance criteria, source links or file
paths, assumptions, and open questions.

## Scout profile

Record change class, semantic size, blast radius, reversibility, evidence
quality, and whether a dedicated Scout worker was used.

## Agenda

| Order | Lens | State | Depth | Selection reason |
|---:|---|---|---|---|

Allowed lens states: `selected`, `skipped`, `in-progress`, `paused`,
`completed`, and `reopened`.

## Findings and dispositions

| ID | Lens | Priority | Location | Summary | Disposition | Gate | Evidence/change |
|---|---|---|---|---|---|---|---|

Allowed dispositions: `open`, `resolved`, `accepted-risk`, `deferred`,
`out-of-scope`, and `blocked-missing-evidence`.

Allowed gate states: `blocking` and `released`. Human-confirmed disposition may
release sequencing without erasing the finding or guaranteeing a ready verdict.

## Decision log

Record confirmed decisions, who confirmed them, accepted risks, non-goals, and
any challenge/reopen event. A recommendation is not a confirmed decision.

## Validation

Record commands/evidence, results, and validation that remains conditional or
unavailable.

## Current position

Record the active lens, stop reason, exact next action, and whether Scout must
run again before continuing.

## PR handoff draft

Keep only the current compact draft. Detailed review history belongs above, not
in the PR handoff.
