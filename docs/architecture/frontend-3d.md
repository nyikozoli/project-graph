# ADR: Use a React application with an optional 3D world

**Status:** Accepted  
**Date:** August 24, 2026

## Context

Project Graph needs conventional operational screens as well as graph and 3D
exploration. The 3D experience must help users understand relationships; it
must not become a separate source of truth or exclude users who need a fast,
accessible work-management interface.

## Decision

Build one frontend using React and TypeScript. Use React Three Fiber and
Three.js for the World visualization. Keep Work, Explore, and World as views
over the same graph API and shared client domain types.

The default experience is a responsive, accessible 2D Work interface. Explore
offers 2D graph, list, and dependency views. World is a progressively enhanced
3D projection that uses the same artifact IDs, relation IDs, filters,
permissions, and selection state as the other views.

The 3D bundle and heavy graph-layout code load only when a user opens World or
an equivalent visualization route. Cap rendering work through level-of-detail,
visible-subgraph limits, instancing where appropriate, and user-controlled
filters. Provide a 2D fallback for every task that has a 3D representation.

Keep presentation-specific layout positions, camera settings, and visual style
separate from domain data. Persist them as optional view preferences or layouts,
not as semantic artifact or relation facts. The server remains authoritative for
graph data and authorization.

Start with a single deployable frontend and a modular codebase. Do not create
micro-frontends for the MVP.

## Consequences

React and TypeScript support a shared component system and type-safe API
contracts. React Three Fiber makes Three.js available without separating the
3D experience into a different application. Lazy loading protects core work
flows from visualization costs.

The team must establish graph rendering budgets and test lower-end devices.
Visual placement can make relationships easier to understand, but it must never
invent a relationship that is absent from the graph. Accessibility, keyboard
navigation, and text alternatives remain required for the primary workflows.

## Alternatives deferred

- A separate Unity, Unreal, or native 3D client is deferred because it would
  duplicate authentication, graph state, and product interaction patterns.
- A standalone 3D-only interface is rejected for MVP because project operations
  require efficient forms, filtering, tables, and accessibility.
- Micro-frontends are deferred. Module boundaries inside one application provide
  enough team separation before independent deployments are necessary.
