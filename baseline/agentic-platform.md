# The agentic platform — invariants first, control planes as mappings

Every hyperscaler now ships an agent control plane: platform identities for
agents, fleet registries, evaluator suites, runtime guardrails. Teams that
built their own governance face pressure to switch wholesale; teams that
built nothing face pressure to adopt wholesale. Both reflexes are wrong.
**The invariants below do not move; the products are mappings** — and every
adopt/keep/hybrid call is an ADR ([ADR-017](../decisions/adr-017-agent-governance.md)),
never drift.

## The invariants (defined elsewhere, gathered here)

| Capability | The invariant | Home |
|---|---|---|
| Identity | every agent write traces to a human sponsor; anonymous writes refused at the tool boundary | [identity.md](identity.md), ADR-003 |
| Tool boundary | one governed socket for every question and action; refusal + per-call telemetry at that boundary | [factory.md](factory.md) |
| Registry | every agent inventoried at birth: owner, purpose, permissions, lifecycle | REGISTRY rule, [factory.md](factory.md) |
| Evaluations | continuous judgment at a declared sampling rate; honest "can't grade"; **acceptance thresholds gate releases** | [ai-standards.md](ai-standards.md) |
| Runtime guardrails | prompt-injection shields at the model gateway; data classes mapped to allowed models | [security/secure-delivery.md](../security/secure-delivery.md) |
| Observability | every call telemetered, emitted in OpenTelemetry GenAI semantic conventions | [observability.md](observability.md), ADR-009 |
| Knowledge | agents read the sources of record live — a mirror needs an owner and a freshness SLO | [knowledge-work-integration.md](knowledge-work-integration.md) |

## Two identity classes, one attribution rule

- **Interactive agents** (a person is driving): no identity of their own —
  they act *as the person*, via short-lived personal tokens. This is
  stronger attribution than any agent-own identity: a human name on every
  write, adoption measurable per person for free.
- **Standing agents** (the factory's long-running workloads: the merge
  gate, the judge, the delivery orchestrator, the team assistant): real
  workload identities with a named owner and a lifecycle — use the
  platform's agent-identity primitive where it exists, and inventory them
  in the registry like any asset.

One rule covers both: **every write traces to a human sponsor** — the
driver, or the standing agent's owner.

## The one-socket dividend

The control-plane vendors converged on the same integration point this
blueprint mandates: the open Model Context Protocol. If your agents reach
their tools through one MCP socket, a native control plane can *see,
govern and secure* that estate by registration — adoption becomes
configuration, not re-architecture. Building the socket first is what
keeps the platform decision reversible.

## The mapping

| Capability | Microsoft | AWS | GCP |
|---|---|---|---|
| agent identity | Entra Agent ID | AgentCore Identity / IAM roles | Vertex agent identities / service accounts |
| fleet registry & governance (org side) | Agent 365 | Bedrock AgentCore console | Agentspace / Agent Builder |
| control plane (builder side) | Foundry Control Plane | Bedrock AgentCore | Vertex AI Agent Engine |
| agent runtime | Foundry Agent Service | AgentCore Runtime | Vertex AI Agent Engine |
| evaluations | Foundry Evaluations (+ continuous eval) | Bedrock evaluations | Vertex gen-AI evaluation service |
| runtime guardrails | Prompt Shields / Content Safety | Bedrock Guardrails | Model Armor |
| AI observability | Foundry Observability (OTel) | AgentCore Observability / CloudWatch | Cloud Trace / Vertex dashboards |
| knowledge / grounding | Foundry IQ / AI Search | Bedrock Knowledge Bases | Vertex AI Search / RAG Engine |
| AI data governance | Purview | Macie + Lake Formation | Sensitive Data Protection + Dataplex |
| workplace copilot surface | Microsoft 365 Copilot | Amazon Q Business | Gemini Enterprise |
| code assistant surface | GitHub Copilot | Amazon Q Developer | Gemini Code Assist |

A team's own factory console can *be* the builder-side control plane; the
org-side registry is where native adoption usually starts, because it is
where the auditors look.

## Agent-to-agent (A2A)

The A2A protocol standardizes agents negotiating with each other. A
pipeline-mediated design — agents interacting through the delivery system's
own queues, gates and tickets rather than peer-to-peer — is a *valid
architecture*, not a gap: it keeps every interaction on the audited rails.
Record the choice (ADR-017 does); adopt A2A when two agents genuinely need
to negotiate outside those rails, and not before.
