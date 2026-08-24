# ADR: Integrate through capabilities, starting with GitHub

**Status:** Accepted  
**Date:** August 24, 2026

## Context

The product must connect systems that describe intent, implementation, quality,
and delivery. Those systems have different APIs, auth models, rate limits,
webhook behavior, and data vocabularies. A provider-specific core would make
new integrations costly and would leak external constraints into the graph
model.

## Decision

Use capability-based integration adapters behind an internal integration port.
An adapter declares the capabilities it supports rather than the product
assuming all providers offer the same object model.

Initial capabilities include:

- connect and authorize an account or installation;
- discover repositories or other source containers;
- import and reconcile external objects;
- receive and validate change notifications;
- fetch details on demand;
- publish supported links or status updates when explicitly enabled; and
- expose health, synchronization state, and provider errors.

The first production adapter is GitHub. It maps repositories, issues, pull
requests, commits, checks, releases, and deployments into Artifact, Relation,
Evidence, Event, and Provenance records. The mapping preserves the external
identity and source URLs so users can trace every imported fact back to GitHub.

Imports are idempotent and safe to replay. Webhooks act as change signals; a
worker fetches and reconciles authoritative provider data. Each adapter stores
sync cursors, delivery identifiers, error state, and provenance. The core graph
does not depend on GitHub types or APIs.

Run integrations and background reconciliation within the modular monolith.
Use durable database-backed jobs or a simple worker process where needed. Do not
introduce Kafka, Redis, or Kubernetes for the MVP.

## Consequences

Capability boundaries make integrations predictable and let future providers
implement only the functions they can support. GitHub delivers an end-to-end
engineering evidence path early: planned work can connect to code, checks,
releases, and deployments.

The integration layer needs explicit rate-limit handling, retries, dead-letter
visibility, secret storage, permission scopes, and audit logs. Users must see
when data is stale, unavailable, or imported with limited visibility. Adapter
contracts require versioning and contract tests as providers evolve.

## Alternatives deferred

- Direct provider SDK calls from UI or domain modules are rejected because they
  couple the product core to provider behavior.
- A generic bidirectional sync engine is deferred. The MVP prioritizes
  traceable GitHub ingestion and carefully scoped outbound actions.
- Kafka is deferred because the initial event and reconciliation workload fits
  database-backed jobs. Kubernetes is also deferred because a modular monolith
  does not require orchestration complexity at MVP scale.
