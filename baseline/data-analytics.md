# Data & analytics — the same rails, at data scale

A data platform does not get a different operating model; it gets the same
invariants in data-shaped form. The failure mode this file prevents:
production notebooks edited live by hand, pipelines nobody gates, clusters
nobody turns off, and numbers that are silently three days old.

## Notebooks are code — no exceptions
A notebook is source: versioned in the repo, reviewed through the AI gate,
deployed to the workspace by the pipeline. **Nobody edits a production
notebook in place** — the workspace's production folders are deploy targets,
not editors. Exploration happens in personal sandboxes on sandbox data.

## Platform & governance (ADR-016)
Managed lakehouse on the chosen cloud; a **data catalog is mandatory from
day 0** — every dataset registered with owner, classification and lineage.
An uncatalogued production dataset is the data-platform equivalent of an
unregistered resource: it should not exist.

## Structure & contracts
- Layered refinement (bronze → silver → gold): raw is immutable, curated is
  tested, consumption is contracted.
- **Data contracts** at team boundaries: schema, semantics, freshness — a
  producing team's schema change is a versioned, reviewed event, not a
  surprise in a consumer's dashboard.

## Quality & freshness — failed ≠ empty, applied to data
- Data tests run **in the pipeline** (schema, nulls, ranges, referential
  checks); a failing test blocks promotion to the next layer exactly like a
  failing unit test blocks a merge.
- Every consumption dataset has a **freshness SLO**, monitored; a broken
  pipeline renders as "stale since <time>" on every dashboard it feeds —
  never as a silently old number. These signals feed the monitoring map's
  platform and product rows.

## Jobs, identity, cost
- Jobs are code, scheduled by the platform's orchestrator, run under
  **per-job workload identities** — no personal tokens in production jobs.
- **Clusters auto-terminate**; job compute is separated from interactive
  compute; interactive clusters sleep like every other non-prod resource.
  Warehouse/cluster autoscaling carries a cap.
- Pipeline and query spend lands in the same unit-economics review as
  everything else: cost per pipeline run, per consumed dataset, per
  dashboard — a denominator, not a bill.

## AI overlap
Training, evaluation and vector/feature data live under the same catalog,
classification and lineage as everything else — the data-to-model policy
(security/secure-delivery.md) is enforceable only if the data's class is
known.

## The mapping

| concept | Azure | AWS | GCP |
|---|---|---|---|
| lakehouse / warehouse | Databricks on Azure / Microsoft Fabric | Databricks / EMR + Redshift | BigQuery / Dataproc |
| catalog & lineage | Unity Catalog / Purview | Glue + Lake Formation | Dataplex / Data Catalog |
| orchestration | Databricks Jobs / Data Factory | Step Functions / MWAA | Composer / Workflows |
| data tests | dbt tests / platform expectations | dbt / Deequ | dbt / Dataform assertions |
| patching the invariants | same gate, same guards, same sleep, same ledger — whichever column you run |||
