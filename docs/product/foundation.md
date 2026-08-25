# Project Graph product foundation

Project Graph helps software delivery teams trace a project from intent to
delivery. This document establishes the durable product principles and scope
that guide the MVP and later decisions.

## Product thesis

Project Graph is a headless, extensible project-graph platform. It connects:

```text
Intent -> Work -> Evidence -> Delivery
```

The graph is the source of truth. Work, Explore, and World are projections of
the graph, so a user can investigate the same facts through operational, 2D,
or 3D interfaces without synchronizing separate models.

## Problem

Software delivery information is fragmented across product specifications,
planning tools, source control, CI, testing, deployment tooling, and release
records. Teams can often find each fact in isolation, but they cannot readily
answer where a requirement came from, what implements it, what validates it,
where it is deployed, or what prevents release.

Project Graph addresses connected information rather than replacing every tool
that produces it. It keeps traceable links between internal and imported facts
so users can follow the delivery path and understand its evidence.

## Initial customer and users

The initial customer is a software engineering or product team that uses
multiple systems to deliver a product. The first release serves these users:

- **Product managers** investigate feature progress, dependencies, and release
  readiness.
- **Engineering leads** investigate implementation state, validation, and
  blockers.
- **Developers** trace a ticket to its requirement, pull requests, checks, and
  deployments.

The product does not assume that these teams use a particular methodology,
hierarchy, or vocabulary.

## Product vocabulary

The product uses a small set of graph primitives. Their exact persistence and
integration rules are defined by the architecture ADRs.

- **Artifact** is a durable object, such as a requirement, ticket, pull
  request, test run, deployment, release, or user-defined type.
- **Relation** is a typed, directed connection between artifacts.
- **Evidence** is a verifiable record that supports a claim about an artifact
  or relation.
- **Event** is a time-bound observation or lifecycle change.
- **Provenance** identifies where data came from and how it was observed or
  imported.

An item can be **known** when evidence supports it, **inferred** when a
documented rule derives it from graph facts, or **unknown** when insufficient
information exists. Unknown is not a failure state.

## Product principles

The following principles keep the platform useful across teams and providers.

- Preserve lineage. Users must be able to trace an important conclusion to its
  graph facts and, for imported data, to the original provider record.
- Keep semantics explicit. Relation types, lifecycle rules, and custom type
  definitions must not be hidden in UI labels or visualization layout.
- Support optional process layers. A project can omit a layer such as design
  rather than representing it as failed or incomplete work.
- Separate facts from presentation. Graph data is distinct from layouts,
  dashboards, camera positions, and visual themes.
- Use evidence before assertion. The product must distinguish an observed fact
  from a manual claim or an inference.
- Add extensibility deliberately. Custom types prove workflow flexibility, but
  the MVP does not build a full no-code ontology designer.

## Product surfaces

Project Graph exposes the same graph through three surfaces. A user must be
able to complete operational work without using the 3D surface.

- **Work** provides lists, tables, filters, forms, and detail screens.
- **Explore** provides 2D graph, dependency, lineage, and impact views.
- **World** provides optional 3D lifecycle exploration with an accessible 2D
  fallback.

## Scope boundary

The MVP proves a single engineering-delivery vertical slice and a real GitHub
integration. It is not a general replacement for all planning, documentation,
or DevOps systems. The MVP avoids micro-frontends, a graph database, Kafka,
Redis, Kubernetes, and a generic bidirectional synchronization platform.

## Next steps

Read the [MVP PRD](prd-mvp.md) for the delivery slice and acceptance criteria.
Read the architecture ADRs before choosing implementation details that affect
the graph model, storage, frontend, or integrations.
