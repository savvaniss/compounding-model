# Terraform standards — verified modules first

## Modules
- **Prefer the cloud's verified module ecosystem**: Azure Verified Modules
  (`Azure/avm-res-<provider>-<resource>`) and the `terraform-aws-modules`
  collection / AWS-IA modules from the Terraform Registry; on GCP, the
  registry-published `terraform-google-modules` (Cloud Foundation
  Toolkit), or Cloud Foundation Fabric vendored from GitHub. They carry maintained, well-architected defaults and
  standard interfaces (diagnostics, locks, IAM, private endpoints). Only
  write a custom module when no verified module covers the resource or a
  hard requirement conflicts with its interface.
- Custom modules follow the verified-module shape: one resource family per module;
  `variables.tf` fully typed and described with `validation` blocks for every
  finite domain (a wrong value must fail at plan, not at 2 a.m.); outputs
  minimal; examples/ folder; versioned by git tag and **consumed by tag,
  never by branch**.
- Defaults equal current production behaviour, so adding a variable is never
  itself a change.

## State & execution — the rule above all others
- Remote state in a locking backend (Azure storage account / S3 +
  DynamoDB / GCS), **touched only by a pipeline**: `terraform plan` on every PR with the plan posted as a comment;
  `apply` on merge; no local applies.
- **Weekly drift detection**: a scheduled pipeline runs `plan` against live
  and fails red on any diff. Drift is an incident. This single job prevents
  the worst structural failure a project can have — state so stale that
  applying it becomes dangerous, forcing emergency changes through the CLI
  that become permanent undocumented drift.
- An emergency CLI change is allowed, but must land in code within 24h or be
  reverted.

## Bootstrap — the only hand-made resources
The state backend (and the identity that runs the first apply) cannot be
created by a pipeline that does not yet exist. This is the **entire**
legitimate hand-made set: create it once, tag it, give it its registry row,
and `terraform import` it in the repository's first PR so that from commit
one, everything that exists is in code. Everything after bootstrap —
including new pipelines, policies and dashboards — arrives as code through
the pipeline; a hard platform limit with no code path is recorded as an
exception in the registry, never silently accepted.

## Code rules
- `required_version` and provider versions **pinned**; upgrades are their own
  PR with the plan attached.
- One apply-unit per lifecycle (cluster, addons, data, AI services,
  monitoring), order encoded in the pipeline, not a README.
- Tags injected centrally via locals; secrets never in tfvars — vault
  data sources or pipeline-injected.
- Review gate for infra PRs: fmt + validate + tflint + **IaC security scan**
  (PSRule for Azure / Checkov — any high finding fails) + plan + the same AI
  review the application repos get. Destructive plans require human approval
  even on non-production.
