# Knowledge & work integration — the agent knows the project; every action has a ticket

Two integrations decide whether an AI agent is a colleague or a very fast
stranger. **An agent without project knowledge is useless**: the repository
says *how*, but the documentation of record says *what and why*, and the
ticket history says *who decided and when*. And **an action without a
ticket never happened**: untracked work is invisible to the team, the
metrics and the audit alike.

## The knowledge plane

The agent reads the same sources the team does, live, through the tool
server (one socket — baseline/factory.md):

| source | carries | integration |
|---|---|---|
| documentation of record (wiki) | platform decisions, domain knowledge, org procedures | live search + read tools — never a stale copy |
| work tracker | what happened, who decided, current state | search + read tools |
| factory repo | how *this team* operates: runbooks, standards, lessons | read tools, size-capped pages |
| code | how it actually works | indexed search + file read |
| telemetry | what is true right now | query tools |

Rules that keep it honest:
- **One federated search** across all sources — the agent (and the human)
  asks once; each silo answering separately is how context gets missed.
- **Read live, never mirror.** A copied wiki is stale the week after; the
  tools fetch the page of record at ask time.
- **Read before acting** is contract, not courtesy: an agent starting a
  task searches the knowledge plane for prior decisions, related tickets
  and the relevant runbook first (CLAUDE.md).
- **Writing back is attributed and conservative**: page updates carry the
  person's identity, replace whole bodies only with a version check, and
  land in the documentation of record only when a person asked.

## The work plane — the ticket is the spine

- **Every action traces to a ticket.** The gate refuses branches and PR
  titles without one; the delivery orchestrator reports every build,
  deploy and verification **as a comment on the ticket**, with links. Work
  a stakeholder cannot find from the ticket did not happen.
- **Agents comment, attributed** — progress, outcomes, validation guides —
  as the person driving them.
- **Agents never open tickets unasked.** The backlog belongs to the team;
  an agent inventing work items is scope creep with an API key. Ticket
  creation is a tool that requires an explicit human ask.
- The ticket is also the **memory join**: cost rows, gate verdicts and
  delivery runs all carry the ticket id, so "what did this feature cost to
  build" is a query.

## The mapping

| concept | Atlassian | Microsoft | GitHub | GitLab |
|---|---|---|---|---|
| work tracker | Jira | Azure Boards | Issues/Projects | Issues/Epics |
| documentation of record | Confluence | SharePoint / ADO wiki | wiki / pages | wiki / Pages |
| integration surface | REST + webhooks | REST + service hooks | REST + GraphQL | REST + GraphQL |

The invariants never change with the vendor: live read, federated search,
attributed comments, ticket-traceability enforced at the gate, no
agent-created tickets without a human ask.
