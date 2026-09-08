# Cloud foundation: isolation, governance, identity

These are the blueprint's own standards. They are compatible with every
major cloud's well-architected guidance (Azure CAF/WAF, AWS
Well-Architected, Google's Architecture Framework) — but the authority
here is the invariant, not the vendor. The [mapping table](#the-mapping)
translates each concept to Azure, AWS and GCP.

## Hierarchy & isolation
- An organizational hierarchy separating platform from workloads, with one
  **isolation boundary per workload per environment** (at minimum:
  non-production and production separated; a shared *management* area for
  the factory, registry, vault, telemetry). Blast radius, quota, billing
  and policy scope all inherit from this choice.
- Resource groupings per environment named by a policy-enforced
  convention, not by discipline. Plan for **provider-managed resource
  groups** (e.g. managed-cluster node groups) in cost reporting from day
  1 — fold them back into their environment, because the console will not.

## Policy as code — day 0, not day 200
- **Auto-remediating diagnostics**: every gateway, cluster, database and
  vault ships logs to the central log store by policy. Discovering during
  an incident that production has no logs is a governance failure, not
  bad luck.
- Tag enforcement: `environment`, `owner`, `workload`, `cost-center` —
  cost folding and the asset registry key on them.
- Log retention standard (90 days) — 30-day defaults silently cap every
  history feature you will later want.
- Allowed SKU/size/region policies on non-production.
- **Deny public network access** on data-holding managed services
  (storage, DB, vault, registry) at hierarchy scope — private endpoints
  are the only path (ADR-004).
- Cloud posture management enabled per boundary from day 0; the security
  score is reviewed monthly (security/security-operations.md).
- Deletion locks on stateful production resources — a database that can
  be deleted by a mistyped script is a governance gap, not an accident.

## Identity
- **Groups, never people**: platform-admins (2–3, JIT-elevated),
  developers (write non-prod, read prod), prod-approvers.
- One **workload identity per environment** for pipelines, federated
  (OIDC), least privilege. Never a shared god-principal, never long-lived
  keys.
- AI agents authenticate **as the person driving them** via short-lived
  signed tokens; agent writes are refused without attribution.
- Secrets live in the managed vault — the only home; pipelines and
  runtimes materialize from it at run time. A docs/CI gate scans every
  commit for credential-shaped strings.

## Cost management
- Budgets + alerts per isolation boundary, each with a named owner; a
  cost view that folds provider-managed groups into environments and
  prices the AI/factory workload separately by tag.
- Nightly stop/start for non-production compute — the single largest
  saving available (uptime 65h/week vs 168 ≈ 60%).

## The mapping

| concept | Azure | AWS | GCP |
|---|---|---|---|
| hierarchy | management groups | Organizations OUs | folders |
| isolation boundary | subscription | account | project |
| policy engine | Azure Policy (deny + DeployIfNotExists) | SCPs + AWS Config rules | Organization Policy + Config Validator |
| JIT privileged access | Entra PIM | IAM Identity Center + temp elevation | PAM / short-lived grants |
| workload federation | federated credentials (OIDC) | IAM Roles with OIDC | Workload Identity Federation |
| secrets vault | Key Vault | Secrets Manager + KMS | Secret Manager + Cloud KMS |
| central logs | Log Analytics | CloudWatch + S3 | Cloud Logging |
| posture management | Defender for Cloud | Security Hub + GuardDuty | Security Command Center |
| SIEM | Microsoft Sentinel | Security Lake + your SIEM | Google SecOps |
| budgets | Cost Management budgets | AWS Budgets | Cloud Billing budgets |

The naming convention follows the chosen cloud's published abbreviation
scheme (e.g. `rg-`, `kv-`, `cr`, `psql-` on Azure) — the invariant is that
policy enforces it, whichever scheme you pick.
