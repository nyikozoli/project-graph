# ADR: Use graph-first domain primitives

**Status:** Accepted  
**Date:** August 24, 2026

## Context

Project Graph must connect product intent, planned work, engineering evidence,
and delivery state without imposing a single project-management methodology.
Teams use different terms, work-item hierarchies, workflows, and external
systems. A fixed issue hierarchy or a tool-specific data model would make the
product less useful as soon as a team uses a different process.

The system also needs to answer lineage questions across those boundaries, such
as which requirement led to a deployment, which code or test supports an item,
and why an item is blocked.

## Decision

The domain model is a directed, attributed graph. The graph is the source of
truth; Work, Explore, and World are projections of it.

The MVP defines these primitives:

- **Artifact:** A durable domain object, such as a goal, requirement, work item,
  design, repository, pull request, commit, test run, build, release, or
  deployment.
- **Relation:** A typed, directed connection between two artifacts, such as
  `derives_from`, `implements`, `blocks`, `depends_on`, `validated_by`, or
  `deployed_as`.
- **Evidence:** A verifiable assertion or external record that supports a
  conclusion about an artifact or relation, such as a pull request check,
  linked specification, test result, or deployment record.
- **Event:** A time-bound fact that describes a lifecycle change, import, or
  observation, such as `status_changed`, `synchronized`, or `deployed`.
- **Provenance:** Source and traceability metadata for imported or created data,
  including provider, external identifier, actor where available, timestamps,
  and synchronization context.

Artifact types and relation types are configured data, not application enums
that encode a methodology. The platform supplies starter types, but a workspace
can define its own vocabulary, attributes, relation rules, and optional
lifecycle states. Validation enforces only universally useful invariants: IDs,
workspace ownership, valid endpoints, relation direction, timestamps, and
provenance. It does not require Scrum, Kanban, an epic hierarchy, or any
particular workflow.

Artifacts keep stable internal IDs. External identities are stored in
provenance and mapping records so imports can be synchronized without treating
an external system as the source of truth for the graph.

## Consequences

This model makes cross-tool lineage a first-class product capability and lets
new views use the same underlying data. It also gives integrations a stable
target even when the provider's own object model differs.

The trade-off is that type configuration, relation semantics, and permissions
need clear product design. Views must not infer critical business rules merely
from display labels. Query and API layers must support typed traversal and
property filtering without relying on a rigid hierarchy.

## Alternatives deferred

- A fixed `Epic → Story → Task` hierarchy was rejected because it encodes one
  methodology and cannot represent all delivery evidence cleanly.
- A generic document store without explicit relations was rejected because
  lineage and dependency traversal would be fragile and expensive.
- Full ontology reasoning is out of scope for the MVP. The system uses explicit
  types and rules, with semantics kept understandable for product teams.
