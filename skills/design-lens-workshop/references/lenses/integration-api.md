# Integration And API Lens

## Lens ID

`integration-api`

## Purpose

Make service boundaries, contracts, protocols, versioning, compatibility, and message semantics explicit before implementation couples systems accidentally.

## Applicability Signals

- The feature calls or exposes an API, webhook, event, queue, file contract, plugin interface, generated SDK, CLI wrapper, or external dependency.
- Multiple clients, hosts, languages, or versions must interoperate.
- Backward compatibility, rate limits, retries, ordering, idempotency, or schema evolution matters.

## Design Decision Points

- What is the integration style: REST, GraphQL, gRPC, OData, RPC, events, pub/sub, queue, webhook, file contract, SDK, or direct library call?
- What owns the contract and how is it versioned?
- Are operations synchronous, asynchronous, streaming, or eventually consistent?
- Which requests are safe, idempotent, cacheable, or replayable?
- How are authentication, authorization, throttling, and API management handled?
- How are compatibility and schema evolution validated?

## Workshop Conduct

- **Shared view:** for a `full` or `medium` pass, expect a service-interaction or contract sequence unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it clarifies producers, consumers, timing, failure, or ownership. On a text-only host use console ASCII.
- **Facilitate, do not dictate:** sequence the key interaction with the human. Agree the contract shape, error envelope, timing, retry semantics, and ownership before selecting tooling.
- Walk timeout, duplicate, partial-failure, and compatibility behavior where relevant.
- Re-render material changes and iterate until the human confirms, delegates, skips, or leaves the decision open.
- Return the decision to the main workshop method before loading the next lens.

## Question Bank

- Who are the producers and consumers?
- Is the contract data or message oriented, or object and class oriented?
- What fields are required, optional, versioned, or deprecated?
- What happens on timeout, duplicate delivery, partial failure, or retry?
- Does the client need exactly-once behavior, at-least-once behavior, ordering, or compensation?
- Should clients generate from a contract, or should the server provide an SDK?
- What rate limits, auth scopes, and error shapes are needed?
- How does the system bridge old and new protocols during migration?

## Trade-off Dimensions

- Direct call versus explicit remote or asynchronous boundary.
- Request/response versus streaming, event, queue, webhook, or file exchange.
- Informal contract versus schema-first, generated, or managed contract.
- Synchronous consistency versus eventual consistency and compensation.
- Minimal compatibility promise versus explicit versioning and migration support.

## Design Handoff

- Record protocol, contract owner, versioning, authentication, retry, timeout, and compatibility decisions.
- Preserve the key sequence and error behavior when they materially shape the design.
- Identify producer and consumer validation signals and representative fixtures.
- Keep unresolved external limits, ownership, or compatibility assumptions explicit.

## Validation Signals

- Contract validation can cover both producer and consumer expectations.
- Review can verify idempotency and retry claims against actual behavior.
- Compatibility claims can be exercised against old and new shapes where relevant.
