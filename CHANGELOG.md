# Changelog

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
