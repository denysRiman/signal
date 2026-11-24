# ADR-001 — Agent responsibilities

## Context

Signal combines editorial model judgement with safety-critical delivery rules.

## Decision

The agent selects, researches, summarizes, teaches, and replaces rejected
stories. Deterministic services ingest, deduplicate, validate, orchestrate,
publish, and record history. The agent never publishes or writes history.

## Alternatives considered

Letting the agent own publication; or removing editorial autonomy entirely.

## Trade-offs

This adds orchestration steps but keeps irreversible actions outside the model.

## Consequences

Workflow and IAM boundaries must make direct publication/history access
impossible for the harness.
