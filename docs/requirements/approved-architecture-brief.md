# Signal v2 — approved architecture brief

> **Status:** approved target requirements; not an implementation claim.
>
> This is the durable, normalized transcription of the project owner's
> architecture brief. It takes precedence over summaries. A later explicit
> owner instruction may supersede an item; record the change in `docs/STATE.md`
> and the applicable ADR rather than silently changing the design.

## 1. Roles and working boundaries

The project owner is the software architect and project owner. The
implementation agent is the AWS infrastructure and implementation engineer.
Approved architectural decisions are not changed silently. If an implementation
limitation makes a decision infeasible, document the limitation, alternatives,
and trade-offs, then obtain owner approval.

The original architecture exercise was planning-only. The owner may separately
approve small implementation changes in later chats. This repository records
those approvals in its change specs and `docs/STATE.md`.

## 2. Objective

Signal is an automated personal news curator for an English-learning software
engineer living in Austria. It delivers a daily Telegram digest containing
ideally three fresh, relevant, evidence-grounded stories.

It must both provide a useful personal service and demonstrate practical AWS
native agentic development, infrastructure as code, architecture, and AI
evaluation. Avoid accidental complexity while retaining the explicitly selected
agentic capabilities for learning.

## 3. Existing baseline

The AWS Region is `eu-central-1`. Preserve the following implementation as an
evaluation baseline and never destroy it during migration:

- Standard Step Functions workflow `signal-news-digest`.
- Lambda functions `signal-rss-ingest` and `signal-digest-validate`.
- AgentCore Managed Harness and its Amazon Nova 2 Lite invocation
  configuration.
- The related IAM roles, CloudWatch logs, and AgentCore observability.

The baseline fetches BBC World, BBC Technology, ORF News, and ORF Science RSS
feeds concurrently; normalizes article metadata; computes stable SHA-256 URL
fingerprints; assigns short execution-local article IDs; curates through
AgentCore; validates model JSON; safely repairs harmless formatting and
identical duplicate JSON properties; and resolves original URLs through trusted
article identifiers.

## 4. Target architecture

EventBridge Scheduler invokes a Standard Step Functions workflow with this
deterministic sequence:

1. `FetchNews`
2. `FilterSeenNews`
3. `InitializeExecutionContext`
4. `CurateNews` using AgentCore Managed Harness
5. `ValidateDigest`
6. conditional reflection and refinement
7. `PublishTelegram`
8. `RecordPublished`

The harness accesses exactly three MCP research tools through AgentCore Gateway:
`searchArticles`, `getArticle`, and `findRelatedStories`. The tools use Tavily
but enforce deterministic source, freshness, and execution-budget policy.

DynamoDB holds publication history and temporary execution context. Secrets
Manager holds credentials. CloudWatch provides infrastructure and operational
observability. AgentCore Evaluations measures agent quality. Step Functions
remains the deterministic business orchestrator.

## 5. ADR-001 — responsibilities

The agent is responsible for editorial selection, optional autonomous discovery
of additional news, optional investigation of related reporting,
evidence-grounded summarization, English-learning content, and replacement of
rejected stories during refinement.

The deterministic system is responsible for RSS ingestion, exact article
deduplication, source and freshness enforcement, evidence registration,
validation, refinement orchestration, publication, and delivery history. The
agent must not directly publish or modify publication history.

## 6. ADR-002 — publication history

Create on-demand DynamoDB table `signal-news-history` with string partition key
`fingerprint`, the stable SHA-256 hash of canonical URL. Store `headline`,
`summary`, `url`, `sentAt`, and numeric TTL timestamp `expiresAt`.

Apply a 30-day exact fingerprint retention window and seven-day semantic
duplicate-event comparison window. Enforce windows in application code because
DynamoDB TTL deletion is asynchronous. Store only successfully delivered
stories; do not build a permanent archive of ingested or researched news. Short
IDs such as `A01` are execution-local references, not persistent identities.

## 7. ADR-003 — bounded research autonomy

The agent may discover new stories beyond initial RSS candidates. Research is
optional and used only where it improves editorial selection or evidence
quality.

### `searchArticles(query)`

- Search approved publisher domains only.
- Admit only articles published in the prior 72 hours.
- Return at most five results per call.
- Return registered article references with trusted metadata.
- Reject already published exact fingerprints.

### `getArticle(articleRef)`

- Accept registered references only.
- Admit approved publishers only.
- Never permit arbitrary URL fetching.
- Respect publisher access restrictions and applicable content-use policy; do
  not bypass paywalls.

### `findRelatedStories(articleRef)`

- Constrain the investigation using registered subject, publication date, and
  available event context.
- Prevent unrelated or stale reporting from being mixed into current events.
- Shared implementation with `searchArticles` is allowed.

### Global limits

- Target three valid stories; permit a smaller digest only after refinement is
  exhausted and at least one story remains independently valid.
- At most six research tool calls per execution across all agent invocations.
- At most one refinement attempt.
- Target workflow duration of 120 seconds.
- Explicit model-token limits and bounded external requests/costs.
- Enforce tool budget outside the model; harness iterations are not a tool-call
  budget.
- Do not expose a publication-history lookup tool to the agent. Publication
  history feedback comes from validation only.

## 8. ADR-004 — managed agent infrastructure

Use AgentCore Managed Harness, AgentCore Gateway, Standard Step Functions,
Python Lambda, DynamoDB, EventBridge Scheduler, Secrets Manager, CloudWatch,
and AgentCore Evaluations. Do not build a custom agent runtime or loop and do
not introduce a multi-agent framework.

Retain Amazon Nova 2 Lite as the initial evaluation baseline, but make model
selection configurable for later benchmark comparisons. Verify regional
availability and API compatibility through official AWS documentation and AWS
MCP before deployment.

## 9. ADR-005 — security and permissions

Configure approved publisher domains through Lambda environment variables;
initially approve BBC and ORF. Research implementation must enforce domain
allowlisting, final redirect validation, publication-date validation, registered
references, SSRF/arbitrary-fetch protection, input validation, and treatment of
external content as untrusted.

Use least-privilege IAM. The runtime agent may invoke approved read-only
research tools only. It must not access Telegram credentials, publish, modify
history, manage infrastructure, or invoke unapproved tools. Tools may write
execution-scoped evidence and budget records as an internal detail, but the
agent must not receive general-purpose DynamoDB access.

Store Tavily and Telegram credentials in Secrets Manager. Never expose them in
prompts, logs, tool responses, or Git. Development AWS MCP authority is
separate from runtime Gateway authority.

## 10. ADR-006 — failures and observability

If one RSS source is unavailable, continue with the rest. If the research
provider is unavailable, continue with eligible existing evidence. Apply
bounded retries with exponential backoff to transient infrastructure failures.

Safely repair harmless model formatting errors; otherwise reflect and refine.
Skip publication silently when no eligible news exists. Technical failures send
a short Telegram failure notification. If Telegram is unavailable, use
CloudWatch alarms and a separate notification channel. Never record an
unsuccessful Telegram delivery as published.

Document the remaining duplicate-delivery risk where Telegram succeeds but the
subsequent publication-history write fails. Monitor tool calls and latency,
model token consumption, external requests, execution duration, validation
failures/reasons, refinement rate, delivery outcome, and estimated costs.

## 11. ADR-007 — evaluation

Use AgentCore Evaluations alongside deterministic automated tests. Compare the
simple baseline curator and the tool-using agent on identical fixtures. Evaluate
editorial relevance, diversity, factual grounding, temporal correctness,
source-policy compliance, duplicate-event detection, research usefulness,
reflection/refinement usefulness, token use, tool use, latency, and cost.

Keep results reproducible and suitable for public documentation. Evaluation
scores never replace deterministic publication safeguards.

## 12. ADR-008 — Tavily

Use Tavily Search and Extract, wrapped behind the three approved research tools.
Do not provide unrestricted direct Tavily access to the agent. Enforce source
and freshness rules independently of provider filters. Search snippets alone
are insufficient evidence for unsupported factual claims. Verify Tavily
capabilities, authentication, quota, extraction limitations, and price before
enabling scheduled use.

## 13. ADR-009 — temporary execution context

Create on-demand table `signal-execution-context` with 24-hour TTL. Maintain
execution ID, initial RSS articles, research-discovered articles, evidence
references, shared research-call budget, and necessary execution metadata.

Use atomic operations to enforce the six-call shared budget. Research tools and
validator use the same context; no evidence may cross execution boundaries.
Enforce expiry in code. Do not add Redis, a vector database, or permanent
article storage.

## 14. ADR-010 — validation and LLM-as-a-Judge

Structural Python validation checks JSON structure, required fields, types,
valid article references, duplicate JSON properties, duplicate selected
references, vocabulary, and conversation starter. Preserve repair only for
identical duplicate properties; reject conflicting duplicates.

Policy validation checks approved publishers, registered evidence, publication
dates, previously published fingerprints, and execution boundaries.

One bounded, separate Bedrock LLM-as-a-Judge call per validation pass compares
selected stories with each other and the previous seven days of history. It
returns structured rejected references and specific reasons, distinguishes
repeated coverage from a genuinely new development, has no tools, and does not
attempt comprehensive fact checking. Detailed factual-grounding assessment
belongs in AgentCore Evaluations. A technical semantic-validation failure must
not silently count as complete validation.

## 15. Reflection and refinement

Initial generation gets eligible RSS candidates, may research optionally, and
requests three valid distinct stories. Validation identifies rejected stories,
reasons, and unfilled slots. Refinement preserves accepted stories unchanged,
feeds back rejected references/reasons, asks only for replacements, permits the
remaining research budget, and regenerates learning content if necessary.

Allow exactly one refinement, then revalidate. After exhaustion publish three
when valid; publish a smaller digest only if at least one independent story and
consistent learning content remain; never publish rejected content; skip when no
valid digest remains; distinguish insufficient news from technical failure.

## 16. ADR-011 — temporal correctness

All initial and researched evidence must satisfy the hard 72-hour publication
cutoff in the MVP. Do not retrieve older historical articles as added evidence.
Distinguish publication time from reported-event time. Do not join events merely
because they involve the same company, person, or topic. Preserve uncertainty
and attribution.

## 17. ADR-012 — infrastructure as code and GitHub

Use Python AWS CDK. Keep infrastructure, Lambda code, agent prompts, tool
schemas, docs, tests, evaluation configuration, deployment instructions, and
rollback instructions in one Git repository. Use reproducible deployments,
environment-specific configuration outside code, least privilege, explicit
resource names and tags, and safe migration from the prototype.

Never commit credentials, Tavily keys, Telegram tokens, private execution logs,
account-sensitive configuration, or generated deployment artifacts. Do not add
CI/CD unless separately approved. The owner maintains the GitHub remote and
pushes commits personally.

The repository root is the Signal project; this intentionally avoids an
unnecessary nested `signal/` directory. Component folders are `infrastructure/`,
`lambdas/`, `agent/`, `evaluations/`, `tests/`, and `docs/`.

## 18. Public documentation

The README ultimately explains purpose, business case, full architecture,
managed-agent design, MCP tools, bounded autonomy, reflection, LLM-as-a-Judge,
deterministic/probabilistic boundaries, security, persistence, failures,
deployment, testing, evaluation methodology/results, costs, limitations, and
future work. Include Mermaid architecture and sequence diagrams.

Create one ADR per approved decision with context, decision, alternatives,
trade-offs, and consequences. Do not invent performance, evaluation, or
production-readiness claims.

## 19. Digest output contract

Extend structured digest output to support multiple registered evidence
references per story. Resolve source metadata and URLs deterministically from
execution-scoped registered evidence; never trust model-generated URLs.

Retain concise English headline, factual summary, relevance explanation,
original approved sources, exactly two advanced English vocabulary expressions
with definitions, one conversation starter, and B2–C1 English. Treat excerpts
as excerpts, distinguish reported facts from allegations/unverified claims and
possible implications, and do not claim access to article content that was not
provided.

## 20. Acceptance criteria

The implementation must demonstrate all of the following:

- exact deduplication within and across executions;
- semantic duplicate-event detection;
- autonomous research-tool selection;
- approved-source and redirect enforcement;
- freshness enforcement;
- execution-isolated evidence registration;
- six-call research maximum and one-refinement maximum;
- replacement of rejected stories while preserving accepted stories;
- recovery from harmless malformed JSON;
- rejection of conflicting or unknown article references;
- no rejected content in Telegram;
- history only after successful delivery;
- technical-failure notifications;
- reproducible infrastructure deployment;
- automated deterministic tests; and
- AgentCore evaluation evidence comparing baseline and agentic implementation.
