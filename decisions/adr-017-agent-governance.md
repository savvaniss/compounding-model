# ADR-017: Platform-native agent governance — adopt, keep, or hybrid, per invariant

**Status:** accepted (baseline) · **Category:** agent governance
**Refines:** the agent clause of ADR-003.

## Context
The platforms now ship what governed-agent teams once had to build: agent
identities, fleet registries, evaluator suites, runtime guardrails, OTel
observability. A team with a working custom control plane must decide what
to do — and "everything" is the wrong answer in both directions.

## Options considered
- **All-custom, forever** — drifts from the ecosystem; auditors, exams and
  tooling increasingly expect the native primitives. Rejected.
- **Wholesale native adoption** — discards guarantees the custom encodes
  that the platform does not (person-delegation, honest grading,
  governed knowledge freshness, as-billed cost attribution). Rejected.
- **Decide per invariant.** Chosen.

## Decision
For each capability in [baseline/agentic-platform.md](../baseline/agentic-platform.md):
- **Adopt native where it meets or exceeds the invariant**: runtime
  prompt-injection shields, AI-workload threat detection, OTel GenAI
  emission conventions, and native evaluator suites *running alongside*
  the judge.
- **Keep custom where it exceeds native, and write down why**:
  person-delegated identity for interactive agents (a human name on every
  write beats an agent's own name); a judge that can answer "can't
  grade"; knowledge freshness the platform cannot guarantee — live-read
  where no merge automation governs a mirror (see the addendum: a mirror
  refreshed by the same automation that changes the sources meets the
  invariant, and won on retrieval); as-billed cost attribution.
- **Hybrid where the classes differ**: standing agents get platform
  agent-identity and registry entries; interactive agents stay
  person-delegated. The team's own console and the org registry co-exist —
  dual-homed, one source of truth per audience.
- **A2A is documented, not adopted**, while orchestration is
  pipeline-mediated by design.
Every *keep* is re-reviewed at the quarterly technology-intake cadence:
when the platform catches up to the invariant, the keep loses its reason.

## Consequences
Easier: the estate is legible to native control planes (the one-socket
dividend), and program/audit evidence reads in the ecosystem's own
vocabulary. Harder: dual emission and dual registry until convergence, and
each keep carries a rationale that must be restated as platforms evolve —
that restatement is the review working as intended.

## Addendum — first applications (2026-09)

Two capabilities crossed the framework in practice. Both outcomes are
recorded here so the next review starts from evidence, not memory.

**Knowledge retrieval: the governed mirror was adopted; the live-read keep
is retired for retrieval.** A hybrid search index (keyword + vector +
semantic reranking) over the same documents beat live keyword scans
decisively — paraphrases land, and ranked passages replace whole documents
in prompts. The invariant held because the mirror is governed exactly as
the baseline demands: a named owner, a freshness SLO enforced by the same
merge automation that changes the documents, schema and ingestion in code,
and the repository remaining the system of record for full reads.
Live-read remains right only where no such automation exists.

**Hosted agent runtime: hybrid.** The assistant-class agent moved onto the
platform's agent service — control-plane inventory, per-run traces, an
evaluator surface, workload identity — while pipeline-embedded agents stay
custom, where hosting adds coupling and replaces nothing. Findings for the
next team, all hit in one afternoon:
- Verify your actual models run under the hosted runtime before
  committing: model-channel gaps and forced sampling parameters
  (a runtime that always sends `top_p` cannot drive models that reject
  it) are real, current, and invisible until the first run.
- Grounding connectors may demand broader credentials than retrieval
  needs — one accepted only admin keys or workload identity. Accept
  workload identity only; a write-capable key never belongs in a
  connection object.
- Test hosted models for invented citations: one answered ownership
  questions from memory, citing documents that do not exist, despite
  must-retrieve instructions — and obeyed a forced first retrieval call
  every time. Trust the trace, not the prose.
- Pass the caller's credential per run, never in the agent definition, so
  person-attribution survives hosting intact.
