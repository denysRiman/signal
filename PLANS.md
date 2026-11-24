# Signal execution-plan index

This file is the entry point for spec-driven work. It is intentionally short;
the executable detail belongs with the relevant change.

## New-chat protocol

For every new chat or agent:

1. Read `AGENTS.md`.
2. Read `docs/STATE.md`.
3. Read the active change's `spec.md`, `plan.md`, `tasks.md`, and
   `verification.md`.
4. Work on that change only. Create a new numbered change rather than silently
   expanding the scope.
5. Before ending, update the active change and `docs/STATE.md` with verified
   facts, changed resources, commands, and blockers.

## Change lifecycle

`draft` → `approved` → `in-progress` → `verified` → `completed`

Only the project owner can mark a change `approved`. A change is not
`completed` until its verification evidence is recorded.

## Active work

| Change | Status | Outcome |
| --- | --- | --- |
| [`000-bootstrap`](specs/changes/000-bootstrap/) | completed | Establish repository, durable guidance, and handoff documents. |
| [`002-requirements-capture`](specs/changes/002-requirements-capture/) | completed | Preserve the complete approved architecture and ADR traceability for future chats. |
| [`001-publication-history`](specs/changes/001-publication-history/) | draft | Create and verify the publication-history foundation before wiring it into the workflow. |
