# Capability — Timers

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines timers as a native primitive: durable deferred events to a session's own inbox, fired exactly
> once by its partition, guarded so a wake costs nothing when there is no work, and disciplined against
> empty wakes and clock faults. Requirements realize [Core Principle II](../../constitution.md) and
> [Platform Principle P5](../../constitution.md) and trace to [overview §13](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. Minimum intervals, per-session caps, the note size, and the
> clock-skew bound are declared defaults.

## Purpose And Scope

A timer is a deferred event to its own session's inbox. This capability fixes schedules, identity,
exactly-once delivery, guards that avoid empty model wakes, backoff and governance, ownership, storage,
and clock-fault handling.

## Schedules

### Once Or Recurring

A timer MUST be either a one-shot at an absolute time or after a duration, or recurring at an interval
from an anchor or on a cron schedule in a named time zone.

A due time MUST be stored as an absolute instant from the partition owner's wall clock.

A timer MUST NOT fire before its due time by the owner's clock.

A recurring timer MUST compute its next occurrence from its anchor or its last due time, so that a late
fire does not drift it and a clock step cannot replay one.

### Flex And Missed Occurrences

A timer MAY carry a flex that bounds how late an occurrence may be delivered so that it shares a wake.

A definition's schedules MUST spread their fires over their flex by a hash of the session.

An occurrence missed during an outage MUST be delivered late unless a stale limit drops it.

A recurring timer's missed occurrences MUST collapse into one fire that carries their count.

## Identity

### A Timer Is Named Within Its Session

A timer's name MUST be unique within its session.

Setting a name that already exists MUST replace the timer and report that it did.

A timer set without a name MUST derive one from its schedule and note.

A timer MUST carry a note within the configured size, returned with each fire.

Cancel MUST remove a timer or a name prefix.

Re-arm MUST move a timer's next occurrence.

List MUST show every timer with its owner and counts.

## Delivery

### A Fire Is Exactly Once

A fire MUST append an event to the session's inbox, append the session to the partition's doorbell, and
advance the timer, in one commit under the partition's fence.

A fire's id MUST name the timer, its incarnation, and the occurrence.

A retried append of a fire MUST be dropped as a duplicate.

A fire whose timer was cancelled, re-armed, or replaced before the session applies it MUST be dropped.

A fire MUST carry its timer's priority, so that it interrupts or waits like any notification.

A turn a fire starts MUST name the fire as its cause.

## Guards

### A Guard Makes A Wake Cost Nothing When There Is No Work

A timer MAY carry up to the configured number of guard conditions, all of which must hold for a fire to
wake the model.

A guard condition MUST be one of events held for delivery on given topics, no event on given topics
since the timer was set or last fired, a slot key's value or a write to it, or one decision hook from the
card.

A local guard condition MUST be checked before any decision hook, so that a false guard costs no model
call, no load, and no thaw.

A one-shot timer whose guard is false MUST end or wait rather than wake the model.

A recurring timer whose guard is false MUST skip the occurrence.

A fire that waited MUST be checked against its guard again when it is applied.

## Backoff And Governance

### Empty Wakes Are Disciplined

After consecutive empty wakes, a recurring timer's interval MUST stretch up to the configured bound.

A timer set in a turn that a timer caused MUST inherit that timer's empty-wake streak.

An unguarded recurring timer MUST NOT repeat more often than its configured minimum interval.

A session MUST hold no more than the configured number of its own timers.

Timer-caused empty wakes MUST be capped per session per day at the model-request hook.

## Ownership

### A Session Owns Its Own Timers

A session MUST be able to set timers on itself, and a parent on its sub-sessions.

A timer set on a session by another principal MUST require a grant, pass the control-operation hook, and
arrive through the target's inbox, so that the target's partition is the only writer of its timers.

A timer set by another principal MUST be owned and cancelled by its setter.

Such a timer MUST count against a per-owner limit.

A timer MUST NOT be set across tenants.

## Storage And The Deadline Heap

### Timers Live In The Partition's Index

A session's timers MUST live in its element of the partition's index key, so that they add no keys and
stay in place when the session is evicted or frozen.

A timer's timing fields MUST be in the clear, so that the partition schedules, skips, and recovers
without the session's keys.

A timer's name, note, and comparison values MUST be encrypted under the session's keys.

### One Heap Per Partition

A partition MUST keep one heap of its sessions' earliest durable deadlines, covering its timers and the
platform's own deadlines.

A partition MUST sleep on the earlier of its doorbell watch and the heap's head.

A new owner MUST rebuild the heap from one consistent read of the index.

A process-scoped timer that must stop when its process stops MUST stay in memory rather than in the heap.

## Clock Faults

### A Partition Notices A Bad Clock

A wall-clock step of more than the configured bound MUST be an event, noticed within one doorbell
renewal.

An owner whose clock disagrees with the store's commit timestamps by more than the configured bound MUST
hold its timer fires and alert.

## Observability

### Every Timer Event Is Observable

Every set, rejection, guard check, skip, fire, delivery, drop, outcome, and end MUST be an event.

Timer metrics MUST split only by owner kind, schedule kind, condition kind, delivery, residency, and
reason.
