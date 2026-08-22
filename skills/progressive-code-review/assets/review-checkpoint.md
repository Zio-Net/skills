# Progressive review checkpoint

Copy this document to
`.review/progressive-code-review/<scope-id>/review.md` and fill every metadata
field that is known. Replace bracketed placeholders. Remove instructional
comments from the user-facing section and remove optional sections that do not
apply.

This checkpoint contains one canonical user-facing response plus internal
evidence and workflow state. Return the canonical section verbatim in chat; the
remaining sections are internal persistence.

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

## User-facing initial response

<!--
Write this entire section in the user's language. It is the exact Markdown to
return in chat after saving the checkpoint; do not summarize it again.
-->

### What this change is for

<!--
Give a self-contained orientation: the user or system problem, beneficiary,
intended outcome, important domain concepts, strongest evidence, and material
uncertainty. Scale detail to semantic complexity so a developer unfamiliar with
the change can understand what changed and why.
-->

[Purpose and context]

### How it works

**Before:** [Previous behavior]

**After:** [Changed behavior]

[One representative end-to-end scenario, including important fields, types, or
contracts and what they mean]

<!-- Add one small flow diagram or one concrete before/after data example only
when it materially improves understanding. -->

**Key decisions:** [Material design, compatibility, migration, or scope
decisions. Remove this line when none exist.]

### Answers to your questions

<!--
Optional. Include only for explicit explanatory questions about the change;
a review request, scope boundary, branch, base, or target is not such a
question. Answer each question or name its evidence gap. Remove this heading
when no explanation was requested.
-->

[Answers or named evidence gaps]

### How the review will run

<!-- Keep this to two or three short sentences. -->

[Say all seven areas received an initial check and some need no separate
review. Show selected detailed reviews in order as `A` → `B` → `C`. State the
blocking pause rule and that the review ends with a compact PR handoff.]

### Review plan

<!--
Include every lens exactly once. Put selected reviews first in their planned
execution order, then list checked or skipped lenses.
-->

| Order | Review | Status | Depth | Why |
|---:|---|---|---|---|
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |
| [number or —] | [Review] | [checked / selected / skipped] | [light / standard / deep / —] | [Evidence-shaped reason] |

[Checkpoint link]

[Agenda confirmation request or exact next action]

## Intent and evidence

Record the problem, beneficiary, intended outcome, acceptance criteria, source
links or file paths, assumptions, open questions, and evidence that would be
too detailed for the user-facing response.

## User-requested explanations

Include this section only when the invocation asks explicit explanatory
questions about the change. Mirror every answer shown in chat or its named
evidence gap. Omit the entire section otherwise: a review request, scope
boundary, branch, base, or target is not an explanatory question.

| Question | State | Answer or evidence gap | Evidence |
|---|---|---|---|

Allowed states: `answered` and `blocked-missing-evidence`.

## Change orientation

Record additional previous-flow, changed-flow, field, type, contract, design,
compatibility, migration, scope-decision, and assumption detail needed by later
lenses. When later evidence changes the shared understanding, update this
section and show the corrected user-facing orientation before any finding that
depends on it.

## Scout profile

Record change class, semantic size, blast radius, reversibility, evidence
quality, and whether a dedicated Scout worker was used.

## Agenda

| Order | Lens | State | Depth | Selection reason |
|---:|---|---|---|---|

Keep lens selection, depth, order, and conclusions consistent with the
user-facing review plan. This internal table may retain additional evidence.

Allowed lens states: `checked`, `selected`, `skipped`, `in-progress`,
`paused`, `completed`, and `reopened`.

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
