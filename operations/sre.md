# Operations & reliability

## SLOs and error budgets
Each user-facing service: an SLI (availability, latency) with an SLO target
and an error budget. Budget exhausted → feature releases pause, reliability
work takes the slot. This converts "is it stable enough?" from an argument
into arithmetic.

## Incident management
Severities defined in advance (S1 user-facing outage → S4 cosmetic) with
response expectations per level · one incident channel, one commander ·
timeline kept as it happens, not reconstructed · **every S1/S2 becomes a
lessons-learned entry with evidence** (the same library the AI assistant
serves back to the team). Blameless, but specific: rules that come out are
checkable, not aspirational.

## On-call & operability
A service is not "done" until: health probes, structured logs naming the
actor, a runbook (who restarts it, how you know it worked), dashboards, and
an owner. The deploy pipeline verifies rollout convergence and endpoint
health — a green pipeline with a stuck rollout is the false-positive that
bites hardest.

## DR & continuity
RTO/RPO per data tier, written down · backups automated **and restore
exercised** on a schedule with evidence (an untested backup is a hope) ·
environment rebuild-from-code proven (that is what pipeline-only IaC buys) ·
secrets recovery path documented.

## Capacity & cost operations
Quarterly capacity review against measured usage — requests/limits and HPA
targets are ONE decision (targets are percentages *of requests*; changing one
without the other multiplies replicas) · non-prod sleeps on schedule · the
AI quota table reviewed with the cost review, because model TPM is capacity
too.
