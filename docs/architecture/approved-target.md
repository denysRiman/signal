# Approved target architecture

This document is the durable requirements record derived from the project
owner's approved architecture brief. It records requirements, not a claim that
they are implemented.

## Objective

Deliver a daily Telegram digest of up to three fresh, relevant,
evidence-grounded news stories for an English-learning software engineer in
Austria. The project is both a useful personal service and a public
demonstration of AWS-native agentic engineering.

## Required workflow

```text
EventBridge Scheduler
  → FetchNews
  → FilterSeenNews
  → InitializeExecutionContext
  → CurateNews (AgentCore Managed Harness)
  → ValidateDigest
  → [optional single reflection/refinement]
  → PublishTelegram
  → RecordPublished
```

Step Functions owns deterministic orchestration. The agent owns editorial
selection, optional bounded research, evidence-grounded writing, English
learning content, and replacing rejected stories. The agent must never publish
or modify publication history directly.

## Non-negotiable limits

- At most three selected stories and at least one before a smaller digest may be
  published.
- A hard 72-hour evidence freshness cutoff.
- At most six research tool calls per execution, atomically enforced outside the
  model and shared by initial and refinement calls.
- At most one refinement attempt.
- 120-second target execution limit with explicit model-token limits.
- Only BBC and ORF are initially approved publisher families.
- No unrestricted browsing, arbitrary URL retrieval, paywall bypassing, Redis,
  vector database, permanent article archive, custom agent runtime, or
  multi-agent framework.

## Managed services

- AgentCore Managed Harness and AgentCore Gateway
- Standard Step Functions and Python Lambdas
- DynamoDB on-demand tables: `signal-news-history` and
  `signal-execution-context`
- EventBridge Scheduler, Secrets Manager, CloudWatch, and AgentCore Evaluations
- Amazon Nova 2 Lite is the initial evaluation baseline; model selection must
  remain configurable.

## Research tools

The harness gets exactly three read-only MCP tools through AgentCore Gateway:

1. `searchArticles(query)` — maximum five results; approved domains, 72-hour
   cutoff, and publication-history exclusion enforced by the implementation.
2. `getArticle(articleRef)` — only registered, approved evidence; never an
   arbitrary URL.
3. `findRelatedStories(articleRef)` — bounded investigation tied to the
   registered article's subject and date.

All external material is untrusted input. Redirect destinations, domain,
publication date, references, and execution boundaries are validated
deterministically.

## Persistence

### Publication history

`signal-news-history` stores only successfully delivered stories. Its primary
identity is the canonical-URL SHA-256 fingerprint. It supports 30-day exact
deduplication and a seven-day semantic event comparison window. DynamoDB TTL is
cleanup only; application logic enforces retention windows.

### Execution context

`signal-execution-context` has a 24-hour TTL and isolates each Step Functions
execution's registered RSS articles, research evidence, article references,
tool-call budget, and metadata. The research tools and validator must use the
same execution context. Short article references are execution-local only.

## Validation and publication

Validation has three layers:

1. Deterministic structural validation, including safe repair only for harmless
   malformed JSON and identical duplicate properties.
2. Deterministic policy validation for approved sources, evidence references,
   dates, publication history, and execution boundaries.
3. One bounded LLM-as-a-Judge semantic pass for duplicate-event detection.

The judge has no tools and does not replace deterministic safeguards. Rejected
content never reaches Telegram. History is recorded only after confirmed
delivery; the residual risk of a successful Telegram send followed by a failed
history write must be documented.

## Security and observability

- Secrets remain in Secrets Manager and never appear in prompts, logs, tool
  responses, source control, or documentation.
- Use least privilege. The agent cannot access Telegram credentials, history,
  infrastructure, or unapproved tools.
- Monitor calls and latency, token use, provider requests, duration, validation
  reasons, refinement outcome, delivery outcome, and estimated cost.
- RSS and research-provider failure may degrade gracefully; technical failures
  require operational notification and must not be reported as valid content.

## Documentation and evaluation

Keep source, CDK, prompts, schemas, tests, evaluations, ADRs, architecture
diagrams, deployment guidance, and rollback guidance in this repository. Compare
the baseline and tool-using implementation using identical fixtures. Do not make
unverified performance or quality claims.
