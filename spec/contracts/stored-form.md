# Contract — The Stored Form

> **CONTRACT.** This document pins the form in which a tenant's data rests: the envelope every durable
> value and blob is wrapped in, how a blob is named, and how formats evolve. Data written today must be
> readable, deletable, and recoverable by every later release, so this is an irreversible choice decided before
> the first durable record. Its requirements realize [Core Principle V](../../constitution.md) and
> [Governance Floor "The Tenant Is The Boundary"](../../constitution.md) and trace to
> [overview §11](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The cipher modes, the hash function, the compression, and the epoch length are declared
> defaults.

## Purpose And Scope

Two properties make deletion and recovery mechanical: every ciphertext carries what is needed to decrypt
it given the tenant's root key, and nothing is readable under any other key. This contract fixes the
envelope, the key hierarchy as the stored form sees it, blob naming, the fields that stay in the clear,
and format versioning.

## The Key Hierarchy

### Keys Descend From The Tenant's Root

Every stored value of a tenant's content MUST be encrypted under a data key that descends from that tenant's root key.

A branch key MUST be derived per tenant per epoch under a service-held root, or per resource under a
tenant-held root.

A data key MUST be random per resource.

### A Resource Carries Its Own Wrapped Keys

A stored resource MUST carry its own wrapped keys, so that any node can read it with at most one call to
the key service.

A long-lived resource's data key MUST be re-wrappable under a new epoch's branch key without a call to
the key service.

### The Tenant Is In The Encryption Context

Every envelope's encryption context MUST name the tenant.

## The Envelope

### Authenticated Data Binds The Envelope

Every envelope MUST carry authenticated data that binds it to its tenant, its owner, and its range.

A ciphertext copied to another owner or range MUST fail to decrypt.

### Values And Blobs Use Authenticated Encryption

A stored value MUST be encrypted under an authenticated mode.

An immutable blob MUST be encrypted under a deterministic authenticated mode, so that a retried write
produces the same bytes under the same name.

## Blobs

### A Blob Is Named By Its Stored Bytes

A blob MUST be named by the hash of its stored ciphertext.

A blob's name MUST reveal nothing about its plaintext.

A store or cache MUST be able to verify a blob's integrity from its name without holding any key.

### A Blob Carries Its Key Chain

A blob's header MUST carry its wrapped key chain, so that the blob store and the key service alone can
recover a session's history.

### A Blob Is Immutable And Owned

A blob MUST NOT change once written.

A blob MUST be stored under its tenant and its owner.

### Frozen Ranges Are Compressed Before Encryption

A frozen transcript range MUST be compressed before it is encrypted.

## What Stays In The Clear

### Scheduling Fields Are Readable Without Keys

A timer's timing fields are operational metadata rather than tenant content, so the obligation to
encrypt a tenant's content does not reach them.

A timer's timing fields MUST be stored in the clear, so that a partition schedules and recovers without
the session's keys.

A timer's name, note, and comparison values MUST be encrypted under the session's keys.

## Format Versioning

### Every Durable Format Is Versioned

Every durable format MUST carry a version.

Two consecutive versions of a durable format MUST be readable side by side during a rollout.

### Decode Never Swallows

A decoder MUST distinguish an absent value, a valid value, and a corrupt value.

A decoder MUST NOT overwrite a value it could not decode.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to already-written data, or else carry an
explicit version increment.

A change to this contract that is not additive with respect to already-written data MUST carry a stated
migration path.
