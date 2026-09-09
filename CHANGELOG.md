# Changelog

## 1.3.0 — 2026-09-09

- **Roadmap recalibrated to agent execution**: five dependency-ordered
  phases; the bulk build is hours of agent work, the calendar belongs to
  human decisions, cloud lead times and evidence accumulation.
- **VM coverage** (ADR-005 annex): golden-image pipelines, in-guest
  config as code (the drift control terraform cannot see), patch rings,
  JIT-only access, cattle-not-pets rebuilds — for the estates where a VM
  is unavoidable.
- **Data & analytics** (baseline/data-analytics.md + ADR-016): managed
  lakehouse per cloud; notebooks are code through the gate; mandatory
  catalog with classification and lineage; data tests gate layer
  promotion; freshness SLOs render "stale since", never silently old
  numbers; clusters auto-terminate and join the unit-economics review.
  New NFR row for data freshness & quality.
- Second deep-audit pass: 25 verified corrections (diagram parse fix,
  ticket-approval stop in the quick start, guard-placement alignment,
  ticket/feature id through the cost ledger, residual vendor terms).

## 1.2.0 — 2026-09-09

- **Everything is code — no manual actions** (CLAUDE.md,
  baseline/terraform.md, working agreement): agents never change
  infrastructure, pipelines or policies by hand; the bootstrap set and
  recorded hard-platform-limit exceptions are the only hand-made surface;
  emergency changes land in code within 24h.
- **Knowledge & work integration** (baseline/knowledge-work-integration.md
  + ADR-015): the wiki of record and the work tracker are live tools on
  the one MCP socket — federated search, attributed comments, read-before-
  acting as contract; **an action without a ticket never happened**, and
  agents never open tickets unasked. Wired through the factory connection
  pattern, utility #2, the agent contract and the README.

## 1.1.0 — 2026-09-09

The agent-first release.

- **AI assessment replaces self-assessment**: the Pages site is now a
  launcher for a prompt your own agent runs against your repos, pipelines
  and telemetry, grading only on cited evidence ("can't tell" scores as
  the lower stage); the prompt is fetchable at
  assessment/ai-assessment-prompt.md.
- **Written for your agent** is now the stated operating philosophy:
  humans decide, agents execute, every execution leaves evidence. The
  quick start is an agent instruction set with marked human decision
  stops, the roadmap is addressed to the agent as its executor, and the
  agent explicitly **builds the factory infrastructure itself** (spoke as
  IaC, thirteen utilities in order, each a gated registered PR).
- **Cloud-agnostic**: the foundation states every invariant as the
  blueprint's own standard with full Azure / AWS / GCP mappings
  (baseline/cloud-foundation.md); Terraform is verified-modules-first
  across ecosystems; DevOps practices are engine-neutral with an Azure
  DevOps / GitHub / GitLab / AWS / GCP mapping table; ADRs and security
  docs neutralized to match.
- **Secrets decision restored**: the vault is the only home — pipeline
  variables and secure files are transports materialized from it.
- **README redesigned** around the model: a guides/enforces hub diagram,
  demand-phrased category table, and recreated architecture diagrams
  (one-socket MCP edge, scan lane, quota-split gateway).
- **Deep-audit pass**: 23 verified findings fixed (broken relative links,
  cross-file contradictions, dead-end pointers, factual corrections) and
  the git history rebuilt to carry no engagement-identifying metadata.
- Repository renamed to **compounding-model**.


## 1.0.0 — 2026-09-09

First public release of **The Compounding Model** and its blueprint.

- The maturity model: six questions × three stages; your stage is your
  lowest provable answer.
- AI-run assessment: an agent prompt that audits your repos, pipelines
  and telemetry and grades on cited evidence (GitHub Pages launcher +
  assessment/ai-assessment-prompt.md).
- 14 seeded architecture decision records — the recommended default per
  category, pre-accepted.
- Solution front-half: intake, business cases with falsifiable value
  hypotheses, NFR catalogue, benefits realization.
- Delivery baseline: cloud foundation as policy-as-code invariants with
  Azure/AWS/GCP mappings, Terraform with verified modules, DevOps guard
  rules, development & quality standards.
- The AI factory: merge gate, tool server, cost ledger, judge + eval
  harness, and its own infrastructure architecture (factory as a spoke).
- DevSecOps scan lane and security operations doctrine.
- Operations: SRE practice and the seven-altitude monitoring map.
- Enforcement specs, evidence checklists per stage, seeded templates,
  and an agent operating manual (CLAUDE.md).
