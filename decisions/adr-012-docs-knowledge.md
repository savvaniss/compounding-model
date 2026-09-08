# ADR-012: Documentation & knowledge — docs-as-code, a registry, a lessons library

**Status:** accepted (baseline) · **Category:** knowledge

## Context
An AI-mature team's assistant is only as good as the written knowledge it
can serve back. Undocumented assets become unowned assets; unrecorded
lessons get re-learned at production prices.

## Options considered
- **Wiki, best-effort** — unversioned, unreviewed, invisible to agents in
  their working context. Rejected as the primary home.
- **Docs-as-code in the factory repo, enforced by the same gates.**

## Decision
Runbooks, ADRs, standards and lessons live in the factory repository as
markdown, PR-reviewed like code. **Registry rule**: every created asset
(pipeline, resource, dashboard, utility) gets its registry row in the same
commit that creates it — the docs guard blocks the PR otherwise. **Lessons
library**: every incident and every process failure becomes an entry with
its evidence, written as a checkable rule ("after merge, deploy; after
deploy, verify") — and the AI assistant serves these back during work,
which is what makes them compound instead of rot. Docs have a size cap;
past it, split — a document nobody can load is a document nobody reads.

## Consequences
Easier: onboarding (human or agent) is "read the repo"; the assistant's
answers are grounded in reviewed truth. Harder: the same-commit registry
rule feels pedantic weekly and priceless quarterly.
