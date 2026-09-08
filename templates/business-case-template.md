# Business case: <epic title>

One page. If it cannot fit on one page, it is not understood yet.

**Requested by:** · **Date:** · **Ticket/epic:**

## Problem
In the words of the person who has it — not the solution restated as a need.

## Who benefits, and how often
Named roles/segments and the frequency of the pain.

## Value hypothesis
> We believe **<change>** will move **<metric>** by **<amount>** within
> **<period>**.
This sentence is checked at +30/+90 days after shipping (benefits
realization); write it so it can lose.

## Options
Including "do nothing". One line each: cost, risk, reversibility.

## Rough cost
Build + run + **AI consumption** (tokens are a run cost, not a rounding
error).

## NFR impact
Which rows of solution/nfr-catalogue.md does this touch or degrade?

## Risks & reversibility
What we must monitor if we proceed; how we back out.

---

## Seeded example (abridged)

**Problem:** reviewers spend the first day of every release hand-checking
that changed services still answer after deploy.
**Value hypothesis:** we believe automated post-deploy verification will cut
release-day verification effort by half within one quarter, measured by the
release checklist's timed entries.
**Options:** do nothing (keeps a day of toil per release) · buy a synthetic
monitoring add-on (fast, recurring cost, only covers HTTP) · extend the
deploy pipeline to verify rollout convergence + endpoint health (a week to
build, covers every service, becomes a guard).
**Decision path:** third option; NFR rows touched: operability,
availability. Reversibility: full (a pipeline stage).
