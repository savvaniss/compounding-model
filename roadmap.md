# Roadmap: four weeks to a Stage 3 (compounding) delivery system

**Agent: this roadmap is yours to execute**, the way you executed the
quick start — you provision the infrastructure, build the utilities and
wire the telemetry as gated PRs; humans decide at approvals, ADR
disagreements and steering points. The order matters: **decisions and the
factory come first**, so everything later arrives as a reviewed, gated
pull request instead of being retrofitted under a moving train. Security, operations and the solution front-half are
lanes from day one — not phases bolted on at the end.

## Day 0 — decide & govern
Adopt the [decision baseline](decisions/) — fifteen pre-accepted ADRs;
disagreements become superseding ADRs now, not drift later. Repositories,
branch policies, identity (groups + PIM, never individuals; per-person
tokens for agents), secrets policy (one home: the vault, everything
else a transport from it; push-protection scanning on), asset registry file created empty,
[working agreement](enforcement/working-agreement.md) signed by humans and
agents alike.

## Week 1 — the factory and both gates
The AI merge gate (blocking, legible, overridable on the record), the
pipeline skeleton with its guards, **and the security scan lane in the same
pipeline** (secrets, SAST, SCA, IaC scans — thresholds start at warn,
tighten to fail). Runbook library seeded with your first lessons. From this
point nothing lands except through a gated pull request — including the
factory's own changes.

## Week 2 — provision through the factory
Infrastructure as code arrives as the factory's first gated deliveries —
authored and applied by the agent through the pipeline, never by hand: the
**factory spoke first** (architecture/factory-infrastructure.md), then the
product spokes — hub-spoke, private endpoints, workload identities, per
the ADRs. Environments with identical deployment definitions (charts or
container-app templates) and per-environment values;
production promoted only from an environment branch, behind a human
approval. Image scanning, SBOM and signing join the build.

## Week 3 — observe, measure, operate
Stand up the [monitoring map](operations/monitoring-map.md) from the bottom
up: platform telemetry, SLOs with error budgets per user-facing service,
the activity/usage panel, the LLM cost ledger with its effective-dated
price table, the judge loop sampling real generations, delivery metrics
(DORA four keys). Posture management and the first SIEM detections go live
(security/security-operations.md). Alerts page on budget burn, not CPU.

## Week 4 — attach delivery to value
The solution front-half becomes routine: one intake front door, one-page
business cases with value hypotheses, Definition of Ready enforced, the
NFR catalogue filled with this project's real targets, steering cadence
running with the numbers visible. First benefits check scheduled (+30
days). Evidence checklists reviewed — anything not yet produced as a
by-product gets a mechanism, not a screenshot.

## Then — the application work, on rails
Every subsequent feature demonstrates the maturity questions by
construction: AI-assisted (and attributed), reusable (in the factory),
measured (in the ledger), governed (through the gates), and **traceable to
an outcome whose effect is checked after shipping**.

## Preparing for validation
Keep the [evidence checklists](evidence/) green continuously. A project
that can export its gate verdicts, cost dashboard, usage telemetry, asset
registry and findings ledger on any random Tuesday passes validation
without a scramble.
