# Security and privacy

## Mission

Assess the attack and privacy surface introduced or changed by the scope.
Produce concrete, evidence-backed abuse or disclosure scenarios rather than a
generic security checklist.

## Select when / skip when

Select for authentication, authorization, tenancy, untrusted input, public
endpoints, secrets, sensitive data, logging, file/network access, or changed
trust boundaries. Skip only when no credible security or privacy surface
changes.

## Evidence to inspect

- Actors, assets, entry points, trust boundaries, and data-flow diagrams or
  architecture docs.
- Authentication and authorization enforcement at every relevant boundary.
- Input parsing, validation, output encoding, rate/size limits, and abuse
  controls.
- Tenant scoping, secret handling, encryption assumptions, retention, logging,
  telemetry, and error responses.
- Security tests and relevant configured scanners as supporting evidence.

## Review sequence

1. Name the changed assets, actors, entry points, and trust transitions.
2. Trace identity and authorization from request origin to the protected
   operation; distinguish authentication from permission.
3. Follow attacker-controlled data through validation, storage, execution,
   rendering, logging, and outbound calls.
4. Check tenant and subject scoping on reads, writes, caches, and events.
5. Trace sensitive data and secrets through persistence, logs, errors,
   telemetry, and deletion.
6. Consider practical abuse: replay, enumeration, confused deputy, excessive
   resource use, bypass, and unintended disclosure.
7. Verify that proposed defenses exist at the enforcing boundary.

## Light / standard / deep

- `light`: inspect the changed trust boundary, direct authorization, and obvious
  sensitive-data flow.
- `standard`: trace all changed entry points and sinks, tenant scoping, abuse
  paths, logs, and tests.
- `deep`: build a compact threat model, inspect transitive consumers and mixed
  trust contexts, and validate controls with targeted evidence.

## Finding threshold

Report only when a plausible actor can reach a concrete unauthorized,
integrity, availability, privacy, or disclosure outcome. Name the path,
preconditions, affected asset, impact, and missing or misplaced control.

## Do not report

- Vague advice such as "add validation" or "consider encryption."
- Unchanged repository-wide security debt unrelated to this scope.
- Scanner output without confirming reachability and impact.

## Challenge signals

Reopen architecture when a control belongs at a different boundary or data
ownership creates leakage. Reopen purpose when required functionality itself
creates an unaccepted trust or privacy decision. Move this lens earlier when
the mitigation changes contracts or user flow.

Use the shared worker result contract from `SKILL.md`.
