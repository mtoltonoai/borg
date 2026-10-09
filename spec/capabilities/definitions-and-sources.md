# Capability — Definitions And Sources

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how the platform reconciles its sessions against the definitions a tenant's sources serve: the
> controller per binding, validation and governance of each version, adoption at turn boundaries by
> cohort, experiments against a parent, and the live-reconfiguration paths that change a running session
> without a redeploy. Requirements realize [Platform Principle P3](../../constitution.md) and
> [Boundary "The Platform Hosts No Programs For Its Tenants"](../../constitution.md) and trace to
> [overview §12](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The source interface and the definition's shape are pinned by
> the [definition-source contract](../contracts/definition-source.md).

## Purpose And Scope

A definition is the desired configuration of one agent identity, and the platform runs sessions against
whatever a tenant's sources serve. This capability fixes the controller's behavior, how a version is
adopted without rewriting the cached prefix, and what happens when a source misbehaves.

## The Controller

### One Controller Per Binding

The platform MUST run one controller per source binding.

A controller MUST read its source's index on connect and on each change.

A controller MUST validate each new version against the schema and the tenant's ceilings.

A controller MUST pass each new version through the input deciders and the control-operation hook.

A controller MUST keep an accepted version under its digest.

A controller MUST reject an invalid version with a reason and keep the previous one.

### The Desired Set Is Materialized

A controller MUST materialize the desired set of sessions its definitions call for.

Each partition MUST reconcile its own sessions against the desired set.

### A Widening Version Defers

A controller MUST refuse a widening version unless the binding is trusted for that class of change, or
defer it to the approver the tenant's governance names.

## Adoption

### Sessions Pin A Digest And Adopt At Turn Boundaries

A session MUST pin a definition version by its digest.

A session MUST adopt a newly accepted version at a turn boundary.

A session's configuration MUST be the definition version it pins, narrowed for a sub-session by its
parent.

### Rollout Is By Cohort

A version marked for a fraction MUST be adopted by the sessions whose ids hash below that fraction.

The platform MUST be able to halt a rollout.

The platform MUST NOT advance a rollout past the fraction a version was marked for.

### Versions Form A Chain And Can Be Compared

A version MUST name its parent, so that a change can run as an experiment against its parent.

An experiment MUST report its significance against the parent on the usage stream.

## Live Reconfiguration

### A Change Applies At The Next Turn Boundary

A newly accepted version MUST apply to a running session at its next turn boundary.

A revocation MUST apply at dispatch, so that a revoked tool cannot run from a turn already in flight.

### A Prefix Change Preserves The Cache

A change to the cached prefix MUST be made through each provider's cache-preserving path rather than by
rewriting the prefix.

A prompt or policy change MUST be applied as a mid-conversation instruction and schedule a compaction
that folds it into the system prompt.

Accumulated prefix changes MUST fold into the prefix at the next compaction.

A field the model never sees MUST apply at once.

### Specs And Versions Are Named By Digest

A spec, a definition, and a change MUST be named by digest.

A spec, a definition, and a change MUST be audited by events.

A spec, a definition, and a change MUST be admitted at the control-operation hook.

## Instancing And Lifecycle

### Instancing Is Singleton Or Keyed

A singleton definition MUST have one session.

A keyed instance's session id MUST be the hash of its tenant, source, definition, and key.

A keyed instance MUST be created on the first event addressed to its key.

A keyed instance's status MUST flow out as events and hooks rather than be written back into a source.

### Lifecycle Follows The Source

An unreachable source MUST change nothing.

A definition missing from the index MUST be held, never retired.

A definition's sessions MUST end only on served retirement or an authorized emergency stop.

A recovered source MUST NOT override an authorized emergency pause or stop without explicit authorized
reconciliation.

## Standing Schedules

### Schedules Come From The Definition

A standing schedule MUST become a timer the definition's sessions can list but not cancel.

A standing schedule MUST be created when an instance starts and ended when the instance ends.

A keyed definition's standing schedule MUST carry a guard.
