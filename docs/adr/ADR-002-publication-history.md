# ADR-002 — Publication history

## Context

The digest needs exact repeat prevention and recent semantic comparison without
becoming a permanent article archive.

## Decision

Use on-demand DynamoDB `signal-news-history`, keyed by canonical-URL SHA-256,
with explicit application retention windows and TTL cleanup.

## Alternatives considered

Permanent article storage, a vector database, or in-memory-only deduplication.

## Trade-offs

DynamoDB needs a deliberate recent-history access pattern, but is managed and
fits the personal-scale workload.

## Consequences

Only successful delivery records are written; semantic comparison reads at most
the seven-day window.
