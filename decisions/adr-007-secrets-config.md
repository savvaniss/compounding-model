# ADR-007: Secrets & configuration — the vault is the only home

**Status:** accepted (baseline) · **Category:** security

## Context
Secrets sprayed across pipeline variables, `.env` files, wikis and tickets
are the most common real-world breach path — and the hardest habit to
unwind later.

## Options considered
- **Pipeline-variable secrets** — invisible to rotation, copied freely,
  no audit of reads. Rejected.
- **Secrets in (private) repos** — history never forgets. Rejected.
- **Key Vault as single source, synced to runtime.**

## Decision
Every secret lives in the managed vault (Key Vault / Secrets Manager /
Secret Manager — one per workload+environment), RBAC'd to
the workload identity (ADR-003). Runtime consumption via the platform's
vault sync (CSI driver / secret store integration) — apps read environment
variables, never call the vault in code. Pipelines fetch at run time by
federated identity. Whole-file secrets (certificates, layered values
files) live in the vault too; where a platform needs a file or a secure
file blob, the pipeline **materializes it from the vault at run time** —
transports exist, second homes do not. **Non-secret config is layered files
in git** (base + per-environment overlay) so a diff between environments is
a `diff`. Push-protection secret scanning on every repo; a leaked secret is
rotated the same day, not assessed for weeks.

## Consequences
Easier: rotation is one vault write + redeploy; "who read this secret" is an
audit query. Harder: local development needs a documented fetch path or
developers will re-invent `.env` sharing — write that runbook first.
