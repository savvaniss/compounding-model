# Quality engineering

## Test strategy
The pyramid enforced by convention and review: fast unit tests on extracted
decision logic (pure modules, no framework welding) · service/API tests per
capability · a thin, stable E2E smoke on the critical journeys ·
**AI-assisted test generation runs advisory on every PR** so coverage gaps
are visible at review time, accepted or consciously declined.

## Definition of Done
Code + tests + docs updated + observability (logs name the actor, metrics,
runbook delta) + NFR rows checked + feature flag/rollback path + evidence
attached to the ticket. "Works on my machine" and "works on stage" are
different claims from "verified on the deployed environment" — only the last
closes a ticket.

## Test data & environments
Production-shaped data in stage, personal data masked per classification ·
seeds and fixtures versioned with the code · every environment rebuildable
from code, so a broken test env is a redeploy, not a week.

## Non-functional testing
Performance tests on the hot paths with budgets in the pipeline ·
accessibility checks in DoD for UI · chaos-lite: kill a pod in stage on a
schedule and watch the SLO dashboards react — that is the monitoring map
being tested, not just the app.

## The release checklist
The mechanism the NFR catalogue's operability row points at. Per release,
timed entries (so toil is measurable and improvements provable): rollout
converged in every target environment · endpoint answers from outside the
cluster · migrations applied and history consistent · dashboards green,
no new alert · release notes and registry rows updated · rollback tag
verified pullable. Kept as code next to the pipeline; the deploy pipeline
automates entries out of the checklist over time — that is the point of
timing them.

## AI-assisted work is reviewed work
AI-generated code follows the same gate, standards and tests as human code —
the gate does not care who typed it, and neither does production.
