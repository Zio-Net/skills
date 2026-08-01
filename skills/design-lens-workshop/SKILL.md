---
name: design-lens-workshop
description: Use when a user explicitly asks to workshop, deepen, challenge, or compare a feature's technical design before implementation. Do not use for ordinary coding requests or implementation plans.
---

# design-lens-workshop

## Purpose

Drive the design lenses as a real, point-of-use **workshop** — not a checklist skimmed once and not a finished design handed down before discussion. By default, facilitate the selected lenses one at a time with the human. When parallel scan mode is explicitly requested, run independent lens scans and synthesize their concerns into a decision agenda before co-design. Finish with a reviewable design handoff.

This is a standalone workshop. It has no lifecycle, gate, selector, planning, or implementation role. Do not write code, implementation plans, task lists, or governance records, and do not continue into planning when the workshop ends.

## The Big Picture (read this first, every time)

You are facilitating a design workshop with a human. Lenses are perspectives on one design. Workshop mode works through selected lenses one at a time with the human and is the default. Parallel scan mode runs an optional independent multi-lens scan that discovers concerns and decision points before discussion; it is not a shortcut to a finished design. The lenses live in `references/lenses/`:

- `architecture-core`
- `component-design`
- `requirements-nfr`
- `ui-ux`
- `data-storage`
- `security-compliance`
- `integration-api`
- `devops-operations`
- `observability-resilience`

Before composing the agenda, read only the `Applicability Signals` section from each of the nine lens files. Do not load their remaining content yet. This is a lightweight applicability scan, not a selector or questionnaire.

Treat `architecture-core`, `component-design`, and `requirements-nfr` as foundational. Always include all three in the selected set, then use the applicability scan to select any of the remaining six lenses that can materially shape or validate this design. Foundational means always evaluated, not always `full`. In workshop mode, every selected foundational lens still gets a visible turn; when it introduces no new concern, use `light` depth, render an explicit no-new-concern finding, and ask the human to confirm, correct, or move on. In parallel scan mode, its worker may return that finding to the coordinator without a separate human turn.

After the applicability scan:

- **Workshop mode:** fully load only the active lens file, facilitate it using the method below, capture its result, reload this method, and then load the next lens.
- **Parallel scan mode:** create one independent, read-only analysis task per selected lens. When the host supports delegation, assign each task to a separate worker and run as many concurrently as capacity allows; batching is fine. Give every worker the same design brief and evidence boundary plus only its assigned lens reference. The worker may inspect relevant sources inside that boundary but must not load other lens references. If delegation is unavailable, say so plainly and execute the same lens tasks sequentially; never imply that independent workers ran.

Each parallel-scan worker returns only: relevant evidence, concerns and risks, assumptions, real decision forks, questions that could change the design, validation signals, or an explicit no-new-concern finding. Workers do not produce the final design, make cross-lens decisions, write files, or continue into planning. The coordinator consolidates their findings and owns the conversation.

In both modes, treat each `Question Bank` as prompts for reasoning, not a questionnaire to hand to the human. Do not ask merely because a lens contains a question.

The per-lens *knowledge* is in the lens Markdown; this skill is the *method* that ties the lenses together. Keep both in view.

**Render before you ask.** Before you raise any confirm, approve, move-on, or choice question about the workshop agenda, a diagram, the component map, an option set, or a design verdict, the material MUST already be rendered in your message in this exchange. Present the current understanding, evidence, recommendation or real alternatives, and assumptions fully enough to review. The question may reference only content that is on screen. Never ask the human to approve a count, a summary, an unseen file, or a "shown above" design that was not actually shown. Use a structured choice only for a genuinely discrete decision whose full alternatives are visible; free-text discussion always remains available.

## Interaction modes

Use **workshop mode** by default. Do not ask the human to choose a mode before starting.

- **Workshop mode (default):** treat every selected lens as interactive. Give it its own visible discussion and human response, delegation, or skip before moving to the next lens. Offer all-at-once versus one-at-a-time pacing for a dense lens.
- **Parallel scan mode:** use only when the human explicitly asks for parallel scan mode or parallel mode. Investigate every selected lens independently, consolidate concerns and questions, and stop at a prioritized cross-lens decision agenda. Do not turn the initial scan into a preferred design or design handoff.

The human may switch modes at any point. A parallel-scan concern cluster may continue in workshop mode without changing how the remaining clusters are handled.

## First stage — build the design brief

Before the lens-applicability agenda, build the smallest useful design brief. This is lightweight technical framing, not product discovery.

Check context in this order:

1. The current conversation.
2. User-provided specs, documents, diagrams, or examples.
3. Relevant repository code, configuration, tests, and documentation.

Render:

- the desired outcome and affected users;
- scope and non-goals;
- binding constraints;
- existing system context and evidence.

Tag every material statement as `Known`, `Assumed`, or `Open`. Do not silently turn an assumption into a requirement. If context is too weak for technical design, say what is missing and continue with explicit provisional assumptions.

Ask a question only when all of these are true:

- the answer cannot be established efficiently from the available evidence;
- the human is the appropriate source or decision owner;
- the answer can materially change the recommendation, risk, or validation approach;
- choosing and clearly labelling a provisional default would be unsafe or misleading.

Otherwise inspect, infer, or recommend. In particular, do not ask the human to invent cost, latency, reliability, or quality thresholds before researching existing behavior and proposing a defensible target or range.

## The Method (the same for every lens)

1. **Frame the workshop.** Tell the human up front: the workshop selects relevant technical lenses, works them one at a time by default, asks only for input that can materially change the design, and ends with a design handoff — not an implementation plan. State that they can request parallel scan mode when they want the main concerns and questions surfaced first.

   Keep the human oriented while preparing. Explain that the agenda will show each selected lens, its depth, and the decision or risk it will examine; in parallel scan mode it also shows the independent scan angle. Say plainly that they can correct the framing, ask for an explanation, bring an artifact, delegate a decision, or request different pacing at any point.

2. **Infer applicability.** Read only the `Applicability Signals` sections from all nine lens files. Always select the three foundational lenses: `architecture-core`, `component-design`, and `requirements-nfr`. Use the design brief, evidence, and applicability signals to determine which of the remaining six lenses apply. Never make the human answer obvious applicability questions. A lens can shape the parallel scan without creating its own human turn.

   Select a depth by risk and novelty:

   - `full` — several consequential or costly-to-reverse decisions;
   - `medium` — bounded decisions with meaningful trade-offs;
   - `light` — one narrow check or confirmation that no new concern is introduced.

   Right-size the workshop. The three foundational lenses are always selected; select every additional lens that can materially shape or validate the design, and skip the rest with a feature-specific reason. Do not inflate a foundational lens into ceremonial work: when it introduces no new concern, keep it `light` and say so explicitly. In workshop mode, depth still varies but every selected lens is interactive. Ordering a lens later is not skipping it.

   Render the mode-appropriate agenda in-band before asking for input or proceeding.

   Workshop mode (default):

   ```text
   Workshop agenda — <N> lenses
   <lens-id> (<full | medium | light>, interactive) — <the decision this lens will work through>
   <lens-id> (<full | medium | light>, interactive) — <the decision this lens will work through>

   Skipped:
   <lens-id> — <why it does not apply here>
   ```

   Parallel scan mode:

   ```text
   Parallel lens scan — <N> lenses
   <lens-id> (<full | medium | light>, independent) — <the angle, concern, or risk to investigate>
   <lens-id> (<full | medium | light>, independent) — <the angle, concern, or risk to investigate>

   Skipped:
   <lens-id> — <why it does not apply here>
   ```

   Fill one line per selected lens with its depth and **concrete decision or risk**, not only its name.

   In workshop mode, stop and wait for agenda confirmation before opening the first lens. In parallel scan mode, do not spend a separate turn confirming an unambiguous agenda: run the independent scans, render their concern consolidation and global question agenda, and stop for the human. The human may correct the lens scope in that reply.

3. **Work evidence-first and co-design before conclusions.** Establish the relevant decision points, evidence, assumptions, trade-offs, and validation signals before settling a direction.

   In workshop mode, keep one active lens at a time. Fully load it, then render the current understanding, evidence, and assumptions. Begin with an open question when one passes the question gate. For an evidence-backed `light` no-new-concern finding, ask only for `confirm / correct / move on` rather than inventing an open question. For a dense lens, offer all-at-once versus one-at-a-time pacing. Do not open the lens with a finished design. Wait for the human before moving to the next lens.

   In parallel scan mode, consolidate related worker findings into one or more concern clusters. Keep question counts out of individual clusters:

   ```text
   Concern cluster — <cross-lens concern or unresolved decision>
   Lenses: <contributing lens ids>
   Evidence: <known context and source>
   Concerns: <risks, tensions, and why they matter>
   Real forks: <candidate directions only when there is a genuine fork>
   ```

   After all clusters, render exactly one global agenda:

   ```text
   Global parallel-scan question agenda
   Critical blockers — answer first (recommended: up to 5 total):
   1. <answer needed to choose a direction or control a major risk>
   Prioritized follow-ups (recommended: up to 10 total):
   1. <important question that can wait until the blocker is resolved>
   ```

   Create one global question agenda across the entire parallel-scan response, not one budget per cluster. Keep critical questions scarce: recommend up to five blockers whose answers can change direction, expose unacceptable risk, or determine validation. Recommend up to ten material follow-ups; fewer are valid. These are soft limits: never hide a genuinely important question only to meet a number. When either list exceeds its recommendation, show the necessary questions, group and rank them, and make the overload visible. Ask the human to answer the critical set first. Treat follow-ups as an agenda for later discussion, not a demand to answer everything at once.

   After rendering the parallel-scan concern consolidation and global question agenda, stop and wait. Do not provide a finished component map, preferred architecture, or design handoff until the relevant load-bearing clusters have been discussed, delegated, skipped, or explicitly left open.

   Match the question form to the question. Use free text for constraints, intent, responsibilities, and genuinely open design work. For a discrete, enumerable choice, render the alternatives and an `other / let me explain` path before asking. Compare options only when there is a real fork; never manufacture alternatives to make the workshop look thorough.

   Adapt depth and explanation to the human's expertise. Explain trade-offs, failure modes, reversibility, and what evidence would change a direction. In workshop mode, begin with evidence and an open question when one passes the question gate; use the short review prompt above for an evidence-backed `light` no-new-concern finding. Offer a recommendation after the human's intent and constraints are understood. In parallel scan mode, workers may identify promising directions but do not recommend the final design. Use open co-design whenever vocabulary, responsibilities, risk acceptance, or desired outcomes depend on human intent.

4. **Surface visuals IN-BAND so the human can SEE them.** During the initial parallel scan, use a view only when it makes a concern, conflict, or decision fork easier to understand; do not use an integrated diagram to smuggle in a finished design. In workshop mode, for a `full` or `medium` structural or UI lens, expect a native shared view unless it would add no clarity for this feature. For a `light` lens, use one only when it materially clarifies the decision. After the relevant decisions are discussed, use the smallest set of shared views that explains the emerging design. Never create a ceremonial diagram. On a text-only host, a fenced Mermaid block may be source text rather than a rendered picture, so render console ASCII inline by default. A richer file may supplement the inline view, but never replace the material the human is asked to review.

   At any approval point, the relevant diagram or component map MUST be rendered in the same exchange as the question. Never stand in a reference to it or a bare count. If the human changes it, render the updated form before asking again. Ask whether they have an existing diagram, screenshot, Figma file, whiteboard photo, or document when that evidence would materially affect the design.

5. **Co-design structure where it is consequential.** For architecture and component work:

   - Determine whether the design method or decomposition style creates a real, costly-to-reverse fork. In workshop mode, co-decide it. In parallel scan mode, workers identify viable decompositions, evidence, and boundary concerns; the coordinator surfaces the meaningful fork without choosing it silently.
   - **When structure becomes the active decision, build the component map — render the FULL form, never a summary or count.** Put in the message: first, an inline diagram of all components and dependency arrows; then a named list grouped by the chosen vocabulary, with every component's one-line responsibility.
   - Walk at least one important user-and-system flow through the map. Invite corrections when the full map is visible. In parallel scan mode do this only after the relevant concern cluster has been opened with the human; never treat approval of unseen details as agreement.
   - If the human asks for a change, re-render the affected form before asking for approval again.
   - Only then present remaining trade-off options such as transport, storage technology, or granularity.

   Use this fill-in template:

   ```text
   Proposed component map

   <inline diagram: every component and dependency arrow>

   <vocabulary group 1 — Managers | Bounded Contexts | Layers | Services>:
     <ComponentName> — <one-line responsibility>
     <ComponentName> — <one-line responsibility>
   <vocabulary group 2 — Engines | Aggregates | Repositories | ...>:
     <ComponentName> — <one-line responsibility>

   Key flow: <actor action> -> <Component> -> <Component> -> <outcome>
   ```

   After the human provides the direction needed for the active structural decision, drive the first draft unless they ask to lead. Explain more where their architecture expertise is low, but never author human agreement silently.

6. **Capture results honestly.** During the initial parallel scan, capture concerns and concern clusters as `open`, not as decisions. Record an evaluated lens with no material design impact as a finding, not a forced decision:

   ```text
   Finding — no new concern
   Lens: <lens id>
   Evidence: <why the existing design remains sufficient>
   Review: agent-assessed | human-reviewed
   ```

   `agent-assessed` is not human agreement. In workshop mode, render the finding and wait for confirmation, correction, or explicit move-on before marking it `human-reviewed`. After discussion, capture cross-lens decisions; in workshop mode capture the active lens result:

   ```text
   Decision — <decision name>
   Lenses: <contributing lens ids>
   Choice: <selected direction>
   Why: <reasoning and evidence>
   Consequences: <constraints, risks, and implications>
   Status: recommended | confirmed | delegated | skipped | provisional | open
   ```

   Use `recommended` for an agent-recommended direction that is rendered with evidence but not explicitly accepted. Use `confirmed` only after the human reviews the rendered decision. Use `delegated` only after an explicit "you decide" and `skipped` only after an explicit skip. Use `provisional` for a default that depends on a material assumption. A human response may confirm several clearly rendered recommendations within the current active lens, never across several workshop-mode lenses at once.

   Carry settled decisions, assumptions, and constraints into later lenses. If a later lens invalidates an earlier decision, reopen it as `provisional`, re-render the affected design, and obtain confirmation again.

   Before handoff, count-check the agenda: every selected lens must have contributed to a rendered decision, risk, validation signal, or explicit no-new-concern finding. In workshop mode every selected lens needs an actual human response; a no-new-concern finding closes only after human review, otherwise the lens remains `open`. In parallel scan mode every load-bearing concern cluster needs a human response, delegation, skip, or honest `open`/`provisional` status. The initial scan alone never counts as design acceptance. Do not silently mark a recommendation as confirmed.

7. **Checkpoint when useful.** Remain chat-only by default. In parallel scan mode offer a checkpoint after the concern consolidation, after consequential discussion, or before a pause; do not announce internal worker transitions. In workshop mode offer one after a consequential lens, then announce and load the next lens. Resume from the conversation while it remains available. Durable resume across an unexpected exit, lost conversation, or agent change is guaranteed only by a saved checkpoint or export; never claim otherwise.

## Design synthesis and handoff

Only after the selected workshop-mode lenses or parallel-scan concern clusters have been discussed, delegated, skipped, or honestly left open, reconcile conflicts and dependencies. The initial parallel-scan concern consolidation must stop before design synthesis. Surface decisions that constrain several areas. Include named components and responsibilities, at least one important end-to-end flow, key risks, and validation signals. Add other views only when they clarify the design.

Return the design handoff in the conversation with these seven sections:

1. Design brief, context, and evidence.
2. Lens scope and decisions, including selected lenses, skipped-lens reasons, and keeper links.
3. Proposed design, components, key flows, and any agreed UI layout.
4. Alternatives and rationale.
5. Risks, quality, and operational considerations.
6. Assumptions and open questions.
7. Validation signals.

Stop there. Planning and implementation are separate workflows.

## Artifact contract

Remain chat-only by default. Treat `checkpoint`, `export`, and `save to <path>` as natural-language instructions, not CLI parameters.

Offer a checkpoint just in time before a pause or when the workshop grows long. Before the first write, show its purpose and path once; an explicit save request to that path already counts as agreement. Otherwise ask before writing. Choose the path in this order:

1. User-provided path.
2. Existing repository convention.
3. `.design-workshop/<feature-slug>/`.

Use `assets/design-record.md` for a checkpoint or final export. Create a separate lens keeper only for substantial material that is lossy in chat, such as a diagram, contract table, state model, or UX sketch. Render it in the conversation before using it for a decision, then link it from the record.

A checkpoint may omit sections that are not useful yet. A final export must keep all seven design-handoff sections, including lens scope and keeper links; write `None` when a section is empty rather than deleting it. Mark the overall record `Handoff complete` when the workshop and export are complete. Use overall `Accepted` only after the human explicitly accepts the whole design and no load-bearing decision remains unresolved.

Never save a transcript, implementation tasks, an implementation plan, JSON state, governance data, or files under a vendor-owned user directory. Do not delete working artifacts automatically; at the end, offer to keep or clean them up.

## Review Standard

This skill is doing its job only when, in the human's view: workshop mode was the default and preserved genuine one-lens-at-a-time co-design; parallel scan mode used one independent analysis task per selected lens when delegation was available, disclosed any sequential fallback, and returned concern consolidation plus one prioritized global question agenda — scarce critical blockers first, material follow-ups second, with soft recommended limits — instead of a finished design; diagrams and option sets were visible before approval; every decision has honest status and provenance; and the final handoff preserves the design without turning it into a plan.

## Lineage

This is a standalone adaptation with deliberate subtraction and handoff-oriented reframing of the latest [Specrew Design Workshop](https://github.com/alonf/specrew) method and its nine-lens knowledge pack. Workshop mode retains the Specrew workshop spine; parallel scan mode is a standalone ZioNet extension. The skill removes Specrew lifecycle, gates, Spec Kit integration, mandatory persistence, product-domain and code-implementation phases, and repository governance. It neither requires Specrew nor represents an official Specrew distribution.
