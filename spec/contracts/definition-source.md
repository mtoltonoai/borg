# Contract — The Definition Source

> **CONTRACT.** This document pins the interface through which a tenant's service serves agent
> definitions to the platform, and the shape of a definition itself. It is the interface the first
> adapter implements as a server and the platform implements as a client, and it is honored by
> both across releases, since the two ship through separate pipelines. Its requirements realize
> [Boundary "The Platform Hosts No Programs For Its Tenants"](../../constitution.md) and trace to
> [overview §12](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The canonical serialization, the digest function, and the long-poll bound are declared
> defaults.

## Purpose And Scope

A definition is the desired configuration of one agent identity, and a tenant serves its definitions
from its own sources under the change control it already has. This contract fixes what a source serves
(an index, immutable versions, and a change feed), what a definition contains, and what it never
contains. How the platform validates, governs, and adopts a version is the definitions capability.

## The Interface

### A Source Is Bound To One Tenant

A source MUST serve definitions for exactly one tenant per binding.

The tenant of a definition MUST come from the binding rather than from the served payload.

A source MUST serve this contract over its well-known interface rather than over the tool protocol.

### The Index Is A Consistent Snapshot At A Rising Revision

A source MUST serve an index that is a consistent snapshot of every definition it serves, at one
revision.

A source MUST NOT serve a revision lower than one it has served before.

An index MUST carry a validator that lets a client ask only whether the revision has changed.

A source MUST support a long-poll on the index that returns when the revision changes or when the
configured bound elapses.

An index entry MUST name the definition, the digest of its current version, and its lifecycle intent.

### A Version Is Immutable And Addressed By Digest

A version MUST be addressed by the digest of its canonical serialization.

A source MUST serve the bytes of a version by its digest for as long as any session may pin it.

A version MUST NOT change once it has been served.

A version MUST name the digest of its parent version, or state that it has none.

### The Change Feed Resumes From Any Revision

A source MUST serve a change feed that resumes from any revision the client names.

A change feed MUST answer a distinct gone status for a revision inside a range the source lost to a
restore, so that the client relists the index.

A source that restores from backup MUST move its revision past the highest it ever served before serving
again.

### A Source May Announce Its Revision

A source MAY publish its new revision through the platform's event envelope as well as through the feed.

## The Definition

### What A Definition Carries

A definition MUST name its lifecycle intent as one of run, paused, or retired.

A definition MUST name its instancing as singleton, keyed by requester, or keyed by a publisher's key.

A definition MUST carry its prompt components, model and parameters, tool server references, decider
additions, budget caps, deadlines, priority, standing subscriptions, standing schedules, compaction
settings, cache settings, observation settings, and hook destinations by name.

A definition MUST reference a tool server by a name registered with the tenant rather than by an
arbitrary address.

A definition MUST reference a hook destination by a name registered with the tenant rather than by an
arbitrary address.

A definition MUST name a key only as a label drawn from the tenant's permitted set.

### What A Definition Never Carries

A definition MUST NOT name a principal.

A definition MUST NOT carry a grant.

A definition MUST NOT carry a credential or a secret.

### A Definition Only Restricts

A definition's own deciders and policies MUST only restrict what the tenant's governance allows.

A definition's base priority MUST NOT exceed its binding's ceiling.

### Tools That Write Definitions Declare It

A tool that writes a definition MUST declare the definition-write call type on its spec.

## Lifecycle Semantics

### Only A Served Retirement Ends Sessions

A client MUST treat a definition missing from the index as held rather than retired.

A client MUST end a definition's sessions only when the source serves the definition as retired.

### Pause Is Served, Not Inferred

A source MUST serve a paused intent as the only way to pause a definition's sessions.

## Conformance

### One Suite Runs Against Both Sides

A conformance suite for this contract MUST run against a source implementation and against the
platform's client.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed sources and clients, or else carry
an explicit version increment.

A change to this contract that is not additive with respect to deployed sources and clients MUST carry
a stated migration path.
