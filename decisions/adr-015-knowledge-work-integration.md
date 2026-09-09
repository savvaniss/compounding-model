# ADR-015: Knowledge & work integration — live tools, ticket as the unit of work

**Status:** accepted (baseline) · **Category:** knowledge & work

## Context
An agent that sees only the repository acts without the project's memory:
platform decisions live in the wiki, history and intent live in tickets.
And agent work that is not tied to a ticket is invisible — unauditable,
unmeasurable, unfindable.

## Options considered
- **Repo-only agent context** — fast to set up; the agent re-derives or
  invents what the wiki already settled. Rejected.
- **Mirror everything into a vector store** — stale within days, and the
  copy becomes an unaudited second home for restricted content. Rejected
  as the primary mechanism.
- **Live tool integration through the tool server.**

## Decision
The documentation of record (Confluence / SharePoint / wiki) and the work
tracker (Jira / Azure Boards / Issues) are integrated as **live tools** on
the tool server: federated search, page/issue read, attributed comment;
page creation and updates only on explicit human ask, with version checks.
The **ticket is the unit of work**: the gate refuses untracked branches
and PRs, the orchestrator reports every outcome on the ticket, telemetry
and cost rows carry the ticket id — and **agents never create tickets
unasked**. Full standard: baseline/knowledge-work-integration.md.

## Consequences
Easier: the agent answers with the project's actual memory; "what
happened on this feature" is one ticket thread with links; audits read
themselves. Harder: tool-server auth to two more systems (per-person
where the API allows it), and search quality across sources needs real
ranking — budget for it in utility #2.
