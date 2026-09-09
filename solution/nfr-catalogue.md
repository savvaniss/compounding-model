# Non-functional requirements catalogue

NFRs are requirements, not aspirations: each has a target, a measurement, and
an enforcement point. Set the targets per project; never leave a row "TBD".

| quality | target (example) | measured by | enforced at |
|---|---|---|---|
| Availability | SLO 99.5% monthly, error budget tracked | synthetic probes + SLI dashboards | SRE review; releases pause when budget exhausted |
| Performance | p95 page/API latency targets per journey | RUM/APM + load tests | perf test in pipeline for hot paths |
| Scalability | 2× peak without redesign | capacity tests, autoscaling verified | quarterly capacity review |
| Security | see security/secure-delivery.md | SAST/DAST/SCA/secret scan | pipeline gates + pen-test cadence |
| Privacy | data classified; retention per class; DSR path | data inventory | design review + policy |
| Accessibility | WCAG 2.2 AA for user-facing UI | automated + manual audit | definition of done |
| Operability | every service: health probes, structured logs w/ actor, runbook | deploy checklist | gate + release checklist |
| Recoverability | RTO/RPO per tier; **restore tested**, not assumed | DR exercise evidence | scheduled game day |
| Cost | unit economics per transaction/user incl. AI tokens | cost ledger + cloud cost views | monthly FinOps review |
| AI quality | groundedness/quality floor on sampled outputs; human escalation path | LLM-eval loop | quality review; feature flag rollback |
| Auditability | every admin/approval/agent action attributed and queryable; evidence exportable on demand | audit log queries by actor | identity design (ADR-003) + quarterly access review |
| Data freshness & quality | consumption datasets meet freshness SLOs; data tests pass before layer promotion | freshness monitors + pipeline test results | data pipeline gate (ADR-016) + SLO review |

Every epic's DoR asks: which rows does this change? A feature that degrades a
row needs an explicit, recorded decision — not a surprise in production.
