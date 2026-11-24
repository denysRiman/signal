---
name: signal-change-workflow
description: Run one scoped Signal repository change from specification through handoff. Use when planning, implementing, reviewing, or closing any independently reviewable Signal outcome; do not use for a one-line answer unrelated to project files.
---

# Signal scoped-change workflow

## Start with the durable project state

Read, in order:

1. `AGENTS.md`
2. `docs/STATE.md`
3. `PLANS.md`
4. The relevant `specs/changes/<number>-<slug>/` files

Treat `docs/requirements/approved-architecture-brief.md` as the normative
architecture source. Read `docs/requirements/traceability.md` and relevant
ADRs whenever a change can affect an approved design decision.

## Keep the work scoped

- Work on one independently reviewable outcome. Do not silently add adjacent
  infrastructure or feature work.
- A new outcome needs `spec.md`, `plan.md`, `tasks.md`, and `verification.md`
  before implementation. Owner approval is required before a draft change
  becomes implementation work.
- Keep acceptance criteria observable. Record actual tests, AWS reads, and
  known gaps in `verification.md`; do not replace evidence with a claim.

## Respect authorization and repository safety

- Current-chat approval controls external writes, deployment, deletion, and
  commits. Earlier approval does not automatically apply to a new outcome.
- Never store secrets, account-sensitive data, alert recipients, private logs,
  or generated artifacts in Git.
- The owner personally pushes. Never run `git push`, alter remote branches, or
  configure global Git identity.
- When a commit is authorized, stage explicit logical paths, inspect staged
  changes, and make a focused commit. Do not use `git add .`.

## Finish for the next chat

Before ending, update `tasks.md` and `docs/STATE.md` with completed work,
resource changes, verification evidence, blockers, and exactly one recommended
next step. Ensure `PLANS.md` reflects the change lifecycle.
