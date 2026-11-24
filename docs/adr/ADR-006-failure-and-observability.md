# ADR-006 — Failure handling and observability

## Context

The digest must fail safely while retaining useful degradation behaviour.

## Decision

Continue with healthy sources/evidence, bound retries, distinguish invalid
content from technical failure, and monitor the complete agent/delivery path.

## Alternatives considered

Failing the whole digest on any unavailable source or silently accepting failed
validation.

## Trade-offs

More state-machine branches and metrics are required.

## Consequences

Document the Telegram-success/history-write-failure duplicate-delivery risk.
