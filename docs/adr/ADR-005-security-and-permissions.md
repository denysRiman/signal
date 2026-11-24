# ADR-005 — Security and permissions

## Context

External articles and research are untrusted, while publication and credentials
are sensitive.

## Decision

Enforce source/redirect/freshness/reference policy in code; use least-privilege
IAM; isolate secrets; and prohibit harness access to Telegram, history, and
infrastructure.

## Alternatives considered

Prompt-only safety controls or broad execution roles.

## Trade-offs

More IAM policies and validation code are required.

## Consequences

Every runtime role and tool contract needs an explicit permission boundary.
