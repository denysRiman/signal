# Spec-driven development

Every independently reviewable change lives in `changes/<number>-<slug>/`.

Required files:

- `spec.md` — goal, requirements, non-goals, and acceptance criteria.
- `plan.md` — design, affected components, rollout, and rollback.
- `tasks.md` — small tasks with current status.
- `verification.md` — commands, tests, AWS-console checks, and evidence.

Start implementation only after the owner approves the change. Keep proposed
future work out of an active change unless the owner explicitly expands it.
