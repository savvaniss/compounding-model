# ADR-006: Data platform — managed PostgreSQL, migrations as code, pgvector first

**Status:** accepted (baseline) · **Category:** data

## Context
Data outlives every service that touches it. The default store, the
migration discipline and the vector-search choice shape the next three
years.

## Options considered
- **One database per microservice, mixed engines** — operational sprawl a
  small team cannot staff. Rejected as a default.
- **NoSQL-first** — schema flexibility you pay back with interest at every
  report and join. Rejected as a default.
- **Managed PostgreSQL (Azure Flexible Server / Amazon RDS / Cloud SQL),
  logical DB per service.**

## Decision
Managed PostgreSQL as the default OLTP store; separate logical databases per
service on shared infrastructure until scale says otherwise. **Migrations
are versioned code, forward-only** — applied by the deploy pipeline, never
by hand; a hotfix that touched the schema manually must be reconciled into
the migration history the same day (unreconciled histories block every
future deploy). Embeddings start in **pgvector** — one store, one backup
story, joins between vectors and business rows; adopt a dedicated vector DB
only on measured retrieval-scale evidence. Backups automated **and restore
tested** on a schedule (see ../operations/sre.md).

## Consequences
Easier: one engine to secure, monitor, back up and price. Harder:
forward-only means "fix forward" discipline — write the compensating
migration, don't fantasize about rollback.
