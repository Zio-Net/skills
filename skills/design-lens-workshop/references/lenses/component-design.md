# Component Design Lens

## Lens ID

`component-design`

## Purpose

Keep implementation structure aligned with responsibilities, volatility, coupling, cohesion, extension needs, and testability.

## Applicability Signals

- The feature introduces or modifies shared modules, helpers, generators, services, layers, engines, accessors, SDKs, plugins, or schema mapping.
- The change risks duplicated logic, hidden coupling, or unclear ownership.
- Different callers need the same capability with different policies.

## Design Decision Points

- What responsibilities belong together, and what should stay separate?
- Which dependencies point inward versus outward?
- Is the right abstraction a function, helper, module, service, plugin, engine, accessor, data file, generated artifact, or SDK?
- Where should schemas be decoupled from internal models?
- What extension mechanism fits: inheritance, composition, configuration, events, aspect, plugin, or generated code?
- What public seam makes the behavior testable without exposing internals?

## Workshop Conduct

- **Shared view:** for a `full` or `medium` pass, expect a component map with dependency direction unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it clarifies ownership or coupling. On a text-only host use console ASCII.
- **Render the full component map before asking:** show every component by name with a one-line responsibility, grouped by the chosen decomposition vocabulary. Never present only a count or a reference to an unseen map.
- In parallel scan mode, identify responsibility, ownership, dependency, and test-seam concerns plus viable boundary forks; do not propose the final component map. In workshop mode, co-design the map and re-render every material change.
- Justify an abstraction with real variation, ownership, complexity, or reuse. Prefer a local implementation when those signals are absent.
- Return the lens result to the main workshop method. Do not load another lens from this reference.

## Question Bank

Use these questions to guide analysis. Do not present them as an interview list; ask only an unresolved question selected by the main workshop method.

- What is the unit of responsibility?
- Which part changes most often?
- Which part should be stable and depended on by others?
- What dependency would make future change expensive?
- Is the abstraction created because there is real variation?
- How will tests exercise the component without relying on implementation internals?
- Are we sharing schema/contract or sharing classes/binaries?
- Does this need DI, factory, registry, plugin lookup, or simple construction?

## Trade-off Dimensions

- Local implementation versus a shared abstraction.
- Direct construction versus dependency injection, factory, registry, or plugin lookup.
- Shared internal types versus explicit schemas and DTO mapping.
- Composition and configuration versus inheritance, events, aspects, plugins, or generated code.
- Minimal seams versus stronger dependency inversion and extension boundaries.

Compare only alternatives justified by actual variation or ownership boundaries.

## Design Handoff

- Record each component's responsibility, owner, dependencies, extension points, and test seams.
- Justify every new abstraction by variation, complexity, ownership, or reuse.
- Identify schema or DTO mapping and compatibility where external contracts exist.
- Preserve the agreed component map and key flow when they are material to the design.

## Validation Signals

- Tests can exercise behavior through the public seam.
- Review can verify that coupling did not increase across ownership boundaries.
- Generated or configured behavior has parity or drift signals when applicable.
