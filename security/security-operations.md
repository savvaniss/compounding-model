# Security operations — after the pipeline, forever

The pipeline proves the artifact was clean when it shipped. Security
operations is everything after: posture, detection, response, rotation,
review. Small teams don't get to skip this — they get to automate it.

## Posture management (know your surface)
Cloud-native posture management (Defender for Cloud / AWS Security Hub +
GuardDuty / Security Command Center) enabled on every isolation boundary, with the secure score reviewed monthly and
regressions treated as bugs. The deny/auto-remediate policies from the foundation
(ADR-001) are the preventive layer; posture management is the detective
layer that catches what policy could not express.

## Detection & the SIEM
Audit and sign-in logs, WAF logs, Kubernetes audit, key-vault access logs
and the app's actor-attributed logs (ADR-009) flow into one SIEM
(Microsoft Sentinel, Google SecOps, or whichever your organization runs). Start with few high-signal detections
instead of a hundred noisy ones:
- privileged role activated outside PIM process, or by an unexpected actor
- secret read from the vault by an identity that never read it before
- deploy to production outside the change-delivery pipeline
- agent token minted for a person outside the team roster
- impossible-travel / anomalous sign-in on accounts with prod access
- WAF spike on a single route (someone found something interesting)

Every detection has an owner and a runbook; an alert nobody triages within
its SLA gets deleted or fixed — alert debt is a security hole shaped like
fatigue.

## Vulnerability management
One findings ledger across all scanners (pipeline + runtime + posture),
severity SLAs owned by the service owner, not "the security person":

| severity | fix SLA | escalation |
|---|---|---|
| critical / known-exploited | 48h, hotfix path allowed | incident process |
| high | 7 days | tech lead |
| medium | 30 days | backlog, visible |
| low | best effort, batched | quarterly review |

Base images rebuilt on a weekly schedule (not only on release) so patch
inheritance actually happens; cluster nodes on auto-upgrade with a
maintenance window; dependency-update PRs (Renovate/Dependabot) merged on
cadence, not hoarded.

## Secrets & key rotation
Every secret in the vault carries an expiry and a rotation runbook; rotation
is exercised on schedule, not only on compromise. A secret that appears in
any transcript, log, ticket or repo is **rotated the same day** — no
"assessing exposure" period. Certificate expiry is monitored with a 30-day
alert; an expired cert is a preventable outage with a security label.

## Access & audit cadence
Quarterly: access review of prod-touching groups, PIM eligibility, vault
RBAC and pipeline approvers — with evidence stored. Continuous: admin and
approval actions audited and queryable by actor (the identity design in
ADR-003 is what makes this a query instead of a project).

## Security incident response
Security incidents run through the same incident process as outages
(operations/sre.md) with two additions: a containment-first playbook
(revoke tokens, rotate credentials, isolate workload — in that order,
pre-written per scenario) and a disclosure decision point (legal/privacy
notified when personal data may be affected). One tabletop exercise per
half-year: pick a detection from the SIEM list and walk it end-to-end; the
gaps found become backlog items with owners.

## Assurance
Annual penetration test or red-team on the exposed surface, scoped against
the container diagram; restore-from-backup exercised with evidence (an
untested backup is a hope); postmortems — security and otherwise — feed the
lessons library the AI serves back during work.
