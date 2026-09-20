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
  live-read knowledge, as-billed cost attribution). Rejected.
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
  grade"; live-read knowledge over index mirrors; as-billed cost
  attribution.
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
