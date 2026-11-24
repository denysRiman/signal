# ADR-007 — Evaluation strategy

## Context

Agentic quality claims need reproducible evidence beyond prompt inspection.

## Decision

Use deterministic tests and AgentCore Evaluations to compare baseline and
tool-using agent on identical fixtures.

## Alternatives considered

Manual subjective review only or model evaluation replacing safeguards.

## Trade-offs

Fixtures and evaluations take effort and incur controlled evaluation cost.

## Consequences

Evaluation results are documented evidence, not publication authorization.
