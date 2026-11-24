# Signal repository guidance

## Mission

Signal is an AWS-native, daily personal news curator. It must preserve the
complete approved requirements in `docs/requirements/approved-architecture-brief.md`
and keep deterministic safety controls separate from agentic editorial work.

## Start every task

1. Read `docs/STATE.md` for the deployed baseline and current handoff state.
2. Read `PLANS.md` and the relevant change under `specs/changes/`.
3. Confirm the task's scope, acceptance criteria, and verification steps before
   editing code or AWS resources.
4. Use the applicable repository skill from `.agents/skills/` for change
   workflow, requirements traceability, or an approved AWS console change.

## Working agreement

- Use one numbered change directory per independently reviewable outcome.
- A change has `spec.md`, `plan.md`, `tasks.md`, and `verification.md`.
- Update `tasks.md` while working and update `docs/STATE.md` before handoff.
- Keep `AGENTS.md` short. Put design detail in `docs/` and change-specific
  requirements in `specs/`.
- Preserve the existing AWS prototype as a baseline unless the owner explicitly
  authorizes a change. The owner has authorized updating the existing state
  machine rather than creating a second workflow.
- Do not deploy, commit, delete resources, or reveal credentials unless the
  current chat explicitly authorizes that exact action.
- Never run `git push`. The owner reviews and pushes commits personally. This
  applies even when a commit has been explicitly authorized.

## Safety

- Never put credentials, account-specific secrets, private logs, or alert
  recipients in Git.
- The runtime agent must never receive Telegram credentials, publication-history
  access, infrastructure permissions, or unrestricted network tools.
- Implement source allowlists, freshness checks, redirect validation, and the
  shared research-call budget outside the model.

## Verification

- Add focused tests with each behaviour change.
- Prefer deterministic validation over claims based only on a model response.
- Record commands run, AWS resources changed, and unresolved risks in the
  change's `verification.md` and in `docs/STATE.md`.
