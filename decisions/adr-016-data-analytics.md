# ADR-016: Data & analytics platform — managed lakehouse, notebooks are code, catalog mandatory

**Status:** accepted (baseline) · **Category:** data & analytics

## Context
Analytics estates rot differently: hand-edited production notebooks,
uncatalogued datasets, always-on clusters, and dashboards fed by silently
stale numbers. None of the OLTP rules (ADR-006) address this shape of work.

## Options considered
- **DIY Spark on VMs** — you inherit the platform team you did not hire.
  Rejected.
- **Warehouse-only** — viable for pure BI; falls over when data
  engineering, ML features and streaming arrive. Viable subset, not the
  default.
- **Managed lakehouse with a mandatory catalog.**

## Decision
Analytics runs on the chosen cloud's managed lakehouse (Databricks /
Fabric / BigQuery — mapping in baseline/data-analytics.md). **Notebooks
are code**: repo-versioned, gate-reviewed, pipeline-deployed; production
workspaces are deploy targets, never editors. **The catalog is mandatory
from day 0** — owner, classification, lineage per dataset. Layered
refinement with data tests gating each promotion; freshness SLOs on every
consumption dataset, rendering "stale since" rather than silently old
numbers. Jobs run under per-job workload identities; clusters
auto-terminate and interactive compute sleeps; spend joins the
unit-economics review.

## Consequences
Easier: the same six questions grade the data estate — attribution,
cost, quality, compounding all carry over; AI training/eval data inherits
governance for free. Harder: analysts lose live-editing of production
notebooks — give them fast sandboxes with representative data, or they
will route around the rule.
