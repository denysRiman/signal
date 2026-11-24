# Signal

Signal is an AWS-native personal news curator for an English-learning software
engineer in Austria. It is being rebuilt as a reproducible, safety-bounded
agentic system using AWS CDK and Python.

## Repository status

This repository is in its bootstrap phase. The AWS prototype exists separately;
its current state and the approved target design are recorded in
[`docs/STATE.md`](docs/STATE.md) and
[`docs/architecture/approved-target.md`](docs/architecture/approved-target.md).

## How work is organized

- `specs/changes/` contains one small, reviewable change at a time.
- `docs/STATE.md` is the required handoff document for a new chat or agent.
- `AGENTS.md` holds concise, durable repository instructions.
- Application, infrastructure, tests, and evaluations will be added only when
  their approved change is implemented.

No credentials, private logs, account identifiers, or generated CDK artifacts
belong in this repository.
