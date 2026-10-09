# Capability — Sessions And Partitions

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines what a session is, how sessions are owned and placed in partitions, how a partition wakes,
> reads, and commits, how an inbox is applied exactly once, and how the durable layout keeps cost
> following the active load. Requirements realize [Platform Principle P5](../../constitution.md) and
> [Core Principle II](../../constitution.md) and trace to [overview §3](../overview.md) and
> [overview §4](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The partition bit count, the transaction key bound, the
> transcript head bound, and the inbox cap are declared defaults.

## Purpose And Scope

This capability fixes the unit the platform schedules and persists (the session), the unit that owns it
(the partition), and the discipline that makes ownership safe under crashes, moves, and network splits.
It states invariants: exactly once, in order, fenced, and re-drivable. The failure table that says what
happens when each dependency misbehaves is the failure-handling capability.

## Sessions

### A Session Is One Continuous Transcript

A session MUST have one continuous transcript for its lifetime.

The platform MUST NOT bound a session's lifetime.

Which sessions exist MUST be decided by the definitions a tenant's sources serve and by sessions starting
sub-sessions, rather than by the core.

### Starting And Ending Are Cheap

Ending a session MUST publish its final observation record.

Ending a session MUST freeze its transcript into the blob store.

Ending a session MUST release its partition resources.

### A Session Id Encodes No Location

A session id MUST be unique across tenants.

A session id MUST NOT encode where the session runs.

A keyed instance's session id MUST be the hash of its tenant, source, definition, and key.

## Partitions

### The Partition Owner Is The Single Writer

Every write to a session's state MUST be made by the owner of the session's partition.

A partition MUST have exactly one owner at a time.

### A Session's Partition Is A Function Of Its Id

A session's partition MUST be determined by the leading bits of a hash of its session id, with the bit
count taken from configuration.

Moving a partition between nodes MUST NOT move a session's durable data.

### The Space Splits By Doubling

Growing the partition space by one bit MUST split each partition into exactly two children.

A session MUST move only from a partition to one of that partition's two children.

### Placement Comes From A Coordinator

Partitions MUST be assigned to nodes by one coordinator per partitioned set, from one consistent view of
membership.

Adding a node MUST move only the partitions the placement policy gives to it.

Removing a node MUST move only the partitions it owned.

The assignment table MUST be durable.

The assignment table MUST record each partition's node and generation.

A node MUST start the partitions it is assigned with the table's generation as their instance
generation.

A node MUST drain the partitions it loses.

A coordinator restart MUST NOT stop a running partition.

A lapsed membership lease MUST cause the lapsed node's partitions to be reassigned.

A planned drain MUST be an input to the placement policy.

### Ownership Is Fenced By Generation

Every partition commit MUST check the partition's generation.

A commit by a superseded owner MUST fail.

Correctness MUST NOT depend on a superseded owner noticing that it was superseded.

## Waking

### A Partition Sleeps On Its Doorbell And Its Deadlines

A partition MUST sleep on the earlier of a watch on its doorbell and its next deadline.

A partition MUST read the wall clock again on every wake.

A partition MUST NOT poll its doorbell or its inboxes.

A doorbell MUST be written once when its partition is created, so that its watch parks rather than
re-issuing.

A doorbell watch MUST be kept alive or renewed within the transport's idle bound.

### A Partition Reads At The Woken Version

A partition MUST read its doorbell and its dirty inboxes in one read-only transaction pinned at the
version its watch woke for.

A partition MUST NOT read its doorbell or inboxes with a stale read that can miss the append that woke it.

### A Commit Validates Only The Fence

A partition's commit of state, cursors, and trims MUST be a blind write that read-validates only the
fence key.

A partition's commit MUST NOT read-validate the doorbell.

## Applying An Inbox

### Each Event Applies Exactly Once, In Order

A publisher MUST append an event to the session's inbox and the session id to the partition's doorbell
in one transaction.

A partition MUST apply each session's inbox in order.

A partition MUST commit a session's new state and its cursor in one transaction.

Each inbox event MUST apply exactly once relative to the session's state.

A slow session MUST delay only itself.

### The Doorbell Advances To A Watermark

The doorbell cursor MUST advance to the position below which every named session has caught up.

Recovery MUST re-read the doorbell from the watermark.

### Fan-Out Appends In Bounded Batches

An append on behalf of fan-out MUST stay within the store's configured transaction key bound.

### A Pending Inbox Is Bounded

The pending inbox of a session that is not draining MUST be bounded by the configured cap.

An inbox past its cap MUST keep a dropped-count marker per topic in place of further events.

A session whose inbox kept a dropped-count marker MUST re-sync from the topic when it drains.

An inbox MUST NOT drop an event silently.

## Work Runs Inside The Partition

### Long Work Commits As State Transitions

A model turn or tool run MUST be a task whose result the partition commits as a state transition.

A crash during a step MUST leave the durable state at the step in progress.

The next owner MUST re-drive a step left in progress.

## Residency

### The Resident Form Is A Cache

A partition MUST load a session when the session has work.

A partition MUST evict idle sessions lowest priority first when it needs room.

Dropping a session's resident form MUST lose nothing.

## Reconciliation

### Each Partition Reconciles Its Own Sessions

Each partition MUST reconcile its own sessions against the desired set the definition controllers
materialize.

A partition MUST create a keyed instance on the first event addressed to its key.

The platform MUST NOT require a global session reconciler.

## Durable Layout

### Cost Follows Active Sessions

An active session MUST occupy no more durable-store keys than the configured per-session key count.

A frozen session MUST be recorded as an element of its partition's index key rather than as keys of its
own.

A partition MUST be the only writer of its index key.

The transcript head held in the durable store MUST be bounded by the configured size.

A queue in the durable store MUST be trimmed once its entries are applied and frozen.

The durable store's working set MUST follow the number of active sessions rather than the total number of
sessions.
