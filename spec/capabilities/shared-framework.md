# Capability — The Shared Framework

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the generic components the platform, the collaboration platform, and the workspace service all
> build on, and the discipline that a mechanism is extracted into the framework from working cases rather
> than designed ahead of them. Requirements realize [Core Principle VI](../../constitution.md) and
> [Core Principle I](../../constitution.md) and trace to [overview §21](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. Which store, transport, and object store realize these
> components are declared defaults.

## Purpose And Scope

A generic mechanism belongs in one place that every product shares and that improves with each of them.
This capability fixes the discipline of what goes into the framework and the behavioral obligations of
the components the products depend on. The detailed behavior of a component used by one capability is
stated there; this document states what makes it a shared component and what it must guarantee to more
than one user.

## The Discipline

### A Mechanism Is Extracted, Not Designed Ahead

A mechanism that more than one product needs MUST be a component of the shared framework.

A component MUST be extracted from at least two working cases that use it.

A product MUST NOT keep a second copy of a mechanism the framework provides.

A component MUST have at least two first users from the start.

### A Component Is Deterministic And Honest

A component MUST run under the deterministic simulation framework.

A component MUST have a simulator for each external dependency it calls.

Each such simulator MUST be kept honest by a contract test.

## The Components

### The Partitioned Actor Set

The framework MUST provide a partitioned actor set with one coordinator per set.

The coordinator MUST assign partitions to nodes by a pluggable placement policy.

The coordinator MUST assign from one consistent view of membership.

The assignment table MUST be durable.

A partition's ownership MUST be fenced by generation.

The actor set MUST support draining a node's partitions.

The coordinator MUST stay off the critical path.

### Reads At The Woken Version

The framework MUST provide a read pinned at the version a watch woke for, so that the reader always sees
the append that woke it.

### Deadlines On The Environment

The framework MUST provide a sleep to an absolute instant.

The framework MUST provide a deadline queue with insert, cancel, and next-deadline.

### A Per-Node Wall Clock Under Simulation

The framework MUST provide a per-node wall clock under simulation whose offsets, drift, and steps can be
injected.

### Envelope Encryption

The framework MUST provide one envelope-encryption implementation, generic over the resource and its authenticated data.

The implementation MUST realize the key hierarchy the stored-form contract pins.

The implementation MUST provide a deterministic mode for immutable blobs.

The implementation MUST support a root dedicated to each tenant, whether the tenant's own or one the service holds for it.

The implementation MUST re-wrap a long-lived resource's data key per epoch without a call to the key service.

The implementation MUST provide a key-service client built for the key service's limits.

There MUST be exactly one envelope implementation, never a second copy in a product.

### The Blob Store

The framework MUST provide the owner-scoped blob store, named by the hash of a blob's ciphertext.

The blob store MUST NOT deduplicate across tenants.

The blob store MUST record owners and pins from the first write.

### The Change Feed

The framework MUST provide a change feed as an ordered log.

A change feed's cursor MUST be resumable and fenced by a lease generation.

An external effect a change-feed consumer makes MUST carry an id derived from its position.

The framework MUST provide a resumable form of the change feed over the network.

### The Live-Stream Hub

The framework MUST provide a live-stream hub fed by a change feed.

The hub MUST apply per-viewer filters and access checks.

The hub MUST let a viewer resume from a last-seen id.

The hub MUST send a reset past a bounded horizon.

The hub MUST hold a bounded buffer that drops a slow viewer.

### Rate Limiting

The framework MUST provide keyed rate limiting by tenant, principal, and route class.

Rate limiting MUST answer a throttled caller with a retry-after.

Rate limiting MUST stay bounded under a flood of distinct keys.

### Caller Authentication And Call Guards

The framework MUST provide caller authentication that turns a request into a caller whose identity comes from authentication.

The framework MUST provide call guards that dedupe by call id.

The call guards MUST fence by lease generation.

A call receipt MUST commit in the same transaction as the call's effect.

### Durable Store Durability

The framework MUST provide the durable store with a write-ahead log, a durable snapshot, and a durable index.

The durable store MUST survive a full-cluster restart.

The durable store MUST have a defined backup and restore.

## Evolution

### Components Change Through Their Own Review

A change to a framework component MUST go through the framework's own review.

A component MUST reach a product only through the framework's released version.
