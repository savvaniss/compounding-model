# ADR-002: Infrastructure as code — Terraform + verified modules, pipeline-only

**Status:** accepted (baseline) · **Category:** infrastructure

## Context
Infrastructure changed by hand cannot be reviewed, reproduced or rolled
back, and AI agents cannot safely operate on it.

## Options considered
- **Portal + scripts** — fast day one, unauditable by day thirty. Rejected.
- **First-party IaC (Bicep / CloudFormation)** — good, but single-cloud
  and a smaller reusable-module ecosystem than the Terraform registry.
  Viable alternative.
- **Terraform with the cloud's verified modules** — Azure Verified
  Modules, terraform-aws-modules/AWS-IA, Google Cloud Foundation Fabric.

## Decision
Terraform, verified-modules-first: reach for the maintained module before
writing your own; custom modules only where the ecosystem has a gap, built to the same interface
standards (typed variables with validation blocks, pinned provider
versions). Remote state per environment with locking. **Apply happens only
in the pipeline** — plan on PR, apply on merge; nobody, human or agent,
applies from a laptop. A weekly scheduled `terraform plan` against live
turns red on any diff: drift is an incident, not a curiosity.

## Consequences
Easier: every infra change is a reviewed PR; environments are rebuildable
(which is what makes DR credible). Harder: emergencies tempt portal edits —
the drift check is what makes that temptation visible and reversible.
