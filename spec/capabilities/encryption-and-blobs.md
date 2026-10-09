# Capability — Encryption And The Blob Store

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines encryption of sessions at rest, keeping the key service off the turn's critical path, and the
> owner-scoped blob store that holds frozen transcripts and large outputs. Requirements realize
> [Core Principle V](../../constitution.md) and [Governance Floor "The Tenant Is The
> Boundary"](../../constitution.md) and trace to [overview §11](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The stored form is pinned by the
> [stored-form contract](../contracts/stored-form.md); the cipher modes, the hash, and the cache lifetime
> are declared defaults.

## Purpose And Scope

Encryption makes the tenant boundary a cryptographic fact and makes deletion mechanical. The blob store
holds everything too large for the durable log. This capability fixes the behavior around the stored
form: how keys resolve without stalling a turn, how a blob is owned and collected, and how deletion
reaches every copy.

## Encryption At Rest

### Sessions Are Encrypted Under An Envelope

Every session's values and blobs MUST be encrypted under the envelope the stored-form contract pins.

A node MUST be able to read a session with at most one call to the key service.

A plaintext key MUST live only in a per-node cache whose lifetime is the revocation window.

A component that cannot resolve a key MUST fail closed.

### The Key Comes From A Trusted List

The root key for a session MUST be named in the session's configuration as a label.

The label MUST be checked against the node's trusted list of permitted keys.

A value read from a store MUST NOT choose the key.

### The Key Service Stays Off The Critical Path

A session's keys MUST resolve when its doorbell wakes it, before a turn needs them.

The decrypt cache MUST refresh a key ahead of its expiry, so that a revocation still lands within the
window.

A key-service call that must be waited on MUST be hedged and bounded by a short deadline.

A connection to the key service MUST be kept warm only where calls come often enough to hold it open.

Resumes after a failover MUST be paced, so that a surge of sessions does not exceed the key service's
request quota.

### A Long-Lived Session Re-Wraps Per Epoch

A session that outlives an epoch MUST re-wrap its data key under the new epoch's branch key without a
call to the key service.

## The Blob Store

### A Blob Is Owned And Immutable

Every blob MUST have one owner and be stored under its tenant and owner.

A blob MUST be immutable once written.

The owner's partition MUST be the only writer of a blob's references and deletions, so that a new
reference cannot race a delete.

### A Blob Is Named By Its Ciphertext

A blob MUST be named by the hash of its stored ciphertext.

A name MUST let the store and its caches verify a blob without any key.

A name MUST reveal nothing about the blob's plaintext.

### Sharing Is An Explicit Pin

Sharing a blob across owners MUST be an explicit pin with a holder and an expiry, recorded through the
owner before the reference is used.

Ownership MUST transfer only through the current owner.

A blob MUST NOT be deduplicated across tenants.

Deduplication inside a tenant, where it is shown to pay, MUST be a tenant-owned class named by a keyed
hash under a tenant key.

### Collection Comes Later Without Migration

The first version of the blob store MUST collect nothing.

Owners and pins MUST be recorded from the first write.

Reclaiming bytes MUST run late and conservatively rather than on the path of a write.

### Deletion Is Key Destruction

Deletion MUST come from destroying keys.

Destroying a key MUST reach the blob store, caches, and backups.

The node cache MUST hold only ciphertext, so that destroying a key revokes cached copies within the
decrypt cache's window.

## Platform-Shared Content

### The Catalog Owns Shared Content

Platform-shared content such as sandboxed modules, model weights, and templates MUST be a small set the
catalog owns and pins by version.
