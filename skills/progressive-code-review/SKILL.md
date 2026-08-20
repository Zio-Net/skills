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
- Lens workers and any delegated evidence workers are always read-only, even
  during an authorized fix interlude.
- Use any non-destructive Git operations needed to inspect the requested
  change, including fetching missing refs, without asking permission. Ask
  before any additional operation that would modify repository or remote/PR
  state unless the user already authorized it.
- Always run the initial analysis. A user may override its agenda, but may not
  skip the analysis itself.
- After scope is resolved, explain the change to the user before rendering the
  lens agenda or starting a substantive lens. The first finding must not be the
  user's introduction to the changed flow.
- Run substantive lenses sequentially. Never run two lens workers in parallel.
- Use fresh workers for separate lenses and for every post-fix re-review.
- Compare repository state before and after each read-only worker. Stop only
  when drift affects reviewed content, the checkpoint, HEAD/current branch,
  the index, or evidence used by the active review.
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

This initial boundary is the discovery boundary. If Scout finds unrelated
change groups, the group or groups explicitly confirmed by the user become the
exact review path boundary. Keep excluded changed paths in repository-state
fingerprints and status diagnostics so drift remains visible, but exclude them
from lens evidence and findings. Reviewing another excluded group later is a
separate review scope.

When HEAD is detached, treat it as the target commit and ask for a base only if
the normal inference rules remain ambiguous.

### Explicit target branch

With `--branch <ref>`:

Review only committed changes on that target relative to the resolved base; do
not include the current checkout's working-tree changes or switch its branch.
Resolve base and target to exact commits before Scout, fetching missing refs as
needed under the non-destructive Git rule above.

If executing tests requires materializing the target, propose an isolated
temporary worktree and wait for authorization. A static review may proceed
without one if the evidence is sufficient.

### Base resolution

Resolve base in this order:

1. Explicit `--base <ref>`.
2. Pull-request metadata for the target, if a PR is available.
3. Applicable repository instructions that name the integration branch.
4. The target branch's configured upstream and merge base.
5. The remote default branch and merge base.
6. Ask the user when multiple plausible bases would produce materially
   different scopes.

Fetch missing base evidence as needed. Ask only when choosing among materially
different plausible bases requires a user decision.

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

Do not create a new checkpoint while an unrelated-group scope question is
unresolved. The scope-only Scout remains read-only and reports its groups in
chat. After the user confirms the exact group boundary, compute its boundary
fingerprint, then create or resume the matching checkpoint and persist the
scope decision with the Scout profile.

Choose one stable scope id:

- `pr-<number>` when a PR is known at review start and its full discovery
  boundary remains in scope.
- `pr-<number>-<boundary7>` when the user narrows a PR to confirmed change
  groups.
- `branch-<sanitized-branch>-<initial-head7>` for a named branch whose full
  discovery boundary remains in scope.
- `branch-<sanitized-branch>-<initial-head7>-<boundary7>` when the user narrows
  a named branch to confirmed change groups.
- `review-<utc-timestamp>-<initial-head7>` for any remaining full discovery
  boundary.
- `review-<utc-timestamp>-<initial-head7>-<boundary7>` for any remaining
  narrowed boundary.

Compute `boundary7` from the first seven hexadecimal characters of a SHA-256
over the canonical sorted included pathspecs plus base, target, merge base, and
working-tree inclusion mode. Record the full hash, included pathspecs, and
excluded change groups in checkpoint metadata.

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
and after an authorized fix, and before final handoff. For a pre-checkpoint
scope-only Scout, write its profile and confirmed boundary when the checkpoint
is first created.

## Run Review Scout

Read `references/lens-catalog.md` adjacent to the invoked `SKILL.md`. Workflow
resources come from that skill directory, not from another copy on the review
target. Do not load the seven full lens files yet.

Scout must inspect:

- the exact change boundary and changed-file summary;
- the original task, PR, issue, or specification when available;
- every explanation or question explicitly requested by the user at invocation;
- target-ref repository instructions and relevant architecture documentation;
- change type, semantic size, blast radius, reversibility, evidence quality,
  and risk signals.

Scout performs a bounded initial probe of all seven lenses. For each lens it
records one outcome: `skipped`, `light` (covered by the initial analysis), or a
dedicated `standard`/`deep` review. Select a dedicated review only when the
probe finds a concrete warning signal or a material evidence gap that it cannot
resolve and that could change the verdict or implementation direction. A
plausible question, broad change surface, or possible extra scrutiny is not
enough. Otherwise finish the direct check and mark the lens `light`.

Before selecting lenses, partition the included paths by coherent intended
outcome. Current-branch mode defines what must be noticed; it does not prove
that every staged, unstaged, untracked, and committed change belongs to one
review. If the boundary contains unrelated-looking groups and task/PR evidence
does not join them, stop after the scope profile, record
`blocked-missing-evidence`, show the groups, and ask which group or groups are
in scope. Do not finalize or start a lens agenda until that scope question is
resolved.

The coordinator performs the initial analysis and owns its classifications and
three user-facing blocks. It may delegate a bounded evidence lookup when useful,
but never delegates the synthesis or agenda.

If Scout or a lens takes longer than about one minute, send a short progress
update that names the active stage, the flow or boundary being traced, and what
evidence remains.

Use the three-part applicability gate in `references/lens-catalog.md` to
classify each lens. Applicability comes from a surface causally changed by the
reviewed artifact, not from subjects mentioned in documentation, people who
may read it, or downstream decisions it may inform. Only `standard` and `deep`
lenses receive dedicated workers.

## Explain the change and review plan

Before agenda confirmation or the first lens, render this complete shape,
translated into the user's language:

```markdown
### What this PR changes
<one plain-language sentence about the user or system outcome>
<a short before -> after explanation; use one compact flow or up to three
bullets when clearer than prose>

### How the review will run
<one or two sentences: how many light checks are complete, how many detailed
reviews remain, that they run sequentially, when the review pauses, and that it
ends with a compact PR handoff>

### Review plan
| Order | Review | Status | Depth | Why |
|---:|---|---|---|---|
<all seven lenses>
```

Use `checked` with `light`, `selected` with `standard`/`deep`, and `skipped`
with `—` for order and depth. Translate these labels when useful. Before asking
for confirmation, verify that all three blocks and all seven rows are present.
The confirmation response itself must contain the complete result; progress
updates do not satisfy this contract and the response must not refer to content
"above".

Keep all three blocks short. Expand only when the user explicitly asks for a
detailed explanation. If scope or primary flow is unclear, show the evidence
gap and resolve it before rendering the blocks.

Use plain review language in every user-facing message. Internal names such as
Scout, coordinator, worker, and dossier stay internal; say initial analysis,
light check, or detailed review and name what is actually being examined.

Use evidence-shaped reasons: `checked` names the direct path and why it was
enough; `selected` names the observed warning or material gap and the decision
a light check could not resolve; `skipped` names the absent or unchanged
surface. Prefer a focused initial agenda, typically about four dedicated lenses,
rather than selecting every lens by default. Select more whenever each
additional lens has a distinct unresolved signal or material evidence gap; if a
light check cannot support a confident `checked`, promote it.
Before rendering the table, downgrade any `selected` row whose reason only
describes the changed surface; its reason must name the unresolved signal that
requires a dedicated review.
If a changed surface was inspected and holds up, mark it `checked`, not
`skipped`. Substantial human-authored changes make simplicity at least a light
check; overlap with other lenses is not a reason to skip it.

Start the first lens immediately only for a small, unambiguous, low-risk scope.
Wait for agenda confirmation when scope is large, mixed, ambiguous, or
high-risk. The user may override selection, depth, or order.

## Build the lens context pack

Before each selected lens, prepare a compact context pack containing:

- exact base, target, merge base, diff fingerprint, and path boundary;
- Scout change profile and the active lens depth;
- the rendered change orientation and each user-requested explanation with its
  answered or evidence-gap state;
- original task/specification evidence and unresolved questions;
- applicable repository rules;
- prior lens results and human dispositions;
- confirmed decisions, accepted risks, non-goals, and assumptions;
- evidence commands suitable for the current-branch or ref-aware mode.

Previous decisions are working context, not immutable truth. A worker may
challenge them when new evidence contradicts them.

## Run one lens at a time

Before each selected lens, tell the user its position among selected lenses,
its human-readable name, and in one sentence what it checks and why it was
selected. Count only selected lenses in the total.

Load only the selected file under `references/lenses/` and give it to a fresh
read-only worker with the context pack.

If subagents are unavailable, disclose the fallback and run the same lens in
the coordinator. Do not pretend the result came from an independent worker.
Coordinator fallback is acceptable for the initial review, but it cannot count
as a fresh independent post-fix reviewer.

Workers may use non-destructive Git inspection, fetch missing evidence, and run
tests that do not rewrite tracked files. They must not change product files,
the index, current branch or local history, checkpoint state, or remote/PR
state.

Before and after each Scout or lens worker, compare content-sensitive
fingerprints for HEAD/current branch, the index, working tree, untracked files,
and checkpoint.

Stop only for drift that can affect the active review. Otherwise report it
briefly, refresh the fingerprint, and continue. If relevance is unclear,
inspect it first. Never undo or attribute drift to the worker without evidence.

### Worker result contract

Require this structure:

1. `Evidence checked`: concrete files, contracts, commands, tests, or docs.
2. `Findings`: zero or more findings with:
   - stable local id;
   - priority `P0`, `P1`, `P2`, or `P3`;
   - tight file/line location when applicable;
   - concrete failure or risk;
   - the step or component in the rendered change orientation where it occurs;
   - user/system impact;
   - evidence and why the reviewed change causes it;
   - evidence basis, distinguishing direct code evidence, test/runtime
     confirmation, inference, and missing evidence;
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

### User-facing review result

After each detailed review, render this self-contained shape, translated into
the user's language:

```markdown
### Review X of N — <plain-language name>
<the question checked, why it matters, and enough context to understand the
changed flow; use a compact arrow flow when it has three or more material steps>

#### What I checked
- <intent or domain rule>
- <main end-to-end code, data, or contract path>
- <relevant tests, runtime evidence, or explicit evidence gap>

#### What held up
<the material behavior and edge cases confirmed by the review>

#### Findings
##### <plain-language title> — <blocks merge | should fix | minor>
<current flow and concrete failure scenario>
<impact, precise evidence, and recommended direction>

#### What happens next
<whether review continues or pauses and the available choices>
```

Keep it compact, but preserve every applicable section. A detailed review with
material findings must explain the relevant flow and evidence, not collapse
into a verdict plus a short issue list. Omit `Findings` only when there are no
findings and say so explicitly. Put the complete result in the response that
ends the lens; progress updates do not satisfy this contract. Before sending,
verify that `Review X of N`, `What I checked`, `What held up`, and `What happens
next` are present, plus either `Findings` or an explicit no-findings statement.
Every detailed lens ends the current user turn; start the next one only after
the user continues, including when no findings were found.

Lead with behavior and impact; put method names and file references in the
evidence. Keep local finding ids, `P0`-`P3`, gate states, and dispositions in
the checkpoint. In chat say `critical`, `blocks merge`, `should fix`, or `minor`
instead. Remove internal ids and priorities from the user-facing draft before
sending. Never refer to a review only as first, current, or previous.


If worker evidence contradicts or materially expands the rendered orientation,
the coordinator must update and show the affected orientation before presenting
findings that depend on the new understanding. Do not make the user reconstruct
the corrected flow from a finding.

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
