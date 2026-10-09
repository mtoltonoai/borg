# Capability — The Workspace Service

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the service that hands a session disposable workspaces for shell, files, and builds: a tool
> server on the card, with the session's identity on every call, a workspace's contents as a value, and no
> standing credentials inside. It is a product built beside the platform. Requirements realize
> [Boundary "The Platform Hosts No Programs For Its Tenants"](../../constitution.md),
> [Governance Floor "Secrets Never Enter The Model's World"](../../constitution.md), and
> [Governance Floor "The Tenant Is The Boundary"](../../constitution.md) and trace to
> [overview §19](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The substrate, the snapshot store, and the credential protocol
> are declared defaults.

## Purpose And Scope

Coding roles need somewhere to run code, and the platform hosts none. This capability fixes the service's
guarantees: it is a tool server like any other, a workspace is disposable because its contents are a
value, credentials are per call, and isolation holds across tenants. How it provisions hosts is its own.

## A Service On The Card

### It Is A Tool Server, Not Part Of The Platform

The service MUST be called as a tool server on an agent's card, with the session's identity on every
call.

The platform MUST NOT know what the service does beyond calling its tools.

The service MUST verify each call's assertion and its lease generation.

The service MUST refuse a call whose lease generation is older than the highest it has recorded for the
session, with a distinct error.

The service MUST run a call id it has a receipt for at most once.

### Operations

The service MUST let a session acquire a workspace, run commands in it, operate on its files, and release
it.

A workspace MUST be addressed by a handle that works only for the session that holds its lease.

A lease MUST belong to the service and expire unless used, so that the platform needs no eviction hook.

## A Workspace's Contents Are A Value

### A Snapshot Follows Every Changing Call

The service MUST snapshot a workspace's contents, content-addressed, after every call that changed them.

A session MUST be able to continue from its last snapshot on another host after its host is lost.

A snapshot MUST chain to the previous one, so that it stores only what changed.

The next call on a workspace MUST wait for the previous snapshot's upload, so that a lost host loses at
most the call in flight.

A restored tree MUST hash back to its snapshot.

### Swap, Fork, And Rollback

A swap MUST restore a snapshot onto any host.

A fork MUST start a new lineage from a snapshot and pin it.

A rollback MUST make an earlier snapshot the head as a new snapshot, so that a lineage only grows.

### Offline Is An Event

A lost host MUST produce one event into the holding session, naming the handle and the last durable
snapshot, published through the platform's API.

## No Standing Credentials

### Credentials Are Per Call

A workspace-local endpoint MUST serve each call's credentials only while that call runs.

A call's processes MUST be killed when the call ends.

The instance metadata path MUST be unreachable from a tool.

Credentials MUST come from the credential broker in exchange for the call's assertion.

A log, event, or saved output MUST NOT carry a secret or an endpoint token.

## Isolation

### A Workspace Is Isolated And Bounded

A host MUST run one tenant's workspaces only.

A tool MUST run unprivileged under resource limits.

All egress MUST leave through one point the service controls.

A build cache MUST be shared within a tenant and never across tenants.

### Placement And Clearance

A workspace type MUST carry its network placement, so that a tool needing a particular network runs on a
workspace placed there.

A session MUST hold several named workspaces at once, each addressable and with its own snapshot lineage.

A session's own workspaces MAY reach each other.

A workspace MUST NOT reach another session's workspaces.

Acquiring a workspace for a repository MUST require the session's clearance for the repository's domain.

## Snapshot Ids

### Snapshot Ids Are Settled Before Transcripts Cite Them

Whether a snapshot id put into a result or event is a plaintext content hash or an opaque id MUST be
settled before transcripts cite it.
