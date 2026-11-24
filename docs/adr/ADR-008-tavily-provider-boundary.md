# ADR-008 — Tavily provider boundary

## Context

External search and extraction are needed but must not become unrestricted agent
network access.

## Decision

Use Tavily Search and Extract only behind the approved MCP tools and revalidate
all provider output locally.

## Alternatives considered

Direct Tavily tool access or accepting snippets as complete evidence.

## Trade-offs

Wrapper code and provider verification are required.

## Consequences

Authentication, price, quotas, and extraction limits are a deployment blocker
until verified.
