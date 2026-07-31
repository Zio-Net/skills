# Requirements And NFR Lens

## Lens ID

`requirements-nfr`

## Purpose

Turn vague needs into measurable requirements and design-driving constraints. Activate this lens when qualities such as performance, usability, security, availability, maintainability, compatibility, compliance, or cost should shape the design.

## Applicability Signals

- The request contains broad quality words such as fast, secure, scalable, reliable, usable, compatible, auditable, or cheap.
- The feature changes workflow, user experience, public behavior, release policy, or operational expectations.
- The scope has hidden stakeholders or disfavored users.
- Requirements are implied by examples, screenshots, prototypes, or existing system behavior.

## Design Decision Points

- Which NFRs are design drivers for this slice?
- Which constraints are mandatory rather than preferences?
- Which requirements need a measurable threshold?
- Which requirements are unknown enough to require clarification or research?
- Which acceptance criteria prove the quality, not only the happy path?

## Workshop Conduct

- **Shared view:** use a quality-attribute priority table or comparison matrix when it makes priorities and thresholds easier to review.
- **Facilitate, do not dictate:** agree the priority order of the relevant quality attributes and their measurable thresholds. Distinguish a design driver from a generic good practice.
- Keep statements tagged as `Known`, `Assumed`, or `Open`; do not turn a vague quality adjective into a fabricated target.
- Iterate until the human confirms, delegates, skips, or explicitly leaves a threshold open.
- Return the decision to the main workshop method before loading the next lens.

## Question Bank

- Who is the user, customer, operator, and disfavored user?
- What user pain is this feature solving?
- Which NFRs are binding for this feature?
- What does success look like in measurable terms?
- What should the system refuse to do?
- What ambiguity would cause rework if left unresolved?
- What prototype, sketch, example, or existing behavior should be treated as requirements evidence?

## Trade-off Dimensions

- A narrow set of slice-specific NFRs versus a broader quality profile.
- Qualitative intent versus measurable thresholds and evidence.
- User-visible quality versus operator, security, compliance, and maintainability concerns.
- Immediate acceptance signals versus longer-running production indicators.

## Design Handoff

- Separate design-driving NFRs from relevant but non-driving qualities.
- Convert vague quality statements into measurable signals where practical.
- Identify tests, inspections, smoke checks, or other evidence needed for the selected quality goals.
- Preserve unresolved thresholds as open questions rather than invented acceptance criteria.

## Validation Signals

- Review evidence covers NFR claims with execution, inspection, or documented human acceptance.
- No major quality claim is accepted only because an artifact exists.
- Each design-driving quality can be traced to a design decision or explicit risk acceptance.
