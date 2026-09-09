# ADR-005: Runtime platform — managed containers; Kubernetes only when earned

**Status:** accepted (baseline) · **Category:** runtime

## Context
The runtime decides your operational cost floor. Kubernetes is powerful and
expensive to run well; serverless containers are cheap and constraining.

## Options considered
- **VMs** — you inherit patching, scaling and orchestration. Rejected.
- **Serverless containers** (Azure Container Apps, Google Cloud Run; on
  AWS, App Runner/Fargate with the caveat that neither truly scales to
  zero — App Runner pauses to a billed idle state) — managed, minimal ops
  surface.
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

## When a VM is unavoidable

Vendor appliances, GPU hosts, Windows/AD-bound workloads and lift-and-shift
estates exist. They do not escape the invariants — they get VM-shaped
versions of them:

- **Golden images, not hand-built servers**: images are baked by a pipeline
  (shared image gallery / AMI / machine image), versioned, scanned, and
  rebuilt on a schedule so patch inheritance happens.
- **In-guest state is code**: cloud-init / Ansible / DSC applied at boot;
  the IaC drift check cannot see inside the guest, so config management IS
  the drift control there. A hand-fixed server is rebuilt from image, not
  kept — cattle, not pets.
- **Patching in rings** via the cloud's patch manager (Update Manager /
  SSM Patch Manager / OS Config), same severity SLAs as
  security-operations.
- **No standing SSH/RDP**: bastion + just-in-time access only; Windows
  services use managed identities/gMSA, never stored passwords.
- Backup agents, monitoring agents and deletion locks like any stateful
  resource; non-production VMs sleep on the same schedule as everything
  else.

## Consequences
Easier: the small team spends its ops budget on the product, not the
platform. Harder: the serverless→Kubernetes move is real work if triggered late — revisit
this ADR at every architecture review while service count grows.
