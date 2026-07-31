---
name: design-lens-workshop
description: Use when a user explicitly asks to workshop, deepen, challenge, or compare a feature's technical design before implementation. Do not use for ordinary coding requests or implementation plans.
---

# design-lens-workshop

## Purpose

Drive the design lenses as a real, point-of-use **workshop** — not a checklist skimmed once. Facilitate the design with the human, one lens at a time, and finish with a reviewable design handoff.

This is a standalone workshop. It has no lifecycle, gate, selector, planning, or implementation role. Do not write code, implementation plans, task lists, or governance records, and do not continue into planning when the workshop ends.

## The Big Picture (read this first, every time)

You are facilitating a design conversation with a human, **one lens at a time**. The lenses live in `references/lenses/`:

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

For each selected lens you work:

1. **Fully load only the active lens's Markdown file** (`references/lenses/<lens-id>.md`) for its `Design Decision Points`, `Workshop Conduct`, `Question Bank`, trade-off dimensions, validation signals, and source notes. Do not improvise it from memory.
2. **Facilitate** the discussion for that lens using the method below.
3. **Capture** the decision, rationale, consequences, and honest agreement status in the conversation.
4. **Reload this method** and carry settled constraints into the next lens. Load only the next lens's full file when that lens starts.

The per-lens *knowledge* is in the lens Markdown; this skill is the *method* that ties the lenses together. Keep both in view.

**Render before you ask.** Before you raise any structured confirm, approve, move-on, or choice question about the workshop agenda, a diagram, the component map, an option set, or a design verdict, the material MUST already be rendered in your message in this exchange. Explain it fully enough to review, and make the question and any option labels self-explanatory. The question may reference only content that is on screen. Never ask the human to approve a count, a summary, an unseen file, or a "shown above" design that was not actually shown.

**Open each lens with a presentation and an open question, never a menu.** Present its relevant decision points, current understanding, evidence, and assumptions first. End with a free-text design question. Use a structured choice only later for a genuinely discrete decision whose full alternatives are already visible; free-text discussion always remains available.

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

Tag every material statement as `Known`, `Assumed`, or `Open`. Do not silently turn an assumption into a requirement. Ask only questions whose answers can change a design decision, risk, or validation approach. If context is too weak for technical design, say what is missing and continue only with assumptions the human accepts.

## The Method (the same for every lens)

1. **Frame the phases + prepare to hand over the agenda.** Tell the human up front: the workshop selects the relevant technical lenses, works them one at a time, co-designs the system structure and important flows with them, reconciles cross-lens constraints, and ends with a design handoff — not an implementation plan.

   Keep the human oriented while preparing. Explain that the agenda will list the selected lenses and the concrete decision each will ask them to make. Say plainly that they can answer in free text, ask for an explanation, correct the framing, bring an artifact, or request one-at-a-time pacing at any point.

2. **Infer applicability, then hand over and confirm the agenda.** Read only the `Applicability Signals` sections from all nine lens files. Use the design brief and evidence to propose which lenses apply WITH your reasoning; ask the human only to confirm or adjust. Never make them answer obvious yes/no applicability questions, and never silently auto-resolve a material area.

   Select a depth by risk and novelty:

   - `full` — several consequential or costly-to-reverse decisions;
   - `medium` — focused decisions with meaningful trade-offs;
   - `light` — one narrow decision or confirmation that no new concern is introduced.

   Right-size the workshop. There is no mandatory core set and no fixed nine-lens marathon. Select every lens that raises a material decision for this feature; skip the rest with a feature-specific reason. Ordering a lens later is not skipping it.

   Render the agenda in-band before asking for confirmation or adjustment:

   ```text
   Workshop agenda — <N> lenses

   <lens-id> (<full | medium | light>) — <the decision this lens will ask for THIS feature>
   <lens-id> (<full | medium | light>) — <the decision this lens will ask for THIS feature>
   ...

   Skipped:
   <lens-id> — <why it does not apply here>
   ```

   Fill one line per selected lens with its depth and the **concrete decision it raises**, not only its name. Agenda confirmation approves only the selected lens list, order, and depths. It does not answer any lens. Do not offer or accept a batch shortcut as per-lens agreement.

   **Stop and wait for agenda confirmation or adjustment. Do not open the first lens in the same exchange.**

3. **Per-lens facilitated discussion — open with a presentation + an OPEN question, never a menu first.** The first turn of every lens MUST present the lens purpose, relevant decision points, current understanding, evidence, and explicit assumptions, then ask a free-text question about the first load-bearing decision.

   Use this opening shape:

   ```text
   Current lens — <lens-id>

   This lens decides:
   - <relevant decision point>
   - <relevant decision point>

   Current understanding:
   <evidence and assumptions>

   Pacing: answer these together, or ask me to take them one at a time.
   Open question: <first load-bearing question>
   ```

   **Keep one active lens at a time; a lens may span multiple exchanges.** Do not bundle several lenses into one presentation or one confirm-all question. Focus on exactly one lens's decision points, ask for that lens's answer, and wait for the human before moving on.

   **Pace a dense lens — after presenting, you MUST offer all-at-once OR one-at-a-time.** A lens with three or more relevant decision points becomes an overwhelming wall when one question secretly bundles several subjects. Respect the human's pacing choice. A light, single-decision lens skips the offer and asks its one open question.

   Match the question form to the question. Use free text for constraints, intent, responsibilities, and genuinely open design work. For a discrete, enumerable choice, render the alternatives and an `other / let me explain` path before asking. Compare options only when there is a real fork; never manufacture alternatives to make the workshop look thorough.

   Develop the design with the human. Adapt depth and explanation to their expertise. Explain trade-offs, failure modes, reversibility, and what evidence would change the recommendation. Give an evidence-based recommendation only after the relevant co-design discussion. Iterate until the human confirms, corrects, delegates, skips, or leaves the decision explicitly open.

4. **Surface visuals IN-BAND so the human can SEE them.** For a `full` or `medium` architecture, component, data, security, integration, deployment, or UI lens, expect a native shared view unless it would add no clarity for this feature. If you omit it, state the feature-specific reason. For a `light` lens, use a diagram or table only when it materially clarifies structure, state, trust, coupling, data ownership, deployment, interaction, or failure behavior. Never create a ceremonial diagram. On a text-only host, a fenced Mermaid block may be source text rather than a rendered picture, so render console ASCII inline by default. A richer file may supplement the inline view, but never replace the material the human is asked to review.

   At any approval point, the relevant diagram or component map MUST be rendered in the same exchange as the question. Never stand in a reference to it or a bare count. If the human changes it, render the updated form before asking again. Ask whether they have an existing diagram, screenshot, Figma file, whiteboard photo, or document when that evidence would materially affect the design.

5. **Co-design — do NOT hand down finished options.** For architecture and component work:

   - **Co-decide the design method or decomposition style** when it matters: for example bounded contexts, volatility-based decomposition, modular monolith, services, or layers. Discuss candidates and trade-offs; record the choice as a constraint. Do not silently assume it.
   - **Co-build the component map — render the FULL form, never a summary or count.** Put in the message: first, an inline diagram of all components and dependency arrows; then a named list grouped by the chosen vocabulary, with every component's one-line responsibility.
   - Only after the diagram and full named list are visible, invite the human to approve, rename, split, merge, remove, or reassign components. Walk at least one important user-and-system flow through the map together.
   - If they ask for a change, re-render the updated diagram and list, then ask again. Iterate until the decomposition, responsibilities, and flow are agreed or honestly left open.
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

   Where the human's architecture expertise is low, drive the first draft and explain more, but still confirm the responsibilities and flows rather than authoring agreement silently.

6. **Capture the agreements honestly.** Before moving on, render the lens result:

   ```text
   Decision — <lens-id>
   Choice: <selected direction>
   Why: <reasoning and evidence>
   Consequences: <constraints, risks, and implications>
   Status: accepted | delegated | skipped | provisional | open
   ```

   Use `accepted` only after the substantive lens questions were surfaced and confirmed. Use `delegated` only after an explicit "you decide" and `skipped` only after an explicit skip. Agenda approval, silence, or a batch "looks good" is not per-lens agreement. A light decision may close with a short acknowledgement; a consequential decision requires explicit confirmation or correction.

   Carry settled decisions, assumptions, and constraints into later lenses. If a later lens invalidates an earlier decision, reopen it as `provisional`, re-render the affected design, and obtain confirmation again.

   Before synthesis, count-check the agenda: every selected lens must have been surfaced separately and resolved as `accepted`, `delegated`, `skipped`, `provisional`, or `open` from an actual human response. Agenda approval is not a lens response. Do not silently backfill an untouched lens.

7. **Checkpoint when useful, THEN reload for the next lens.** Remain chat-only by default. Before a pause, after a consequential lens, or when the workshop grows long, offer to save the current design record. If no checkpoint is needed, announce the next lens and the decision it will resolve, then reload this method and load that lens's file. Resume from the conversation while it remains available. Durable resume across an unexpected exit, lost conversation, or agent change is guaranteed only by a saved checkpoint or export; never claim otherwise.

## Cross-lens synthesis and design handoff

After all selected lenses, reconcile conflicts and dependencies. Surface decisions that constrain several areas. Include named components and responsibilities, at least one important end-to-end flow, key risks, and validation signals. Add other views only when they clarify the design.

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

This skill is doing its job only when, in the human's view: applicability was inferred and explained; each selected lens was a genuine, paced discussion; diagrams and option sets were visible before approval; components and responsibilities were co-designed before finished alternatives; every decision has honest status and provenance; and the final handoff preserves the design without turning it into a plan.

## Lineage

This is a standalone adaptation with deliberate subtraction and handoff-oriented reframing of the latest [Specrew Design Workshop](https://github.com/alonf/specrew) method and its nine-lens knowledge pack. It retains the facilitation and co-design method while removing Specrew lifecycle, gates, Spec Kit integration, mandatory persistence, product-domain and code-implementation phases, and repository governance. It neither requires Specrew nor represents an official Specrew distribution.
