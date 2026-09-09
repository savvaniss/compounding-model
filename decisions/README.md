# Decision baseline — seeded ADRs

A new project should not start from a blank decision log. These ADRs are the
**recommended default for every category**, pre-written as *accepted* so the
team starts from an opinionated baseline. Adopting the blueprint means
adopting these; disagreeing is fine — but it happens by writing a superseding
ADR with your context, not by silently diverging.

| # | Category | Decision in one line |
|---|---|---|
| [001](adr-001-cloud-foundation.md) | Cloud foundation | governed hierarchy; isolation boundary per workload+environment; policy as code (Azure · AWS · GCP mappings) |
| [002](adr-002-infrastructure-as-code.md) | Infrastructure as code | Terraform with the cloud's verified modules; pipeline-only state; weekly drift check |
| [003](adr-003-identity.md) | Identity & access | directory groups + JIT elevation for humans; federated workload identity; agents act as the person |
| [004](adr-004-network.md) | Network topology | hub-spoke; private endpoints for all PaaS; one WAF'd ingress |
| [005](adr-005-runtime-platform.md) | Runtime platform | serverless containers until you outgrow them; managed Kubernetes when you actually need it |
| [006](adr-006-data-platform.md) | Data platform | managed PostgreSQL; forward-only migrations as code; pgvector before a vector DB |
| [007](adr-007-secrets-config.md) | Secrets & config | the vault is the only home — everything else is a transport materialized from it; layered non-secret config |
| [008](adr-008-delivery-model.md) | Delivery model | PR-only env branches; build once, promote the artifact; human approval into prod |
| [009](adr-009-observability.md) | Observability | OpenTelemetry everywhere; logs name the actor; SLOs per service; failed ≠ empty |
| [010](adr-010-ai-model-access.md) | AI model access | one gateway; quota isolation build-vs-product; cost ledger; eval harness gates swaps |
| [011](adr-011-api-integration.md) | APIs & integration | contract-first OpenAPI; queues for long work; idempotent handlers |
| [012](adr-012-docs-knowledge.md) | Docs & knowledge | docs-as-code with a registry; lessons library served back by the AI assistant |
| [013](adr-013-cost-management.md) | Cost management | tagging standard; budgets with owners; unit economics; non-prod sleeps |
| [014](adr-014-devsecops.md) | DevSecOps | scan lane in the pipeline; one findings ledger; SLAs per owner; suppressions expire |
| [015](adr-015-knowledge-work-integration.md) | Knowledge & work | wiki + tracker as live tools; ticket = unit of work; agents never open tickets unasked |
| [016](adr-016-data-analytics.md) | Data & analytics | managed lakehouse; notebooks are code through the gate; catalog + freshness SLOs mandatory |

Format for new/superseding ADRs: [templates/adr-template.md](../templates/adr-template.md).
