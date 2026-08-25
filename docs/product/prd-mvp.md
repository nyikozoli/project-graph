# Project Graph MVP PRD

This PRD defines the smallest implementation-ready vertical slice for Project
Graph. The MVP must prove that a graph with traceable evidence makes software
delivery easier to understand than disconnected tool views.

## Objective

The MVP lets a user create and investigate a connected delivery path from a
requirement to a release. It proves three hypotheses:

- A connected graph improves lifecycle and blocker comprehension.
- Lightweight configuration supports different team vocabularies without
  forcing a methodology.
- An optional 3D view can improve relationship comprehension without replacing
  accessible operational views.

The first successful demonstration answers, with evidence, "Why is this work
not released?"

## In scope

The MVP delivers one workspace-scoped project template and one GitHub-backed
engineering-delivery slice.

```text
Requirement -> Ticket -> Pull request -> Tests -> Deployment -> Release
                                      |              |
                                      +-> Evidence   +-> DEV or TEST
```

The template supplies these initial artifact types:

- `requirement`
- `ticket`
- `pull_request`
- `test_run`
- `deployment`
- `environment`
- `release`

The template supplies the following directed relation semantics. The left-hand
artifact is the relation source, and the right-hand artifact is the target.

- `ticket implements requirement`
- `pull_request implements ticket`
- `pull_request validated_by test_run`
- `pull_request deployed_as deployment`
- `deployment deployed_to environment`
- `deployment released_as release`
- `artifact blocks artifact`
- `artifact depends_on artifact`

An implementation may add a specification artifact when users need it, but it
must not expand the initial delivery slice with additional mandatory layers.

## Required capabilities

The MVP exposes a shared graph API or equivalent service boundary that supports
the capabilities below. The transport and backend framework are implementation
choices; the graph semantics are not.

### Project setup and configuration

A user can create a workspace-scoped project from the engineering-delivery
template. The project starts with the listed artifact types, relation types,
and semantic lifecycle rules. The product must not require Scrum, Kanban,
epics, or a design layer.

A user can add one lightweight custom artifact type. The configuration includes
a name, a small set of typed properties, and allowed relation types. The type
can participate in the same graph and surfaces as the template types.

### Graph data and provenance

The service persists artifacts, relations, evidence, events, and provenance as
first-class, workspace-scoped records. It assigns stable internal identifiers
and preserves provider, external identifier, source URL, timestamps, and sync
context for imported facts.

All graph reads and traversals must be bounded by workspace, relation type,
depth, and result count. A result that reaches a limit must state that it is
truncated rather than imply that the graph has ended.

### GitHub integration

A user can connect GitHub, select a repository, and import or reconcile its
relevant objects. The first adapter maps repositories, pull requests, commits,
checks, workflow runs, releases, and deployments into graph records where the
provider data supports them.

The integration must be idempotent and safe to replay. Webhooks act as change
signals, and reconciliation fetches authoritative provider data. The MVP must
show synchronization health, last successful synchronization, and provider
errors. It does not require outbound updates to GitHub.

### Evidence and lifecycle conclusions

The product renders a conclusion as one of the following states:

- **Known** when verifiable evidence supports the conclusion.
- **Inferred** when a documented rule derives the conclusion from graph facts.
- **Unknown** when required facts are absent, stale, or unavailable.

For the first delivery slice, the release-readiness view evaluates whether a
selected requirement has a connected ticket, pull request, successful test
evidence, deployment to the required environment, and release evidence. It
lists each missing, blocked, unknown, or stale link and exposes the supporting
records for each positive conclusion.

### Work, Explore, and World

Work provides an accessible 2D list and detail experience for artifacts,
relations, evidence, filtering, and the custom type. Explore provides a 2D
focused-subgraph view for a selected artifact, its lineage, dependencies, and
blockers.

World provides an optional 3D focused-subgraph view of the same artifact IDs,
relations, filters, permissions, and selection state. It supports selection,
focus, relation expansion within traversal limits, and details without leaving
the view. Every World task must have an equivalent 2D flow. Load the 3D code
only when the user opens World.

## Primary journeys

The following journeys define the MVP's minimum usable behavior.

### Create a project and custom type

1. Create a project from the engineering-delivery template.
2. Verify that the template exposes the initial graph vocabulary.
3. Create an `architecture_decision` custom type with `status` and `owner`
   properties.
4. Create an architecture decision and relate it to a requirement or ticket.
5. Confirm that Work and Explore render the custom artifact and relation.

### Connect GitHub and inspect evidence

1. Connect a GitHub account or installation with the required repository access.
2. Select a repository and start an import or reconciliation.
3. Open an imported pull request and inspect its source URL, external identity,
   checks, and synchronization status.
4. Follow the pull request's relations to its ticket and test evidence.

### Investigate release readiness

1. Open a requirement in Work, Explore, or World.
2. Select the release-readiness view.
3. Inspect the path from requirement to ticket, pull request, tests,
   deployment, environment, and release.
4. Identify the first missing, blocked, unknown, or stale part of the path.
5. Open its supporting artifact, evidence, or provider record.

## Acceptance criteria

The MVP is ready for an internal demonstration when it meets all of the
following criteria.

- The project template creates the defined types and relation semantics.
- A user can create, relate, and find an artifact of the custom type.
- A GitHub import can be safely replayed without duplicating graph records.
- An imported pull request retains provenance and links to its source record.
- A focused requirement view shows a bounded path to related delivery
  artifacts, or clearly identifies missing and unknown links.
- The release-readiness view identifies why the selected requirement is not
  released and identifies supporting evidence for each known conclusion.
- Work and Explore provide the required workflow without WebGL or 3D support.
- World displays the same selected graph facts and does not create semantic
  relationships from layout alone.
- The application retains workspace isolation and applies authorization before
  returning graph data.

## Measures

The internal MVP evaluation records whether users can trace a requirement to
release, find the reason for an unreleased item, and distinguish evidence from
inference without consulting another tool. Capture task completion, time to
identify the blocker, and qualitative feedback on Work, Explore, and World.

## Out of scope

The following are explicitly outside the MVP:

- Full replacement of planning, documentation, CI, deployment, or incident
  tools.
- Integrations other than GitHub, except development fixtures or test adapters.
- Generic bidirectional synchronization and outbound GitHub updates.
- A full no-code ontology designer, arbitrary workflow engine, or ontology
  reasoning.
- Graph databases, event sourcing as the current-state model, Kafka, Redis,
  Kubernetes, micro-frontends, and a separate 3D application.
- A 3D-only workflow, gamified world, or presentation-first dashboard.

## Implementation constraints

Implement the MVP as a modular monolith with PostgreSQL as the only
system-of-record database. Use React and TypeScript for the frontend, and use
React Three Fiber with Three.js only for the progressively enhanced World
surface. Keep provider SDKs behind capability-based adapters and keep domain
semantics independent of rendering.

## Next steps

Create an implementation plan that maps each required capability and acceptance
criterion to a module, migration, API contract, and test. Propose an ADR before
changing any technical boundary established in `docs/architecture/`.
