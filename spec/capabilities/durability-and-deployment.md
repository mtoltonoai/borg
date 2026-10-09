# Capability — Durability And Deployment

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how a session survives a crash, a move, a deploy, and a lost cluster: re-drive from committed
> state, a transitional backstop until the durable store is durable on its own, deployment as a cluster
> type, and the alarms and drills that make a restore trustworthy. Requirements realize
> [Platform Principle P5](../../constitution.md), [Platform Principle P6](../../constitution.md), and
> [Core Principle III](../../constitution.md) and trace to [overview §16](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The deployment control plane, the backstop store, the freeze
> cadence, and the epoch form are declared defaults.

## Purpose And Scope

A session's state is durable, and all work is re-drivable from it. This capability fixes what that
guarantee requires of deploys and of cluster loss: a backstop that carries sessions until the store is
durable, an epoch fence so a replaced cluster cannot write back, a deployment path the operating team
already runs, and drilled restores.

## Re-Drive Is The Foundation

### Work Resumes From Committed State

The next owner of a partition MUST resume each session from its committed cursors whenever the
committed cluster state survives.

After total-cluster loss, the next owner MUST resume each session from the cursors recovered within
the last-durable-freeze loss boundary.

An interrupted model attempt MUST run again under its idempotency key.

An interrupted tool call MUST follow its semantics on recovery.

A durable format MUST be versioned so that two versions run side by side during a rollout.

## The Backstop

### A Backstop Carries Sessions Until The Store Is Durable

Until the durable store survives a full-cluster restart on its own, a backstop outside the cluster MUST
hold what a new cluster needs to resume every session.

The backstop MUST hold each session's state and cursors, lease generation, timers, subscriptions,
key-value slot, pending inbox events, in-flight dispatch records, and the names of its frozen blobs.

A transcript MUST stay in the blob store rather than in the backstop.

A backstop record MUST be an envelope under the tenant's keys.

### Records Are Written At Freeze And Shutdown

A session's backstop record MUST be written at each freeze and at shutdown.

A dirty timer change MUST be written to the backstop within the configured interval.

An orderly shutdown MUST lose nothing.

A total-cluster disaster restore MUST lose at most ordinary session progress not covered by the last
durable freeze.

Ordinary session progress acknowledged after that freeze may be lost in this failure domain. This
allowance does not apply to single-node committed-state recovery or to tenant governance records,
which retain their separate durability-before-acknowledgment requirement.

### State That Is Not Per-Session Survives Too

Tenant-level records written by governance-tier calls MUST be written through to the backstop before the
call is acknowledged.

A restore MUST apply tenant-level records before session records, so that it never loses an acknowledged
grant or brings a revoked one back.

Audit events and usage records MUST ship to a sink with its own retention rather than ride the backstop.

A drain MUST flush that outbox before the final records are written.

### The Epoch Fence

A new cluster MUST bump an epoch that every write is conditioned on.

A cluster whose epoch is superseded MUST fail its next write.

A cluster whose epoch is superseded MUST stop its partitions.

The epoch bump MUST be conditional, so that two new clusters cannot both take over.

### The Backstop Is Behind One Interface

The backstop MUST sit behind one narrow interface with a simulator, so that it can be deleted when the
store is durable.

## Drain And Restore

### A Drain Is Orderly

On a drain, the frontend MUST refuse publishes with a retryable status that their ids dedupe.

On a drain, a partition MUST apply what is pending.

A partition MUST freeze after it has applied what is pending.

On a drain, an in-flight model turn MUST get the drain deadline.

A model turn still in flight at the drain deadline MUST be recorded as interrupted.

An interrupted model turn MUST run again after the restore.

On a drain, final records MUST be written before storage terminates.

### A Restore Resumes Every Session

A new cluster MUST restore every session from the backstop or from backup.

A restored session MUST resolve its keys when it next wakes, paced.

A restored recurring timer's missed occurrences MUST follow the missed-occurrence rule at a paced rate.

## Deployment

### The Platform Deploys As Its Own Cluster Type

The platform MUST deploy through the deployment control plane the operating team already uses.

A platform cluster MUST NOT share a cluster with another workload, so that another workload cannot
starve sessions.

Until the control plane patches a cluster in place, a deploy MUST be a drain, checkpoint, new cluster,
and restore.

A deploy that rolls a node's image in place MUST hand that node's partitions off under the fence, so
that no session is paused.

### A Gate Guards Mainline

Every commit that lands on the main line MUST build and pass its tests before any stage can deploy it.

A red main line MUST alarm.

## Operability

### A Node Reports Its Own Health

A node MUST report its own health to the deployment control plane rather than rely on a check that a
crash-looping process passes.

A node MUST withdraw readiness before it drains.

### Alarms Derive From The Platform's Own Events

Alarms and dashboards MUST derive from the platform's own metric definitions.

Every alarm MUST have a runbook.

An alarm on a gauge that must stay at a value MUST treat missing data as breaching.

A node MUST export its resident memory and its allocator's gauges from its first stage.

### Restores Are Drilled

A restore MUST be drilled in a pre-production stage every release, since every deploy and every crash
depends on it.

A full-cluster restart MUST be drilled before the backstop is deleted.
