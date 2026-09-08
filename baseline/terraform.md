# Terraform standards — verified modules first

## Modules
- **Prefer the cloud's verified module ecosystem** from the Terraform
  Registry: Azure Verified Modules (`Azure/avm-res-<provider>-<resource>`),
  the `terraform-aws-modules` collection / AWS-IA modules, Google's Cloud
  Foundation Fabric. They carry maintained, well-architected defaults and
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
