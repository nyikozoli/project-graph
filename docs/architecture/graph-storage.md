# ADR: Store the graph in PostgreSQL

**Status:** Accepted  
**Date:** August 24, 2026

## Context

The MVP needs transactional updates, tenant-safe authorization, configurable
artifact properties, typed relation traversal, auditability, and operational
simplicity. Graph queries are central to the product, but the expected early
scale does not justify another production database or a specialized graph
database.

## Decision

Use PostgreSQL as the sole system-of-record database for the MVP. Model the
graph with relational tables rather than adopting a graph database.

At a minimum, persist these concepts in explicit, workspace-scoped tables:

- `artifacts` stores stable IDs, type, title, lifecycle data, timestamps, and
  common searchable fields.
- `relations` stores source artifact ID, target artifact ID, relation type,
  timestamps, ordering or weight where needed, and relation metadata.
- `evidence`, `events`, and `provenance` store their own first-class records and
  reference the relevant artifact or relation.
- Type definitions, workflow definitions, external identities, and integration
  synchronization checkpoints have separate tables.

Use `jsonb` columns for configured or provider-specific properties that do not
belong in stable relational columns. Promote frequently filtered, sorted, or
authorization-relevant values into explicit columns. Use a GIN index for
appropriate `jsonb` containment queries and ordinary B-tree indexes for IDs,
workspace scopes, types, lifecycle fields, timestamps, and relation endpoints.

Traverse dependencies and lineage with recursive common table expressions. Keep
traversal APIs bounded by workspace, depth, result count, and supported
relation types. Add cycle detection where a relation type has acyclic semantics
and record traversal limits in the API response when relevant.

The application uses migrations, foreign keys where endpoint lifecycle permits
them, and transaction boundaries around multi-record graph changes. A modular
monolith owns database access and domain rules.

## Consequences

PostgreSQL keeps operations simple, supports strong consistency for graph
updates, and provides mature backups, migrations, indexing, and query tooling.
The data model remains inspectable and works well with operational reporting.

Recursive queries and flexible properties require disciplined indexes and query
limits. The team must measure high-cost traversals and add materialized views,
read models, or a search index only when production evidence calls for them.

## Alternatives deferred

- Neo4j or another graph database is deferred. It could be reconsidered if
  measured graph traversal workloads outgrow PostgreSQL.
- Event sourcing is deferred. Events are retained for audit and integration
  history, but they do not replace current-state tables.
- Redis is not introduced for MVP caching or coordination. Add it only for a
  demonstrated latency, throughput, or background-job requirement.
