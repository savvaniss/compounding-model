# Observability & usage

## Day 0 by policy
Container, gateway and database logs flow to the central log store by
auto-remediating policy (see baseline/cloud-foundation.md's mapping); OTel
collector; 90-day retention; structured JSON app logs
where **every meaningful user action names the user** — the single logging
rule that later makes usage analytics possible at all.

## The usage panel ("is anyone on the system?")
Live connection gauge + attributed recent actions, swept every ~2 minutes
into a persistent table so history survives deploys. Three-state verdict:
busy / quiet / **can't tell** — a failed read renders as a warning, never as
an all-clear on a decision to stop production.

## DAU & feature usage (maturity evidence)
- DAU from telemetry (tool usage tables, sign-in events), surveys only as a
  fallback.
- Feature-level usage = one structured log line per feature touchpoint
  (search, templates, export, import, integrations) + a parser in the sweep.
  You can only surface what the application emits — verify with a grep, not
  an assumption.
- Directory enrichment (country/department/title) rides the existing SSO
  federation: optional ID-token claims first (config only), then the
  directory's delegated per-user API (e.g. Microsoft Graph /me on Entra),
  and org-wide directory permissions only with admin consent.
  Validate what is actually populated per country before promising fields —
  affiliates record different attributes in different places. Report
  **aggregate-first**: per-person location tracking is a legal conversation,
  not a technical one.

## One law
**Failed ≠ empty**, in every query, API and view. Null means could-not-read;
empty means nothing-there; the UI shows which.
