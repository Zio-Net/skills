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
- **Facilitate, do not dictate:** trace one important success path and one failure mode with the human. Agree the signals, error handling, ownership, and recovery behavior.
- Distinguish user error, dependency failure, configuration failure, code defect, and platform outage where the response differs.
- Re-render material changes and iterate until the human confirms, delegates, skips, or leaves the decision open.
- Return the decision to the main workshop method before loading the next lens.

## Question Bank

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
