# ADR-011: APIs & integration — contract-first, queues for long work, idempotent

**Status:** accepted (baseline) · **Category:** architecture

## Context
Service boundaries are where teams and agents integrate; ambiguity here
multiplies into every consumer.

## Options considered
- **Code-first, docs generated if remembered** — the contract drifts from
  reality. Rejected.
- **Contract-first OpenAPI + async for anything slow.**

## Decision
Every service API is an OpenAPI contract reviewed in the PR like code;
clients are generated, not hand-written. Anything slower than an
interactive request (ingestion, generation, batch) runs **async via a
queue** with a job status endpoint — no 10-minute HTTP calls. Handlers are
**idempotent** (retries are a fact of queues and of AI agents re-driving
tools); events carry a correlation ID that flows into the traces (ADR-009).
Versioning: additive changes are free, breaking changes get a new version
and a deprecation window — never a silent change under the same path.

## Consequences
Easier: agents and humans consume the same generated clients; retries are
safe by construction. Harder: contract review adds friction exactly where
friction pays for itself.
