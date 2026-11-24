# ADR-011 — Temporal correctness

## Context

News relevance depends on evidence age, not merely shared topics.

## Decision

Apply a hard 72-hour publication cutoff to all evidence and retain uncertainty
and event-time distinctions.

## Alternatives considered

Provider-only freshness filters or retrieval of older contextual articles.

## Trade-offs

Some otherwise useful background articles are intentionally excluded.

## Consequences

Every ingest, research, and validation path must parse and enforce dates.
