# Project Graph product decisions

This register captures product decisions that guide implementation but do not
require a standalone architecture decision. Update it when a decision changes
the MVP's user-visible scope or product semantics.

## Active decisions

The following decisions are active for the MVP baseline.

### Graph first

The graph is the source of truth. Work, Explore, World, and the dashboard are
projections. No surface owns a separate project-state model.

### Initial customer

The first customer is a software engineering or product team. The first
end-to-end workflow traces a requirement through a ticket, pull request, test
evidence, DEV or TEST deployment, and release.

### Evidence status

User-facing conclusions distinguish known, inferred, and unknown information.
Unknown means that the graph lacks sufficient information; it does not mean
that delivery has failed.

### Lightweight configurability

Users can define a small custom artifact type with named properties and allowed
relations. The MVP does not include an unrestricted visual schema designer,
arbitrary rule language, or ontology reasoning engine.

### GitHub first

GitHub is the first production provider. GitHub facts retain provenance and
source URLs. Future providers implement only the integration capabilities they
can support.

### 3D as an investigation tool

World exists to help users understand graph relationships and lifecycle state.
It is not a 3D dashboard, a game, or the only route to project operations.

## Decision process

Record a new decision when it changes the product thesis, intended user,
semantic model, MVP scope, or a committed user experience. Create or update an
ADR when the choice also changes a significant technical boundary.

## Next steps

Use the [MVP PRD](prd-mvp.md) for current delivery requirements. Move a
superseded decision to a dated history section when a later decision replaces
it.
