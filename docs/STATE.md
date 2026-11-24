# Current project state

> Read this document before starting work in a new chat. Update it whenever a
> task changes local files, AWS resources, architecture decisions, or blockers.

## Handoff snapshot

- Last updated: 2026-11-24
- Repository status: local `main` tracks `origin/main`. Requirements capture is
  committed locally and awaits the owner's manual push. Agents may create a
  locally authorized commit but must never run `git push`.
- Active implementation status: complete requirements capture is finished. The
  next proposed AWS implementation change remains
  `001-publication-history`, which has not been approved or deployed.
- Chat workflow: the owner intends to use one new chat per small step. Treat
  this file, `PLANS.md`, and the active change files as the durable handoff.
- Canonical requirements: read
  `docs/requirements/approved-architecture-brief.md` and
  `docs/requirements/traceability.md`, not only the short architecture summary.
- Repository-local skills: use `.agents/skills/signal-change-workflow/` for a
  scoped change, `.agents/skills/signal-requirements-traceability/` for design
  decisions, and `.agents/skills/signal-aws-console-verification/` for an
  approved AWS action with UI learning checks.

## Approved project direction

- Use Python AWS CDK, AWS Lambda, Standard Step Functions, AgentCore Managed
  Harness, AgentCore Gateway with exactly three MCP research tools, DynamoDB,
  EventBridge Scheduler, Secrets Manager, CloudWatch, and AgentCore
  Evaluations.
- Modify the existing state machine when implementation begins; do not create a
  second v2 workflow solely for migration.
- Preserve the current implementation as an evaluation baseline.
- Apply `Project=signal` to new taggable resources.

## Deployed baseline observed in AWS

- Standard state machine: `signal-news-digest` in `eu-central-1`.
- Lambda functions: `signal-rss-ingest` and `signal-digest-validate`.
- AgentCore Managed Harness: ready; the workflow currently overrides its
  default model with Nova 2 Lite.
- No EventBridge schedule, AgentCore Gateway, Signal DynamoDB table, or Signal
  Secrets Manager secret existed when inspected.
- Step Functions logging and tracing were disabled. The two Lambda log groups
  existed without an explicit retention period.

## AWS changes already made

- Added `Project=signal` to the existing state machine, both Lambdas, and the
  AgentCore harness.
- No state-machine definition, Lambda code, execution role, or execution was
  changed.
- The `Project` cost-allocation tag had not yet propagated into Billing at the
  last check. No project-scoped budget exists yet. Do not create the budget
  until the tag appears and is activated in Billing.
- The alert recipient is private configuration. Ask the owner for it in a new
  chat rather than recording it in Git.

## Known baseline findings

- The prototype had 11 inspected executions: 6 succeeded and 5 failed.
- Failures included unknown article references, conflicting duplicate JSON
  properties, and attempts while the AgentCore harness was not ready.
- The current harness permits broad tool categories but has no tools configured.
  The target implementation must narrow this boundary before adding tools.

## Current blockers and required manual inputs

- Wait for the `Project` allocation tag to appear, then activate it and create
  the monthly project budget.
- Tavily credentials, Telegram bot credentials, and an eventual GitHub remote
  remain owner-provided setup; never store their values in the repository.
