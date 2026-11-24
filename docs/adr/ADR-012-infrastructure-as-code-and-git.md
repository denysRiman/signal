# ADR-012 — Infrastructure as code and Git

## Context

The project must be reproducible, reviewable, and appropriate for a public
portfolio without leaking private configuration.

## Decision

Use one Python CDK repository for infrastructure, code, prompts, tests,
evaluations, ADRs, and deployment/rollback guidance. The owner pushes Git
commits personally.

## Alternatives considered

Console-only configuration, split repositories, or early CI/CD.

## Trade-offs

Documentation and change discipline add upfront work.

## Consequences

No credentials, private logs, account-sensitive material, generated artifacts,
or unapproved CI/CD belongs in Git.
