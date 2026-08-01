# Security And Compliance Lens

## Lens ID

`security-compliance`

## Purpose

Surface identity, authorization, data protection, privacy, audit, and regulatory constraints early enough to shape alternatives rather than patching them on after implementation.

## Applicability Signals

- The feature handles users, roles, permissions, credentials, tokens, secrets, PII, customer data, financial data, healthcare data, audit trails, public input, plugins, shell execution, or dependency installation.
- The feature affects trust boundaries or executes code from external sources.
- The domain has regulation, policy, retention, residency, accessibility, or auditability requirements.

## Design Decision Points

- Who authenticates, and where is identity established?
- What authorization model applies: roles, claims, scopes, ownership, tenant boundary, policy, or local trust?
- What data is sensitive, and how is it protected in transit, at rest, logs, telemetry, backups, and exports?
- What actions need audit records?
- What threat surfaces are introduced by scripts, plugins, APIs, file writes, shell commands, generated code, or third-party dependencies?

## Workshop Conduct

- **Shared view:** for a `full` or `medium` pass, expect a trust-boundary or attack-surface diagram unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it makes actors, data, privileges, or controls easier to inspect. On a text-only host use console ASCII.
- **Parallel scan mode:** inspect trust boundaries, actors, data, privileges, policy, and attack surface far enough to identify risks and missing decisions. Do not select final controls or accept risk.
- **Workshop mode:** agree those decisions with the human when a new trust boundary, compliance obligation, or risk acceptance remains open.
- Include denial and failure behavior, not only the happy path. Determine what happens when identity, policy, or secret retrieval fails; ask only if repository evidence and established policy do not decide it.
- Re-render material changes before review. In parallel scan mode, return the finding to the coordinator without a separate human response.
- Return the lens result to the main workshop method. Do not load another lens from this reference.

## Question Bank

Use these questions to guide analysis. Do not present them as an interview list; ask only an unresolved question selected by the main workshop method.

- Who is allowed to do this, and who must be prevented?
- What is the least-privilege role or permission set?
- Does the feature read, write, display, log, or transmit sensitive data?
- What secrets exist, where are they stored, and how are they rotated?
- What audit trail is required for user, operator, or system actions?
- What input must be validated or rejected?
- Does the feature need tenant isolation, data residency, retention, deletion, or consent handling?
- What is the failure mode if authentication, policy, or secret retrieval fails?

## Trade-off Dimensions

- Existing trusted boundary versus a new identity or trust boundary.
- Roles versus claims, scopes, ownership, tenant, or policy-based authorization.
- Broad access versus least privilege and field- or action-level controls.
- Minimal audit evidence versus a structured audit and compliance model.
- Existing controls versus explicit threat modeling and boundary-specific mitigations.

## Design Handoff

- Name trust boundaries, identities, authorization rules, data classification, and secret handling.
- Record threat surfaces, denial behavior, and mitigations.
- Identify security, privacy, audit, or compliance signals required to validate the design.
- Keep unresolved regulatory or policy questions explicit rather than assuming compliance.

## Validation Signals

- Tests or review evidence can exercise denial paths, not only success paths.
- Logs and artifacts can be checked for sensitive-data leakage.
- Shell, plugin, install, or external-input surfaces demonstrate confinement and failure-safe behavior.
