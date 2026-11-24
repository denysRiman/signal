# ADR-003 — Bounded research autonomy

## Context

Research improves curation but unbounded tools create cost, safety, and latency
risk.

## Decision

Expose only `searchArticles`, `getArticle`, and `findRelatedStories`; enforce
sources, 72-hour freshness, registered evidence, and a six-call atomic budget
outside the model.

## Alternatives considered

Unrestricted search/browsing or no research at all.

## Trade-offs

The implementation is more constrained but behaviour is auditable and bounded.

## Consequences

Harness iterations cannot be treated as a call budget; tools need shared
execution context.
