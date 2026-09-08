# ADR-004: Network — hub-spoke, private endpoints everywhere, one ingress

**Status:** accepted (baseline) · **Category:** infrastructure

## Context
The default posture of most PaaS services is a public endpoint. Every one of
them is attack surface and a compliance finding waiting to happen.

## Options considered
- **Public endpoints + firewall rules** — IP allowlists rot; data stores
  remain internet-visible. Rejected.
- **Full hub-spoke with private endpoints and centralized egress.**

## Decision
Hub-spoke virtual networks (VNet / VPC): shared services (DNS, egress,
monitoring) in the hub, one spoke per workload+environment. **Every data-holding PaaS service (database,
storage, vault, registry, AI endpoints) gets a private endpoint** (Private Link / PrivateLink / Private Service
Connect) with private DNS zones managed centrally; the deny-public-endpoint policy from
ADR-001 enforces it. Exactly one internet entry point per environment: a
WAF-fronted ingress terminating TLS. Service-to-service traffic stays
private and is still authenticated (ADR-003) — network position is never an
authorization.

## Consequences
Easier: "is the database on the internet?" is answered by policy, not by
audit. Harder: private DNS is fiddly and breaks silently — it belongs in the
IaC modules, never hand-created; developer access to private resources goes
through a bastion or point-to-site VPN, which must be provisioned day one or
people will demand public endpoints back.
