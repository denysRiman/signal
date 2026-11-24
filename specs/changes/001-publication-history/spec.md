# 001-publication-history — future change draft

## Status

Draft. Do not implement or deploy until the owner explicitly approves it.

## Goal

Create the DynamoDB publication-history foundation for 30-day exact URL
deduplication and seven-day semantic duplicate-event comparison.

## Intended acceptance criteria

- On-demand table named `signal-news-history`.
- Canonical URL SHA-256 fingerprint as the primary key.
- Explicit TTL attribute `expiresAt` and application-level window enforcement.
- Access pattern for the recent seven-day semantic comparison window.
- `Project=signal` tag and least-privilege access.
- CDK definition, deterministic tests, deployment verification, and rollback
  instructions.

## Non-goals

- Do not wire the table into the existing state machine in this change unless
  the owner expands the scope.
