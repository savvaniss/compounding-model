# Lesson: <one-line title>

**Date:** · **Cost:** <what it actually cost: time, outage, rework>

## What happened
Chronology with the evidence inline: the exact error text, the command run,
the numbers observed. A lesson without evidence is an opinion.

## Why nothing caught it
Which guard was missing, which assumption was untested.

## Rules
Checkable rules, not aspirations — each verifiable with a command or a gate.

---

## Seeded example

# Lesson: a deploy referenced a tag no image carried

**Date:** _(engagement)_ · **Cost:** one production incident — old pods kept
serving while the rollout silently failed to pull.

### What happened
A component's build stage was skipped, but the deploy stage ran anyway with
the new tag. The registry had no such image; the rollout stalled in
image-pull backoff; the previous version kept serving and the change was
reported as delivered.

### Why nothing caught it
The deploy pipeline trusted its parameter. Nothing verified that the
artifact existed before rolling, and nothing verified rollout convergence
after.

### Rules
- Before any deploy: query the registry — **the tag must exist** or the run
  fails at parameter time.
- After any deploy: verify rollout convergence and endpoint health; a green
  pipeline is not the finish line.
- Never trigger component pipelines by hand around the orchestrator; the
  orchestrator is where these guards live.
