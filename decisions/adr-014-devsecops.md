# ADR-014: DevSecOps — scanners in the pipeline, findings with owners and SLAs

**Status:** accepted (baseline) · **Category:** security

## Context
Security tooling bolted on late produces a thousand findings, no owners and
no behavior change. The toolchain, thresholds and operating rules must be
decided before the first service ships.

## Options considered
- **Periodic audits / annual pen-test only** — point-in-time confidence
  against a continuously changing system. Rejected as the primary control.
- **Best-of-breed tool per concern, wired ad hoc** — six dashboards, zero
  accountability. Rejected.
- **One scan lane in the delivery pipeline + one findings ledger.**

## Decision
Every stage of the pipeline carries its scan (secret push-protection →
SAST/SCA/IaC/license on PR → image scan, SBOM, signing on build → admission
policy + authenticated DAST on stage), managed-platform tooling first with
pinned OSS alternates — the full table lives in
[security/secure-delivery.md](../security/secure-delivery.md). All findings
flow into **one ledger** (utility #13) with severity SLAs owned by the
service owner; suppressions carry a reason, an owner and an expiry.
Runtime: posture management on every isolation boundary, a SIEM with few
high-signal detections, weekly base-image rebuilds, scheduled key rotation
([security/security-operations.md](../security/security-operations.md)).

## Consequences
Easier: "are we secure?" becomes "read the ledger delta and the secure
score", and the annual pen-test verifies a system instead of discovering
it. Harder: thresholds will block real work in week one — tune by tightening
from warn to fail per check, never by muting the lane.
