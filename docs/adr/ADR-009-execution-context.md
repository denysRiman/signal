# ADR-009 — Temporary execution context

## Context

References, evidence, and tool budget must remain isolated across executions.

## Decision

Use a 24-hour on-demand DynamoDB execution-context table with atomic budget
operations and application-level expiry checks.

## Alternatives considered

Redis, a vector store, permanent article storage, or passing untrusted context
only through prompts.

## Trade-offs

The table requires careful key and capability design.

## Consequences

Tools and validator must identify the same execution without cross-execution
access.
