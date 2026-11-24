# 002-requirements-capture — complete architecture handoff

## Status

Completed locally; commit authorized by the owner. The owner pushes manually.

## Goal

Make the complete approved architecture usable by a future agent that has no
access to prior chat context.

## Requirements

- Preserve every normative architecture requirement in a checked-in canonical
  brief.
- Add one ADR document per approved ADR.
- Add requirement-to-ADR/change traceability.
- Mark the short architecture document as an orientation, not the full source.
- Record the owner's no-agent-push Git rule in durable guidance.

## Non-goals

- No AWS implementation, deployment, credential storage, or remote Git push.
