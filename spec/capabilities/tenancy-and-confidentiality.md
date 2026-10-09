# Capability — Tenancy And Confidentiality

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the tenant as the trust and data boundary, what the substrate commits to now because it is hard
> to add later, and the access scopes that keep a reader inside a tenant seeing only what its sources
> allowed. Requirements realize [Governance Floor "The Tenant Is The Boundary"](../../constitution.md)
> and [Governance Floor "Scopes Are Recorded From The First Event"](../../constitution.md) and trace to
> [overview §10](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The first tenant's name and the scale target are declared
> defaults.

## Purpose And Scope

A tenant holds everyone who could ever see a piece of its data. This capability fixes what names a
tenant, what never crosses one, and how confidentiality works inside one through access scopes. The
cryptographic root of the boundary is the encryption-and-blobs capability.

## The Tenant Is The Boundary

### Everything Names Its Tenant

Every durable record, principal, grant, subscription, and event MUST name its tenant.

A tenant MUST NOT be a metric dimension.

### Authority Stops At The Tenant

A grant MUST cover only its own tenant's sessions and resources.

A topic and an observation queue MUST be named within one tenant.

The platform's own operators MUST reach a tenant's sessions through grants, like anyone else.

### Nothing Derived Crosses

Content derived from one tenant's data MUST NOT reach another tenant.

A content cache MUST be keyed by tenant.

Labels, episodes, and trained models MUST stay within their tenant.

Content shared between tenants MUST be published on purpose rather than leak through a derived store.

### Ids Are Portable

A durable id MUST be unique across tenants.

A durable id MUST NOT encode where it runs.

A principal id MUST name its identity provider, so that adding a provider adds data rather than renaming
principals.

## What Is Committed Now

### The First Tenant Is Bootstrapped

The first tenant MUST be bootstrapped with a stage's first cluster rather than configured.

The platform MUST NOT provide a path to register a tenant until a second tenant needs it.

Every record MUST carry its tenant from the start, even while there is one tenant.

### Keys And Deletion

Each tenant's keys MUST root in a root key dedicated to that tenant, whether its own or one the service holds for it.

Destroying a tenant's root key MUST make the tenant's data unreadable everywhere, backups included.

Data MUST NOT be written under a root key shared between tenants.

### One Usage Record Names Its Tenant

One usage record MUST exist per model attempt, tool call, and decider call, each naming its tenant and
principal chain.

### Bindings Carry The Tenant

Every definition source binding MUST belong to one tenant and have its own credential.

The tenant of a definition, an instance, or a cached read MUST come from the binding rather than from
the payload.

### Retention Is Per Tenant

Retention MUST be tenant configuration, per kind of data.

Audit metadata MUST outlive content.

Deletion MUST be key destruction.

### The Boundary Is Exercised With One Tenant

Simulation scenarios MUST run at least two tenants and assert that nothing crosses between them.

## Confidentiality Inside A Tenant

### Every Inbound Event Carries A Scope

Every inbound event MUST carry an access scope from its source.

### Scopes Propagate As Unions

A transcript range, a compaction, and an observation record MUST carry the union of its inputs' access
scopes.

### A Reader Passes A Scope Check

A consumer or derived store MUST pass a read check against the access scope before it reads.

Delivery to a viewer MUST enforce the read check.

### Scopes Are Recorded From The First Version

An access scope MUST be recorded from the first event the platform accepts, because it cannot be
recovered once content is compacted or distilled.
