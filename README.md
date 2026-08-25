# Project Graph

Project Graph is a headless, extensible platform that connects product intent,
planned work, engineering evidence, and delivery in one graph. It gives teams
a shared way to understand how a requirement becomes a release without forcing
them into a fixed project-management methodology.

> **Note:** Project Graph is in the product-definition stage. This repository
> currently contains the implementation baseline rather than an application.

## Product model

The product connects the delivery path below. The graph is the source of truth;
Work, Explore, and World render projections of that graph for different tasks.

```text
Intent -> Work -> Evidence -> Delivery
```

- **Work** is the accessible operational interface for creating, filtering,
  and updating artifacts.
- **Explore** is the 2D interface for relationship, lineage, and dependency
  investigation.
- **World** is an optional interactive 3D view of the same graph.

The initial customer is a software engineering or product team that works
across a product tool, source control, CI, and deployment environments.

## MVP

The MVP proves one complete delivery path:

```text
Requirement -> Ticket -> Pull request -> Tests -> DEV/TEST -> Release
```

GitHub is the first real integration. The platform imports traceable GitHub
facts and lets users investigate evidence and blockers without making GitHub or
any view the source of truth.

## Documentation

Start with the following documents before implementing the product.

- [Product foundation](docs/product/foundation.md) describes the product
  thesis, audience, vocabulary, and boundaries.
- [MVP PRD](docs/product/prd-mvp.md) defines the first vertical slice,
  acceptance criteria, and non-goals.
- [Product decisions](docs/product/decisions.md) records the current
  implementation-facing product decisions.
- [Architecture decisions](docs/architecture/) define the domain, storage,
  frontend, and integration constraints.
- [Contributor guidance](AGENTS.md) defines repository-level instructions for
  people and coding agents.

## Technology direction

The MVP uses a modular monolith, PostgreSQL, and a React and TypeScript
frontend. World uses React Three Fiber and Three.js as a progressively enhanced
view. The backend language and framework are intentionally undecided.

## Next steps

Use the MVP PRD to create the initial repository and service skeleton. Preserve
the ADR boundaries as implementation begins, and propose a new ADR before
expanding the initial architecture.
