# ADR-005: Runtime platform — managed containers; Kubernetes only when earned

**Status:** accepted (baseline) · **Category:** runtime

## Context
The runtime decides your operational cost floor. Kubernetes is powerful and
expensive to run well; serverless containers are cheap and constraining.

## Options considered
- **VMs** — you inherit patching, scaling and orchestration. Rejected.
- **Serverless containers** (Azure Container Apps, Google Cloud Run, AWS
  App Runner/Fargate) — managed, minimal ops surface, scale to zero.
- **Managed Kubernetes** (AKS / EKS / GKE) — full power: sidecars,
  operators, custom networking, Helm.

## Decision
Start on **serverless containers** for services that are HTTP/queue
workers with standard scaling. Move to (or start on) **managed
Kubernetes** only when you have a concrete trigger: multi-service mesh needs, stateful add-ons, custom
schedulers/operators, or an existing Helm-packaged estate. If on Kubernetes:
deploy **only** through the package manager (Helm) and pipeline — never
`kubectl patch` a managed resource by hand (it steals field ownership and
breaks the next upgrade); requests/limits and autoscaler targets are one
joint decision; non-prod clusters sleep on schedule (ADR-013).

## Consequences
Easier: the small team spends its ops budget on the product, not the
platform. Harder: the serverless→Kubernetes move is real work if triggered late — revisit
this ADR at every architecture review while service count grows.
