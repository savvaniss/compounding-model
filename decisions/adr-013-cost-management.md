# ADR-013: Cost management — tagged, budgeted, owned, and unit-economic

**Status:** accepted (baseline) · **Category:** operations

## Context
Cloud + AI spend grows silently: idle non-prod clusters, orphaned volumes,
registry bloat, model calls nobody attributed. Cost review without
structure is archaeology.

## Options considered
- **Monthly invoice review** — too late, too aggregated. Rejected.
- **Tags + budgets + unit economics, reviewed on cadence.**

## Decision
The required-tags policy (ADR-001) makes every resource attributable;
environment cost folds managed-cluster (`MC_*`) groups into their
environment. Budgets with alerts **and a named owner** per isolation boundary.
**Non-prod sleeps**: dev/stage compute stops on schedule (nights/weekends)
via pipeline, wake on demand. Registry and storage get retention policies
before they get big (an untagged image purge on a 1TB registry is a
migraine; a retention policy on day one is a checkbox). AI spend joins the
same review via the cost ledger (ADR-010), expressed as **unit economics**
— cost per generation, per active user, per delivered change — because
absolute spend without a denominator drives bad decisions in both
directions.

## Consequences
Easier: the monthly review is a dashboard read with decisions attached
(model mix, quota, sleep windows). Harder: sleep schedules break the
"always-on dev" habit — pair them with a one-click wake.
