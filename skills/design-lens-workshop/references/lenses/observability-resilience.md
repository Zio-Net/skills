# Observability And Resilience Lens

## Lens ID

`observability-resilience`

## Purpose

Ensure the design can explain what happened, detect failure, fail safely, and recover. Tie runtime truth to logs, metrics, traces, health checks, alerts, and error-handling decisions.

## Applicability Signals

- The feature runs in production, CI, install scripts, background jobs, distributed systems, user workflows, or long-lived operations.
- Failures may be partial, intermittent, remote, retried, asynchronous, or hard for a user to diagnose.
- The feature makes claims about reliability, availability, performance, operability, self-healing, or supportability.

## Design Decision Points

- What must be logged, measured, traced, or reported to understand behavior?
- What correlation or context should flow through logs or events?
- What health checks, readiness checks, or validation commands are needed?
- What errors are expected, and which are exceptional?
- What retry, timeout, idempotency, circuit, compensation, or recovery pattern applies?
- What is the cost of observability, and what should not be logged?

## Workshop Conduct

- **Shared view:** use a request trace, failure-mode flow, or signal table when it makes diagnosis and recovery visible. On a text-only host use console ASCII.
- **Parallel scan mode:** trace important success and failure paths far enough to identify signal, ownership, degradation, recovery, and operability gaps. Do not select the final telemetry or recovery design.
- **Workshop mode:** agree those decisions with the human when they introduce new promises, ownership, or risk acceptance.
- Distinguish user error, dependency failure, configuration failure, code defect, and platform outage where the response differs.
- Re-render material changes before review. In parallel scan mode, return the finding to the coordinator without a separate human response.
- Return the lens result to the main workshop method. Do not load another lens from this reference.

## Question Bank

Use these questions to guide analysis. Do not present them as an interview list; ask only an unresolved question selected by the main workshop method.

- How will a user or operator know the feature worked?
- How will they know it failed, and what should they do next?
- What telemetry distinguishes user error, dependency failure, configuration failure, code defect, and platform outage?
- Which operations are safe to retry?
- What state must be idempotent to avoid duplicates or corruption?
- What is the timeout behavior and user-facing message?
- What SLI, SLO, or acceptance signal matters for this feature?
- What evidence should review require before accepting runtime claims?

## Trade-off Dimensions

- Clear errors and local logs versus structured correlated telemetry.
- Passive diagnosis versus health, readiness, alerts, and dashboards.
- Immediate failure versus retry, timeout, circuit, compensation, or recovery automation.
- Broad telemetry versus cost, privacy, and signal quality.
- Form or static evidence versus target-runtime proof and failure exercise.

## Design Handoff

- Record important failure modes, error handling, timeout, retry, idempotency, and recovery decisions.
- Name the signals and correlation context needed to diagnose them.
- Separate static or syntax checks from runtime validation.
- Define the evidence that would support each operational or resilience claim.

## Validation Signals

- Failure paths can be exercised in tests, controlled experiments, or manual smoke checks.
- Review can trace a failure from symptom to diagnostic evidence and recovery action.
- Runtime claims are not replaced with form-only evidence.
