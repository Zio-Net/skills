# UI And UX Lens

## Lens ID

`ui-ux`

## Purpose

Make user experience a first-class design input. When a feature changes an interface, discuss the workflow, references, interaction model, data movement, states, and accessibility before implementation choices harden.

## Applicability Signals

- The feature has screens, forms, dashboards, tables, CLI prompts, messages, onboarding, visual evidence, or user-visible workflow.
- The user mentions Figma, screenshots, images, mockups, existing UI behavior, themes, pagination, sorting, grouping, streaming, or async interaction.
- The feature changes how users make decisions or recover from errors.
- The interface must work across device sizes, locales, themes, or assistive technologies.

## Design Decision Points

- What source of UX truth exists: Figma, screenshots, design system, existing product patterns, prototype, or text-only intent?
- What are the primary user journeys and interruption or recovery paths?
- Should data operations be client-side, server-side, streamed, or event-driven?
- How are loading, empty, error, disabled, offline, partial-success, and conflict states shown?
- Which UI state belongs on the client, server, URL, cache, or durable store?
- What accessibility, localization, RTL, theming, or regulatory constraints apply?

## Workshop Conduct

- **Shared view:** use an inline layout, wireframe, navigation flow, or state sketch when it clarifies the interaction. On a text-only host use console ASCII.
- **Facilitate, do not dictate:** start from the available UX source of truth, co-design the main journey and recovery states, and show the view before asking for approval.
- Ask the human to change labels, placement, flow, ownership, or states. Re-render material changes until the layout and interaction are confirmed, delegated, skipped, or left open.
- Do not let a UI decision collapse into backend structure without showing the user-visible consequence.
- Return the decision to the main workshop method before loading the next lens.

## Question Bank

- Is there a Figma file, screenshot, image, or existing screen to match?
- What user task should the first screen optimize?
- Which controls need paging, filtering, grouping, or sorting?
- Should paging, sorting, filtering, and grouping be client-side or server-side, and why?
- Does the UI need polling, push, WebSocket, streaming, or manual refresh?
- What should happen when a server update times out or returns unknown state?
- What can be optimistic, and what requires pessimistic locking or confirmation?
- What are the empty, loading, validation, partial-success, and error states?
- Are there accessibility, keyboard, screen-reader, RTL, localization, or theme requirements?
- Does the UI need offline mode or resynchronization?

## Trade-off Dimensions

- Existing product pattern versus a new interaction model.
- Client-side versus server-side data operations and state ownership.
- Manual refresh versus polling, push, streaming, or offline synchronization.
- Optimistic interaction versus confirmation and stronger consistency.
- Minimal state coverage versus explicit journey, state, and recovery models.

## Design Handoff

- Record the UX source of truth and any missing design evidence.
- Decide client/server ownership for paging, sorting, filtering, grouping, and synchronization.
- Preserve the agreed primary journey, important states, and recovery behavior.
- Identify visual, accessibility, responsive, or interaction signals that would validate the design.

## Validation Signals

- Screens or prompt flows can be inspected, not only compiled.
- Review can verify that text fits, important states are reachable, and recovery paths are visible.
- If no design artifact exists, the handoff names the substitute decision source.
