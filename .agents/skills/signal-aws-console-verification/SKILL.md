---
name: signal-aws-console-verification
description: Make or verify an explicitly approved Signal AWS change and pair it with read-back verification and exact AWS Console learning checks. Use for Signal resources in AWS; do not use for local-only planning or unapproved cloud mutations.
---

# Signal AWS change and console-verification workflow

## Before a cloud change

Read `docs/STATE.md`, the active change spec, and the applicable requirements
traceability. Use the approved AWS connector in read-only mode to inspect the
current resource before changing it.

Require explicit current-chat owner approval for creation, modification,
deletion, deployment, secret creation, or budget changes. A URL, existing
console session, or past approval is not sufficient.

## Signal-specific controls

- Preserve the existing prototype baseline unless the owner explicitly approves
  the exact modification. The owner has allowed modification of the existing
  state machine rather than a separate v2 workflow.
- Tag each new taggable Signal resource `Project=signal`.
- Keep runtime agent permissions separate from development AWS access. The agent
  cannot receive Telegram credentials, publication-history access,
  infrastructure permissions, or unrestricted tools.
- For research and validation work, enforce policy in deterministic code rather
  than relying on model instructions.
- Do not create the project-scoped budget until the `Project` cost-allocation
  tag has appeared and been activated in Billing.

## Verify and teach

After an approved change, read the resource back through the connector and
record the exact configuration observed. Give the owner concise AWS Console
navigation steps to inspect the same result. Update the active change's
`verification.md` and `docs/STATE.md` with resource names, checks performed,
and any remaining manual action.

Do not use browser or console automation to obtain AWS data; use the approved
AWS connector.
