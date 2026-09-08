# ADR-003: Identity — groups + PIM for humans, federation for workloads, agents act as the person

**Status:** accepted (baseline) · **Category:** identity

## Context
Four kinds of actors touch the system: employees, services/pipelines, AI
agents, and (possibly) customers. Each needs a different identity design;
mixing them is how audits fail.

## Options considered
- **Direct role assignments to individuals** — decays instantly on team
  change; unauditable at scale. Rejected.
- **Shared service accounts with long-lived secrets** — the classic breach
  vector; secrets leak into pipelines and repos. Rejected.
- **Groups + PIM, federated workload identity, per-person agent tokens.**

## Decision
Humans: every grant goes to a directory group, never a person; privileged
roles via just-in-time elevation (PIM or the cloud's equivalent) with
time-boxed activation; joiner–mover–leaver handled by group
membership; quarterly access review with evidence. Workloads: one federated
(OIDC) identity per service per environment — no client secrets in
pipelines, ever. AI agents: **no standing identity of their own** — they act
as the driving person via short-lived signed tokens, unattributed writes are
refused at the tool boundary, and production is queue-only for agents (a
named human lands the change). Customers, if any: federated SSO; store the
minimum; tenant-wide directory permissions only with explicit admin consent.

## Consequences
Easier: every action in every log has a human name on it — AI usage becomes
measurable per person for free. Harder: token minting/refresh is
infrastructure you must build early — it lives in the tool server
(utility #2), part of the enforcement spine built first.
