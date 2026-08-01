# DevOps And Operations Lens

## Lens ID

`devops-operations`

## Purpose

Expose deployment, environment, CI/CD, configuration, secrets, access, rollback, and operations choices as architecture, not afterthoughts.

## Applicability Signals

- The feature changes installation, packaging, release, hosting, CI, deployment, infrastructure, configuration, secrets, environment setup, roles, or runtime operations.
- The feature must run on multiple operating systems, clouds, hosts, tenants, or environments.
- The change introduces operational risk, rollback risk, or manual validation.

## Design Decision Points

- What is the hosting model: local tool, web server, VM, container, orchestrator, serverless function, serverless container, hybrid, or embedded?
- What infrastructure is code-owned, manually configured, or external?
- Which environments must be equivalent, and where may they differ?
- How are secrets, configuration hierarchy, and dynamic configuration handled?
- What CI/CD stages, rollout strategy, rollback path, and operational checks are required?
- Which CI lane belongs to this project's actual forge and existing delivery system?
- What users, roles, service identities, and permissions are needed?

## Workshop Conduct

- **Inspect before proposing:** read the repository's existing provider, pipeline, deployment, and infrastructure configuration. Do not default a non-GitHub project to GitHub Actions or replace an established delivery model silently.
- **Shared view:** for a `full` or `medium` pass, expect a deployment topology or promotion path unless it would add no clarity for this feature; state the reason if omitted. For a `light` pass, use one only when it clarifies environments, nodes, identities, pipelines, rollback, or manual boundaries. On a text-only host use console ASCII.
- **Parallel scan mode:** inspect the existing delivery model and identify topology, configuration, identity, environment, rollout, rollback, and evidence concerns. Do not select a final operational topology or policy.
- **Workshop mode:** agree those choices with the human when a new operational boundary or policy decision remains open.
- State capability limits honestly. Do not describe a syntax check, dry run, or proposed control as runtime proof.
- Re-render material changes before review. In parallel scan mode, return the finding to the coordinator without a separate human response.
- Return the lens result to the main workshop method. Do not load another lens from this reference.

## Question Bank

Use these questions to guide analysis. Do not present them as an interview list; ask only an unresolved question selected by the main workshop method.

- What install or deployment command should a normal user or operator run?
- What dependencies are passive and automated versus explicit prerequisites?
- What environments must the design account for: dev, CI, staging, beta, stable, customer tenant, macOS, Linux, Windows, WSL, or VM?
- What secrets or credentials are needed, and where are they stored?
- What should be represented in infrastructure as code, and what stays manual or external?
- How do we roll forward, roll back, disable, or recover the feature?
- Which CI lane is authoritative, and which checks are only syntax or proxy checks?
- Who needs access, and what is the least-privilege role?
- How will operators know deployment or runtime failed?

## Trade-off Dimensions

- Manual installation or deployment versus scripted or declarative automation.
- Existing hosting and pipeline patterns versus a new operational boundary.
- Environment-specific configuration versus stronger parity.
- Simple rollout and rollback notes versus staged delivery and automated recovery.
- Local validation versus forge-native CI and target-environment runtime proof.
- Manual infrastructure versus infrastructure as code.

## Design Handoff

- Name the user-facing install or deploy path and hidden prerequisites.
- Record hosting, environments, configuration hierarchy, secret handling, identities, promotion, and rollback.
- Distinguish authoritative CI and runtime validation from proxy checks.
- State which operational steps remain manual or externally owned.
- Preserve open capability or access questions without turning them into governance policy.

## Validation Signals

- Install or deploy evidence can run in the target environment.
- CI evidence covers the operating systems, hosts, or environments claimed by the design.
- Secrets are not embedded in scripts, logs, generated artifacts, or client-visible configuration.
- Rollback or disable behavior has an observable success signal.
