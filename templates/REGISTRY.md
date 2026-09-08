# Asset registry

Every asset this project creates — resource, repository, pipeline, site,
table, document — gets a row **in the same commit that creates it**. If it is
not registered, it should not exist.

| asset | purpose |
|---|---|
| _(example)_ `pipelines/change-delivery.yml` | the one-call delivery loop |

## Seeded example rows

| asset | purpose |
|---|---|
| `pipelines/change-delivery.yml` | the one-call delivery loop: build → deploy → migrate → verify → report |
| `dashboards/cost-by-environment` | subscription spend with managed-cluster groups folded into their environment |
| `runbooks/environment-wake.md` | one-click wake of a slept non-prod environment, with verification |

Columns to add as the project grows: owner, created (date + PR link),
environment. The rule never changes: **row and asset in the same commit** —
the docs guard blocks the PR otherwise.
