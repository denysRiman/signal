# Requirements traceability

This index converts the approved brief into implementation checks. `TBD` means
no change has been approved yet; it is not permission to omit the requirement.

| ID | Requirement group | Normative source | Primary ADR | Planned change |
| --- | --- | --- | --- | --- |
| R-01 | Objective, baseline preservation, and explicit approvals | §§1–3 | — | All changes |
| R-02 | Deterministic Step Functions workflow | §4 | ADR-001 | TBD workflow change |
| R-03 | Agent/deterministic responsibility boundary | §5 | ADR-001 | TBD agent integration |
| R-04 | 30-day exact and seven-day semantic history | §6 | ADR-002 | `001-publication-history` |
| R-05 | Bounded three-tool research autonomy | §7 | ADR-003 | TBD research gateway |
| R-06 | Managed AWS service choices and configurable model | §8 | ADR-004 | TBD infrastructure foundation |
| R-07 | Least privilege, source policy, and secret boundaries | §9 | ADR-005 | All runtime changes |
| R-08 | Failure behaviour and observability | §10 | ADR-006 | TBD observability/publish changes |
| R-09 | Baseline-versus-agent evaluation | §11 | ADR-007 | TBD evaluations |
| R-10 | Tavily wrapping and provider verification | §12 | ADR-008 | TBD research gateway |
| R-11 | 24-hour isolated execution context | §13 | ADR-009 | TBD execution-context change |
| R-12 | Structural, policy, and semantic validation | §14 | ADR-010 | TBD validation change |
| R-13 | One bounded reflection/refinement pass | §15 | ADR-010 | TBD workflow refinement |
| R-14 | 72-hour temporal correctness | §16 | ADR-011 | RSS, tools, and validator changes |
| R-15 | CDK, Git, tags, documentation, and no CI/CD yet | §17–18 | ADR-012 | `000-bootstrap`, all infrastructure changes |
| R-16 | Evidence-resolved digest output contract | §19 | ADR-001, ADR-010 | TBD curate/validate change |
| R-17 | Demonstrable acceptance criteria | §20 | All | Verification for every change |

## Completion rule

Before marking a change completed, link its verification evidence to every row
it claims to satisfy. No requirement is satisfied merely because a resource
exists or a model prompt requests the desired behaviour.
