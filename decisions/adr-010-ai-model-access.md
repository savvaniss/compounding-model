# ADR-010: AI model access — one gateway, isolated quota, ledger, eval-gated swaps

**Status:** accepted (baseline) · **Category:** AI platform

## Context
Model access sprawls fast: keys in many hands, quotas shared by accident,
spend attributed to nobody, models swapped on vibes. Every one of these
failure modes is expensive and all are preventable with the same four
decisions.

## Options considered
- **Each service holds its own provider keys** — no rotation story, no
  aggregate view. Rejected.
- **Central gateway (single AI endpoint resource per environment) + platform
  discipline.**

## Decision
One AI gateway resource per environment; services get model access only
through it, by workload identity where supported. **Quota isolation**: the
AI that *builds* the product (gate, agents, judge) never shares TPM quota
with the AI *in* the product — quotas are regional and finite, and a noisy
build agent must not starve a customer feature (nor vice versa). **Cost
ledger**: every call writes an attribution row (person/service, model,
tokens); prices are effective-dated per model *as billed* — a model whose
billed name has no price row is an alert, not a silent zero. **Eval-gated
change**: no model added, swapped or upgraded in product paths without a run
of the evaluation harness (../baseline/ai-standards.md) — golden tasks, judge
scores, cost per candidate — attached to the PR.

## Consequences
Easier: "what does AI cost us, per person and per feature" is a query;
model migrations become evidence-based. Harder: the gateway is a single
point of failure — give it the same SLO treatment as any tier-1 dependency,
and expect transient errors under throttle (retry policy must treat
provider-side throttling-shaped errors as retryable).
