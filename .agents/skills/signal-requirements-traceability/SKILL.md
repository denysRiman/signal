---
name: signal-requirements-traceability
description: Check a proposed or implemented Signal design against the approved architecture, ADRs, and traceability matrix. Use when a change affects workflow, research, validation, persistence, security, cost, evaluation, or the digest contract; do not use for unrelated repository housekeeping.
---

# Signal requirements traceability

## Authoritative sources

Read these before proposing an architecture-affecting change:

1. `docs/requirements/approved-architecture-brief.md`
2. `docs/requirements/traceability.md`
3. Relevant files under `docs/adr/`
4. `docs/STATE.md` for current deployed facts and explicit owner overrides

The approved brief records requirements; it does not prove they are deployed.
Do not infer implementation status from an ADR or a planned change.

## Assessment method

- Identify the affected `R-*` rows and ADRs before choosing implementation.
- Preserve hard boundaries: deterministic publication control, registered
  evidence, 72-hour freshness, bounded research, execution isolation,
  least-privilege runtime access, and no secret exposure.
- If the change conflicts with an approved decision, state the conflict,
  feasible alternatives, and trade-offs. Wait for owner approval rather than
  silently redesigning it.
- Add the affected requirement IDs and acceptance evidence to the active
  change's spec and verification record.

## Completion standard

A requirement is complete only when the change has recorded evidence for its
observable acceptance criteria. A prompt, an AWS resource, or a passing happy
path alone does not establish compliance.
