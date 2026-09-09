# Architecture practice

Architecture is a continuous stream of decisions under uncertainty, not a
document phase before the build. The architect owes the project four
things: quality attributes that hold in production, boundaries that survive
team and scale changes, a decision log that explains itself, and structure
that is *enforced*, not aspired to.

## Design starts from quality attributes, not boxes

Structure is decided by the [NFR catalogue](../solution/nfr-catalogue.md),
not by fashion. For each significant capability, write its top three
quality-attribute scenarios as measurable sentences — "P95 below 800ms at
the expected peak concurrency", "a region outage degrades to read-only in
under five minutes", "a tenant's data is provably invisible to another
tenant" — and let those drive the design. Features decide code;
characteristics decide structure. A design review that never mentions a
number is a drawing session.

## Boundaries follow the domain, never the org chart or the tech stack

- Cut along **bounded contexts**: a boundary is right when the language
  inside it is consistent and the data inside it has one owner. No shared
  database across service boundaries — shared tables are a distributed
  monolith with extra latency.
- **Start with a modular monolith** with enforced internal seams; extract a
  service only on measured pain (independent scaling, independent release
  cadence, team contention) — never because microservices are expected.
  Extraction is cheap when the seam already exists; the seam is the
  architecture, not the deployment unit.
- Synchronous calls only for answers a user is waiting on; everything else
  goes through a queue or an event (ADR-011). Third-party edges get an
  anti-corruption layer: their model stops at the adapter, and the exit
  cost stays a refactor instead of a rewrite.

## Trade-off analysis is the job

"Best practice" is the *end* of an argument, never the start. Every
significant decision names: the axis being traded (consistency vs
availability, cost vs latency, build vs buy, time-to-market vs exit cost),
the point chosen on it, and **the signal that would move the point** —
which becomes a monitoring-map entry, so the decision re-opens itself on
evidence rather than on anecdote.

Sort decisions by reversibility. Two-way doors (a library, an internal
schema, a retry policy) get a PR and a paragraph. One-way doors (platform,
data topology, identity model, anything involving data migration at scale)
get an ADR, a design review, and are taken at the **last responsible
moment** — deferred while deferral is cheap, decided before the options
start closing.

## Design for failure, explicitly

For every arrow in the container diagram, the answer to "what happens when
the far end is down or slow" must exist in writing:

- Every remote call: a timeout, a retry policy with backoff and jitter, and
  a budget (retry storms take systems down more often than the original
  fault).
- Every dependency that can brown out: a circuit breaker and a designed
  degradation — "AI unavailable" hides the feature or serves the cached
  answer; it never becomes a stack trace in the user's face.
- Every queue: a dead-letter path and a replay procedure that has been run
  at least once.
- Every handler that can be retried: idempotent (ADR-011) — retries are a
  fact of queues, of networks, and of AI agents re-driving tools.
- Blast radius stated per component: what stays up when this is down. If
  the honest answer is "nothing", that is a finding, not a footnote.

## Fitness functions — structure that enforces itself

The blueprint's core rule ("a rule not enforced at a tool boundary is a
wish") applies to architecture first:

- Dependency direction checked in CI (import/module lint): domain code
  never imports infrastructure; contexts don't reach into each other's
  internals.
- Performance and payload budgets asserted in the pipeline on the hot
  paths, not discovered in production.
- Drift (ADR-002), SLO burn (ADR-009), and unpriced-model alerts (ADR-010)
  are architectural fitness functions too — they verify that the built
  system still matches the decided one.

A principle without a check is an opinion.

## Decision records (ADRs)

Any decision that binds future work gets an ADR
(../templates/adr-template.md): context, options, decision, consequences,
review date — gated like code and **linked from the code it constrains**.
An undocumented architecture decision is a future incident with extra
steps. Do not start from zero: [decisions/](../decisions/) seeds sixteen
pre-accepted baseline ADRs, one per category; disagreement is a superseding
ADR with your context, never silent drift.

## Views that stay true

Exactly three living diagrams (more will rot): **system context** (who and
what talks to us), **container view** (deployable units and data stores —
the same view the failure-design section interrogates), **deployment view**
(environments, networks, identities). Diagrams as code, regenerated or
reviewed each release; a diagram older than the latest architectural ADR is
wrong by definition.

## Threat modeling and technology intake

A new capability or trust-boundary change gets a 30-minute STRIDE-lite pass
with notes committed next to its ADR (details in
[security/secure-delivery.md](../security/secure-delivery.md)). New
technology — including AI models, which are dependencies with invoices —
enters through a one-page assessment (problem, exit cost, security/data
posture, maintainer) recorded as adopt/trial/hold in the repo.

## The architect's cadence

- **Per PR that touches a boundary**: review by asking, not decreeing —
  *what breaks first at 10× load? where does this data live and who can
  read it? which NFR row does this serve? what is the exit cost?*
- **Per release**: diagram and ADR sweep; anything the release contradicted
  gets updated or superseded.
- **Monthly**: NFR/SLO review with operations, cost review against the
  ledger — trade-off signals checked against their thresholds.
- **Quarterly**: the adopt/trial/hold record from technology intake, and
  this document itself. An architecture
  practice that hasn't changed in a year isn't stable, it's unattended.
