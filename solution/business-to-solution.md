# From business need to solution — the front half of the SDLC

A high-quality team is not defined by how it ships but by how it decides
*what* to ship. This is the chain that keeps delivery attached to value.

## The traceability chain (the non-negotiable)
**Outcome → epic → ticket → PR → release → measured effect.** Every link
navigable in both directions. The merge gate enforces ticket-traceability at
the code end; this file covers the business end.

## Intake — one front door
Every request (feature, bug, idea, compliance demand) enters as a ticket in
plain language, from anyone. No side channels. AI assists *refinement* — it
never invents work.

## Discovery & the business case
Before an epic is real: the problem statement in the words of the person who
has it · who benefits and how often · the **value hypothesis** ("we believe X
will change metric Y by Z") · alternatives considered, including "do
nothing" · rough cost (build + run + AI consumption) · risks and reversibility.
One page (templates/business-case-template.md). If it cannot fit on one
page, it is not understood yet.

## Definition of Ready (gate into delivery)
Outcome stated · acceptance criteria testable · NFR deltas identified
(see nfr-catalogue.md) · data/privacy classification done · dependencies
named · sized. The team refuses un-ready work politely and helps make it ready.

## Prioritization & stakeholder governance
A single ordered backlog owned by one accountable person; a recurring
steering cadence where trade-offs are decided with the numbers visible
(delivery metrics, cost, usage). Decisions recorded as ADRs when they bind
architecture, as ticket comments otherwise.

## Benefits realization — closing the loop
Every shipped epic gets a check against its value hypothesis at +30/+90 days
using the product-usage and business-KPI telemetry (see
operations/monitoring-map.md). "Shipped" is not the end state; **"measured"**
is. Features that failed their hypothesis are candidates for removal — code
you delete on usage evidence is maturity too.
