# Identity — workforce, workload, agents, customers

The most cross-cutting concern in the blueprint; every incident class it
prevents is expensive.

## Workforce (humans)
Groups, never individuals, for every grant · privileged roles via PIM/JIT
with expiry · **joiner–mover–leaver**: access is a consequence of group
membership so a mover's old rights fall away · quarterly access review with
evidence. Directory reality check before designing on attributes: guest
accounts carry mangled UPNs (join on mail, never UPN), attributes are
per-affiliate inconsistent, and one human may hold several directory objects
— enrichment must tolerate duplicates and gaps.

## Workload (services & pipelines)
One workload identity per environment, federated (OIDC), least privilege,
no long-lived secrets in pipelines · service-to-service auth explicit, never
"internal network = trusted" · every credential's home is the vault, scanned
for at every commit.

## Agents (the new class)
AI agents hold **no standing identity of their own**: they act as the person
driving them, via short-lived signed tokens; unattributed writes are refused
at the tool boundary; every call is telemetered per person. Production is
queue-only for agents — a named human lands it. This one design choice makes
AI adoption auditable, measurable (DAU per person), and safe to scale.

## Application authorization
Resource-level checks on every mutating route (owner/role on the object,
not "a session exists") · admin actions audited · authorization logic unit
tested like any other decision logic.

## Customer identity (if the product has end users)
Federated SSO to the customer's IdP; the app stores the minimum (subject,
mail, display name) and enriches at sign-in via token claims first, Graph
delegated second, tenant-wide permissions only with admin consent — the
consent letter is a project task, not an afterthought.
