# ADR-009: Observability — OpenTelemetry, actor-attributed, honest about failure

**Status:** accepted (baseline) · **Category:** operations

## Context
The monitoring map (../operations/monitoring-map.md) needs seven altitudes of
signal. They all come from instrumentation decisions made here.

## Options considered
- **Vendor SDKs per concern** — lock-in and N config surfaces. Rejected.
- **Logs-only, add metrics later** — "later" arrives during an incident.
  Rejected.
- **OpenTelemetry end-to-end into the platform's managed backend.**

## Decision
OTel SDKs in every service, one collector per cluster, exported to the
cloud-native backend (logs/metrics/traces in one place). Three
non-negotiable properties: (1) **logs name the actor** — the human or the
human-behind-the-agent, joined to ADR-003 identities, which is what makes
usage and audit queries possible; (2) **every LLM call is telemetered**
with model, tokens and the person (feeding the cost ledger, ADR-010);
(3) **failed ≠ empty** — a broken collector or query must render as "can't
tell", never as a zero that looks like good news. SLOs per user-facing
service with error budgets; alerts page on budget burn, not on raw CPU.

## Consequences
Easier: one instrumentation standard; the AI-usage KPI and the security
audit ride the same pipeline. Harder: cardinality costs money — the actor
label is worth it, per-request UUIDs as metric labels are not.
