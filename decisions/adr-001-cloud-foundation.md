# ADR-001: Cloud foundation — governed hierarchy, isolation boundary per workload+environment

**Status:** accepted (baseline) · **Category:** foundation

## Context
Everything else inherits the blast radius, billing boundary and policy scope
chosen here. Retrofitting a foundation under a live product is the most
expensive migration there is.

## Options considered
- **One boundary (subscription/account/project) for everything** — cheap to start; RBAC, quotas, cost
  attribution and policy scoping all degrade as the project grows. Rejected.
- **Boundary per team** — aligns to org chart, not to failure and billing
  domains; environments end up mixed. Rejected.
- **Governed hierarchy with one isolation boundary per
  workload+environment** — also what every provider's landing-zone
  guidance converges on (Azure CAF, AWS Control Tower, GCP landing zones).

## Decision
A hierarchy of platform / landing-zones / sandbox (management groups, OUs
or folders). One isolation boundary (subscription, account or project) per
workload **and** environment (dev, stage, prod). Policy assigned at
hierarchy level as code: allowed regions, required tags
(`workload`, `environment`, `owner`, `cost-center`), deny public storage/DB
endpoints, auto-remediation for diagnostics and backup. The chosen
cloud's published naming scheme enforced by policy, not discipline (on
Azure, CAF's `rg-<workload>-<env>-<seq>`).

## Consequences
Easier: cost per environment is a boundary filter; prod policy can be
stricter than dev; quota exhaustion in dev cannot starve prod. Harder: more
boundaries to vend — which is why vending is pipeline work (ADR-002), not
portal work. Watch: managed-cluster resource groups (`MC_*`) must be folded
into their environment in every cost report.
