# Architecture Core Lens

## Lens ID

`architecture-core`

## Purpose

Prevent silent structural decisions. Architecture is where costly-to-change choices about structure, boundaries, constraints, and volatility become visible before implementation locks one path.

## Applicability Signals

- The feature introduces a new subsystem, lifecycle stage, command surface, or persistent artifact.
- The implementation can be shaped as inline logic, helper/module, service, workflow, extension point, or generated data.
- A decision would be expensive to reverse after code lands.
- Multiple stakeholders care about different qualities: users, operators, developers, security, UI/UX, product, or management.

## Design Decision Points

- Which design method or decomposition style governs the structure: for example bounded contexts, volatility-based decomposition, modular monolith, microservices, or layers?
- What are the major building blocks and their responsibilities?
- Which areas are volatile and should be isolated behind data, interfaces, or extension points?
- Which constraints are binding, and which are preferences?
- What is deliberately out of scope for this iteration?
- Which direction best balances simplicity, reversibility, and future cost?

## Workshop Conduct

- **Shared view:** for a `full` or `medium` pass, expect a component, service, context, or flow diagram unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it exposes coupling or boundaries. Render it inline so the human can see it; on a text-only host use console ASCII.
- **Facilitate, do not dictate:** raise the decision points as a discussion. Co-decide the decomposition style, co-design the component map, and walk at least one flow before presenting completed architecture options.
- Invite the human to rename, split, merge, or reassign boundaries. Re-render changes and iterate until they confirm, delegate, skip, or leave a decision open.
- Return the decision to the main workshop method before loading the next lens.

## Question Bank

- What structural decision will be hardest to change later?
- Which part should be data-driven rather than prompt-driven or code-driven?
- What stakeholders are affected by this decision?
- Which assumptions should be recorded as constraints?
- What would make the recommended direction flip to another one?
- What needs a diagram because prose alone hides the coupling?
- What is the smallest slice that still proves the architecture?

## Trade-off Dimensions

- Local logic versus an explicit module, service, workflow, or extension boundary.
- Code-driven behavior versus data, configuration, schemas, or generated artifacts.
- Immediate simplicity versus reversibility and known future cost.
- Informal boundaries versus explicit interfaces, validators, migration rules, and diagrams.

Compare only dimensions that create a real fork for this feature.

## Design Handoff

- Name the chosen structure and meaningful rejected alternatives.
- Record reversibility cost, accepted constraints, and deferred architectural debt.
- Map major building blocks to the outcomes and validation signals they support.
- Include a diagram for non-trivial component or flow boundaries when it improves reviewability.

## Validation Signals

- The eventual implementation can be checked against the chosen structure, not only the desired outcome.
- A reviewer can point to the decision and explain why the design exists.
- Deferred alternatives have named conditions that would justify reopening them.
