# 000-bootstrap — repository and agent handoff foundation

## Status

Verified locally; awaiting owner review before an initial Git commit.

## Goal

Create a minimal, durable repository structure that lets independent future
chats understand project requirements, deployed state, and the currently active
step without relying on previous conversation context.

## Requirements

- Initialize Git on `main` without committing or connecting GitHub.
- Add concise repository instructions and a secret-safe `.gitignore`.
- Preserve approved architecture requirements and the observed deployed
  baseline in checked-in documentation.
- Use change-local specification, plan, task, and verification files.
- Create the intended code, infrastructure, test, evaluation, and documentation
  directories without implementing application behaviour.

## Non-goals

- No CDK bootstrap, deployment, AWS resource creation, code migration,
  dependency installation, commit, or GitHub push.
