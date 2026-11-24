# ADR-010 — Validation and refinement

## Context

Model output can be malformed, policy-invalid, or semantically repetitive.

## Decision

Layer deterministic structural and policy validation with a bounded,
tool-less LLM-as-a-Judge; permit one deterministic workflow-controlled
refinement pass.

## Alternatives considered

Prompt-only output validation, unlimited agent self-correction, or LLM-only
fact checking.

## Trade-offs

Validation adds latency and a small controlled model cost.

## Consequences

Preserve accepted stories, reject unsafe output, and never silently treat a
semantic-validator outage as success.
