---
name: progressive-code-review
description: Use when the user explicitly invokes $progressive-code-review or explicitly asks for a progressive or staged branch review; do not use for an ordinary one-pass code review.
---

# Progressive Code Review

Run an evidence-driven review as a sequence of risk-selected lenses. Review the
current branch by default, preserve decisions between stages, and surface
load-bearing problems before details that depend on them.

## Non-negotiable behavior

- Do not edit product code unless the user explicitly asks for a fix.
- Lens workers and Scout workers are always read-only, even during an authorized
  fix interlude.
- Do not stage, unstage, commit, push, switch branches, publish a pull request,
  or change git history.
- Always run Review Scout. A user may override its agenda, but may not skip
  Scout itself.
- Run substantive lenses sequentially. Never run two lens workers in parallel.
- Use fresh workers for separate lenses and for every post-fix re-review.
- Snapshot content-sensitive repository fingerprints before and after every
  read-only worker. Stop on any unexpected repository-state delta.
- Report only issues introduced by the reviewed change or directly blocking its
  stated goal. Do not turn the review into a repository-wide audit.
- Treat deterministic checks as evidence, not as review lenses.

The checkpoint under `.review/` is the only file this workflow may create
without an explicit fix request.

## Invocation and arguments

Interpret these public forms:

```text
$progressive-code-review
$progressive-code-review --branch <ref>
$progressive-code-review --branch <ref> --base <ref>
```

Reject unknown options rather than guessing.

### Default: current branch

With no `--branch`:

1. Resolve the current branch and HEAD.
2. Infer the base using the rules below.
3. Review committed changes from the merge base through HEAD.
4. Also include staged, unstaged, and untracked working-tree files.
5. Exclude every path under `.review/` from status, diffs, evidence packs, and
   findings.

When HEAD is detached, treat it as the target commit and ask for a base only if
the normal inference rules remain ambiguous.

### Explicit target branch

With `--branch <ref>`:

1. Resolve the ref locally to a commit before doing any review work.
2. Review only committed changes on that target relative to the resolved base.
3. Do not include the current checkout's working-tree changes.
4. Do not checkout or switch to the target branch.
5. Inspect target files and instructions with ref-aware commands such as
   `git diff`, `git show`, and `git grep`. Do not accidentally read the current
   checkout as if it were the target branch.

If the ref is missing locally, ask before fetching. If executing tests requires
materializing the target, propose an isolated temporary worktree and wait for
authorization. A static review may proceed without one if the evidence is
sufficient.

### Base resolution

Resolve base in this order:

1. Explicit `--base <ref>`.
2. Pull-request metadata for the target, if a PR is available.
3. Applicable repository instructions that name the integration branch.
4. The target branch's configured upstream and merge base.
5. The remote default branch and merge base.
6. Ask the user when multiple plausible bases would produce materially
   different scopes.

Validate both base and target refs. Render the exact boundary before Scout:
base, target, merge base, whether working-tree changes are included, and the
checkpoint exclusion.

Treat `.review/` as reserved workflow-artifact space. Enumerate changed or
untracked paths there before finalizing scope. Keep them excluded as required,
but if anything exists outside the active progressive-review checkpoint, show
it and add a `blocked-missing-evidence` item until the user confirms that it is
an artifact and not product code. Never silently return `ready` while such a
path is unclassified.

## Start or resume the checkpoint

Use the template in `assets/review-checkpoint.md`.

Choose one stable scope id:

- `pr-<number>` when a PR is known at review start.
- `branch-<sanitized-branch>-<initial-head7>` for a named branch.
- `review-<utc-timestamp>-<initial-head7>` otherwise.

Sanitize path separators and invalid filename characters to hyphens. Do not
rename the scope id if a PR appears later; record the PR number inside the file.

Store state at:

```text
.review/progressive-code-review/<scope-id>/review.md
```

This path is intentionally not assumed to be ignored. Before creating it:

- canonicalize the repository root and intended checkpoint path, verify the
  target remains inside `<repo>/.review/progressive-code-review/<scope-id>/`,
  and reject a pre-existing symlink or reparse path that resolves outside;
- warn that it will appear in `git status`;
- verify it is excluded from review diffs, evidence packs, and ordinary status
  summaries; exact-path tracked/staged safety queries must still inspect it;
- look for a matching active checkpoint and resume it when its boundary
  metadata matches;
- if several checkpoints plausibly match, show the candidates and ask.

Record a new diff fingerprint whenever the reviewed state changes. Update the
checkpoint after Scout, after every lens, after every human disposition, before
and after an authorized fix, and before final handoff.

## Run Review Scout

Read `references/lens-catalog.md`. Do not load the seven full lens files yet.

Scout must inspect:

- the exact change boundary and changed-file summary;
- the original task, PR, issue, or specification when available;
- target-ref repository instructions and relevant architecture documentation;
- change type, semantic size, blast radius, reversibility, evidence quality,
  and risk signals.

For a small and obvious scope, the coordinator performs Scout. For a large,
mixed, ambiguous, or high-risk scope, use one dedicated read-only Scout worker.
The Scout worker returns only a change profile and proposed agenda, not
substantive findings or fixes.

Render an agenda containing every lens with:

- `selected` or `skipped`;
- `light`, `standard`, or `deep` when selected;
- a short evidence-based reason;
- execution order.

Start the first lens immediately only for a small, unambiguous, low-risk scope.
Wait for agenda confirmation when scope is large, mixed, ambiguous, or
high-risk. The user may override selection, depth, or order.

## Build the lens context pack

Before each selected lens, prepare a compact context pack containing:

- exact base, target, merge base, diff fingerprint, and path boundary;
- Scout change profile and the active lens depth;
- original task/specification evidence and unresolved questions;
- applicable repository rules;
- prior lens results and human dispositions;
- confirmed decisions, accepted risks, non-goals, and assumptions;
- evidence commands suitable for the current-branch or ref-aware mode.

Previous decisions are working context, not immutable truth. A worker may
challenge them when new evidence contradicts them.

## Run one lens at a time

Load only the selected file under `references/lenses/` and give it to a fresh
read-only worker with the context pack.

If subagents are unavailable, disclose the fallback and run the same lens in
the coordinator. Do not pretend the result came from an independent worker.
Coordinator fallback is acceptable for the initial review, but it cannot count
as a fresh independent post-fix reviewer.

Workers may run relevant read-only commands and tests that do not rewrite
tracked files. They must not change code, checkpoint state, git state, or PR
state.

Before dispatching Scout or a lens worker, capture:

- branch and HEAD;
- a content hash of the binary staged diff;
- a content hash of the binary tracked-worktree diff;
- a sorted manifest with content hashes for every untracked file, including the
  active checkpoint;
- porcelain status as a readable diagnostic.

Verify all fingerprints immediately after the worker returns and before
updating the checkpoint. Path lists alone are insufficient because a worker can
change an already-dirty, already-staged, or existing untracked file without
changing status. If a worker changes files, refs, branch, or index
unexpectedly, stop, report the exact delta, and do not undo it automatically.
Pre-approved disposable test artifacts are the only exception and must be named
in the context pack with their expected lifecycle.

### Worker result contract

Require this structure:

1. `Evidence checked`: concrete files, contracts, commands, tests, or docs.
2. `Findings`: zero or more findings with:
   - stable local id;
   - priority `P0`, `P1`, `P2`, or `P3`;
   - tight file/line location when applicable;
   - concrete failure or risk;
   - user/system impact;
   - evidence and why the reviewed change causes it;
   - recommended direction, not an implementation patch.
3. `Assumptions and evidence gaps`.
4. `Challenge/reopen` when prior context should be reconsidered.
5. `Validation signals`.
6. Explicit `no-new-concern` when no actionable finding exists.

Priority meanings:

- `P0`: immediate severe harm or a universally blocking defect.
- `P1`: must fix before merge because the change can cause material incorrect,
  unsafe, or unrecoverable behavior.
- `P2`: normal actionable defect or meaningful maintainability/operational risk.
- `P3`: low-risk improvement worth addressing but not merge-blocking.

Workers do not issue an overall merge verdict. The coordinator checks evidence,
deduplicates overlaps, preserves material disagreement, and owns the final
state.

## Gate, disposition, and resume

Pause before launching the next lens when any of these is true:

- an `open` `P0` or `P1` finding exists;
- an `open` finding can change task intent, scope, architecture, or the
  remaining agenda;
- a `blocked-missing-evidence` gap makes the current lens unreliable.

Clear passes and independent `P2/P3` findings may carry forward after being
shown and written to the checkpoint.

Use these dispositions:

- `open`
- `resolved`
- `accepted-risk`
- `deferred`
- `out-of-scope`
- `blocked-missing-evidence`

Only the user can confirm accepted risk, defer, or out-of-scope. Mark a finding
resolved only after a fresh worker re-runs the current lens against the updated
scope.

`open` and `blocked-missing-evidence` keep the gate closed. A human-confirmed
`resolved`, `accepted-risk`, `deferred`, or `out-of-scope` disposition releases
the sequencing gate so later lenses can run, but never erases the finding.
During final verdict, an accepted risk produces `ready-with-accepted-risk`; a
deferred or out-of-scope finding that still contradicts the stated goal keeps
the review `not-ready` until scope or risk disposition is made explicit.

### Explicit fix request in the same task

Do not refuse an explicit request to fix findings in the current task.
This section applies only after this skill was explicitly activated. A bare
"review and fix everything" request does not activate progressive review.

1. Show the finding and write the paused state to the checkpoint.
2. Briefly recommend a separate task for cleaner implementation context and
   reviewer independence, but do not require it.
3. In current-branch mode, suspend review and perform only the authorized
   implementation work with the normal coding workflow, preferably through a
   separate implementation agent.
4. In explicit-target mode, never edit the unrelated current checkout. Require
   either an authorized isolated worktree at the exact target or continuation
   in a task already rooted at that target. Otherwise keep the review paused.
5. Never let a dedicated lens worker fix its own finding. If coordinator
   fallback produced the finding and the coordinator performs the authorized
   fix, record the loss of independence.
6. Refresh boundary and fingerprint after the fix.
7. Use a genuinely fresh reviewer to re-run the entire current lens. If none is
   available, keep the finding open and pause rather than claiming resolution.
8. Re-run Scout before continuing if the fix introduces a new component,
   contract, data/security surface, change class, or other semantic expansion.

Within an explicit progressive-review invocation, an up-front request such as
"review and fix everything" counts as authorization for these fix interludes.
It does not authorize staging, committing, pushing, or publishing a PR.

## Complete the review

Derive one verdict:

- `ready`
- `ready-with-accepted-risk`
- `not-ready`
- `blocked-by-missing-evidence`

Keep the detailed lens history in the checkpoint. Build the PR-ready summary
using `assets/pr-handoff.md`:

- first inspect applicable target-repository PR instructions and templates;
- preserve required first lines, issue links, headings, or other mandatory
  content before applying the generic compact format;
- at most 180 words unless the user asks for more;
- one or two sentences for purpose;
- at most three key decisions;
- at most two material non-goals or accepted risks;
- one validation line;
- one reviewer-focus line;
- omit empty sections and resolved-finding history.

Repository PR requirements override the generic section order. Keep the result
within 180 words when possible; if mandatory repository metadata alone makes
that impossible, follow the repository rule and explain the exception.

Return the verdict and compact PR handoff in chat. Do not publish it.

## Clean up only after durable handoff

Do not delete the checkpoint merely because a PR exists. Delete it only after
the user confirms that the handoff was published, or after read-only
verification proves the material is present in the PR.

Before deletion:

1. Canonicalize the target and verify it is inside
   `<repo>/.review/progressive-code-review/<active-scope-id>/`.
2. Query the exact checkpoint path and verify the file is not tracked.
3. Query the exact checkpoint path and verify the file is not staged. This
   safety query is required even though `.review/**` is excluded from review
   scope and ordinary status summaries.
4. If either check fails, stop and tell the user what must be cleaned manually.
   Do not alter the index.
5. Delete only `review.md` and its now-empty scope directory.

Keep the checkpoint for paused, not-ready, blocked, abandoned-without-cleanup,
or not-yet-published reviews.

## Resources

- `references/lens-catalog.md`: Scout selection, depth, and ordering rules.
- `references/lenses/*.md`: one evidence-driven procedure per lens.
- `assets/review-checkpoint.md`: durable in-repository review state.
- `assets/pr-handoff.md`: compact final PR description contract.
