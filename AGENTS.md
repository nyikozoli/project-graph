# Project Graph contributor guidance

Project Graph is a headless, extensible platform for representing project
delivery as a connected graph. This file gives coding agents and contributors
the repository-level constraints that apply until the project adds more
specific guidance.

## Source of truth

The graph is the source of truth. Work, Explore, and World are projections of
the same graph; no view may create a competing model of project state. Read
the product documents and architecture decisions before changing behavior.

- `docs/product/foundation.md` defines the product thesis and durable product
  boundaries.
- `docs/product/prd-mvp.md` defines the initial vertical slice and acceptance
  criteria.
- `docs/product/decisions.md` records product decisions that are not
  architecture decisions.
- `docs/architecture/` contains accepted ADRs that constrain implementation.

## Implementation constraints

Build the MVP as a modular monolith with PostgreSQL as the only system-of-record
database. Use React and TypeScript for the frontend. Load React Three Fiber and
Three.js only for World or another visualization route.

Keep the domain independent from rendering and external provider SDKs. Model
integration data as artifacts, relations, evidence, events, and provenance.
GitHub is the first production integration and must remain behind a
capability-based adapter boundary.

Do not introduce a graph database, Kafka, Redis, Kubernetes, micro-frontends,
or a generic bidirectional synchronization engine without an accepted ADR that
replaces the relevant constraint.

## Working practices

Keep changes small and traceable to a PRD item or ADR. Add migrations for
persisted schema changes, scope reads and writes to a workspace, and preserve
external identifiers and source URLs for imported data. Bound graph traversals
by workspace, depth, result count, and relation types.

Treat evidence as distinct from a user-entered status. UI labels, layouts,
camera positions, and visual styles are presentation data, not graph semantics.
Every World workflow must retain an accessible 2D alternative.

Update the relevant documentation in the same change when implementation
changes an accepted decision, a user-visible capability, or an MVP boundary.
