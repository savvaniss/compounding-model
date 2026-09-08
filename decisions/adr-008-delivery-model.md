# ADR-008: Delivery — PR-only env branches, build once, human lands production

**Status:** accepted (baseline) · **Category:** delivery

## Context
This is the spine the AI factory bolts onto: the gate reviews PRs, guards
protect pipelines, agents queue changes. The branch/artifact model must make
the safe path the easy path.

## Options considered
- **Pure trunk-based with env promotion by tag** — excellent when test
  automation is mature; unforgiving before that. Viable end-state.
- **GitFlow** — heavyweight, merge-debt machine. Rejected.
- **Environment branches (dev → stage → main) with PR-only movement.**

## Decision
Protected environment branches; nothing reaches any of them except by PR
through the AI review gate (advisory verdicts, blocking overridable with a
stated reason). **Build once**: an image is built from a commit, and that
same artifact is promoted — a deploy of a tag that no registry image carries
is refused by pipeline guard. Promotions between environment branches are
**real merges, never squash** (squash orphans the history and strands the
next promotion). All changes flow through one change-delivery orchestration
(build → deploy → migrate → verify → report); agents may queue production,
only a named human approves it (ADR-003). After merge, deploy; after deploy,
verify on the environment — a green pipeline is not the finish line, the
converged rollout is.

## Consequences
Easier: every production change has a PR, a verdict, an approval and a
verified rollout attached. Harder: hotfix pressure will beg for direct
pushes — the answer is making the orchestrated path take minutes, not
granting exceptions.
