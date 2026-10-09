# Capability — Failure Handling

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the one handling each failure class has and the state a session ends in, the deadlines and
> budgets that bound every retry, quarantine, and the rule that every stuck state has a way out.
> Requirements realize [Platform Principle P6](../../constitution.md) and trace to
> [overview §16](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. Retry budgets, backoff schedules, and attempt counts are
> declared defaults.

## Purpose And Scope

The durable state machine is the source of truth, all work is re-drivable from it, and every side effect
carries an idempotency key. Given that, each failure a dependency can present has exactly one handling,
so the system's behavior under failure is a table rather than a judgment. This capability is that table.

## Foundations

### Every Side Effect Can Be Redone

Every side effect MUST carry an idempotency key.

A model attempt's idempotency key MUST be its attempt id.

A tool call's idempotency key MUST be its call id.

### Every Unit Of Work Has A Deadline

Every model turn MUST have a deadline.

Every tool call MUST have a deadline.

### Every Retry Has A Budget

Every retry MUST be bounded by a budget.

Exhausting a retry budget MUST end in a defined blocked state that carries the reason.

## The Failure Table

### A Throttled Or Unavailable Model

A model attempt that is throttled, overloaded, timed out, or failed internally MUST be retried with
backoff and jitter within the turn's attempt budget.

A retry of a throttled model attempt MUST honor the provider's retry-after when one is given.

The platform MUST keep a circuit breaker per model and endpoint.

A session whose turn exhausts its attempt budget against a model MUST end blocked with the reason.

### A Context Window Exceeded

A model attempt that exceeds the context window MUST cause a compaction followed by a retry of the turn.

### An Invalid Request

A model attempt rejected as invalid MUST NOT be retried.

A session whose request was rejected as invalid MUST end blocked, with an event carrying the reason.

### An Authentication Failure Toward A Dependency

An authentication failure toward a dependency MUST be handled once per cluster, with backoff and an
alert, rather than once per session.

### A Policy Block

A request blocked by policy MUST NOT be resent unchanged.

A session whose request was blocked by policy MUST take the handling the session's policy decides.

### A Failure Mid-Stream

A model reply that fails mid-stream MUST be discarded in full.

A turn whose reply failed mid-stream MUST be retried as a new attempt.

### An Unreachable Or Timed-Out Tool

An idempotent tool call whose server is unreachable or timed out MUST be retried within its budget.

A side-effecting tool call whose server is unreachable or timed out MUST return an unknown outcome to
the model.

### A Tool Error

An error a tool returns MUST be delivered to the model as an error result rather than treated as a
failure of the turn.

### A Store Conflict Or Outage

A durable-store transaction that conflicts or fails MUST be retried with backoff.

A failed store transaction MUST leave the session's state unchanged.

### State That Will Not Decode

A session whose state will not decode MUST be quarantined.

A quarantined session's state MUST NOT be overwritten.

A quarantined session MUST NOT stop its partition from serving its other sessions.

### An Event That Keeps Failing

An event that fails the configured number of attempts MUST be quarantined so that it cannot wedge its
partition.

A session whose event was quarantined MUST end blocked with the reason.

### An Owner That Crashes Or Moves

The new owner of a partition MUST resume each session from its committed cursors.

A superseded owner MUST be fenced from committing.

### A Definition Source That Is Unreachable

An unreachable definition source MUST change nothing about the sessions it serves.

An unreachable source MUST be retried with backoff and jitter.

The platform MUST raise one alert per unreachable source rather than one per session.

### A Definition That Is Invalid Or Blocked

A definition version that fails validation or is blocked at the hook MUST be rejected by its digest.

A rejected definition version MUST NOT be retried.

A rejected definition version MUST produce an event carrying the reason.

### A Definition Missing From Its Index

A definition missing from its source's index MUST be held rather than retired.

## No Dead Ends

### Every Stuck State Has A Way Out

Every blocked or quarantined state MUST have a way out that an adapter can drive through an inbox event
or the resume control.

Alarming an operator MUST NOT be the only handling of any failure class.

### Quarantine And Blocks Are Events

Every quarantine MUST be published as an event with its reason.

Every transition to a blocked state MUST be published as an event with its reason.
