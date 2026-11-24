# ADR-004 — Managed AWS agent infrastructure

## Context

The project intentionally demonstrates managed AWS agent capabilities.

## Decision

Use AgentCore Managed Harness and Gateway, Standard Step Functions, Python
Lambda, DynamoDB, Scheduler, Secrets Manager, CloudWatch, and Evaluations.

## Alternatives considered

Custom agent runtime, custom loop, or a multi-agent framework.

## Trade-offs

Managed service constraints shape integration design but reduce infrastructure
to operate.

## Consequences

Regional/API compatibility must be verified and the model remains configurable.
