# 003-repository-skills — local workflow skills

## Status

Completed locally; the owner pushes manually.

## Goal

Make the established Signal workflow discoverable to future Codex chats through
repository-local skills without relying on prior conversation context.

## Requirements

- Create skills under `.agents/skills/` so Codex discovers them for this
  repository in future chats.
- Separate scoped-change workflow, architecture traceability, and approved AWS
  console-verification work.
- Preserve the owner-only push rule and current approval boundaries.
- Keep skill instructions concise and link to maintained repository documents
  rather than duplicating the complete architecture.

## Non-goals

- Do not install global skills, modify Codex global configuration, add scripts,
  deploy AWS resources, or push Git commits.
