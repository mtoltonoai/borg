# Contract — Event Hooks Out

> **CONTRACT.** This document pins the delivery of a session's lifecycle and status events to the
> destinations its definition names. It is the mirror of webhooks in, and it replaces the turns agents
> once spent reporting their own status and the watchdogs that polled for liveness. It is honored across
> releases by the platform and by every receiver. Its requirements realize
> [Core Principle III](../../constitution.md) and trace to [overview §14](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The retry schedule and the signing keys are declared defaults.

## Purpose And Scope

A receiver such as the collaboration platform learns a session's state from the platform rather than from
the agent. Deliveries come from the session's durable outbox, so they survive crashes and moves, and they
never affect the session they describe. This contract fixes the event set, the delivery guarantees, the
payload, and the receiver's obligations.

## The Event Set

### The Set Is Closed And Versioned

The set of event kinds a hook delivers MUST be a closed set named by this contract's version.

The set MUST include started, adopted a version, woken, idle, blocked, waiting on a defer, stopped,
failed, quarantined, and out of budget.

A blocked event MUST carry its reason.

A waiting-on-a-defer event MUST carry the defer's decision id.

## Delivery

### Delivery Comes From The Durable Outbox

A hook delivery MUST be made from the session's durable outbox rather than from memory.

Deliveries MUST be at least once.

Deliveries for one session MUST arrive in order of their sequence.

Every delivery MUST carry an id that a receiver can dedupe on.

Every delivery MUST carry a sequence number within its session.

Every delivery MUST be signed with a timestamp.

A delivery that fails MUST be retried with backoff.

### A Backlog Coalesces

A backlog of status events for one session MUST coalesce to the latest state rather than replay every
intermediate state.

### A Hook Never Affects Its Session

A failing or slow destination MUST NOT affect the session whose events it receives.

## Destinations And Payloads

### Destinations Are Registered

A destination MUST be registered per tenant and referenced by name.

A definition MUST NOT name a destination by arbitrary address.

A delivery MUST leave through the egress point.

### Payloads Carry State, Never Content

A payload MUST carry ids, states, reasons, and usage.

A payload MUST NOT carry transcript content.

## The Receiver

### A Receiver Dedupes And Orders

A receiver MUST ignore a delivery whose id it has already applied.

A receiver MUST NOT let an older sequence overwrite a newer state.

A receiver MUST verify the signature before acting on a delivery.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed receivers, or else carry an explicit
version increment.

A change to this contract that is not additive with respect to deployed receivers MUST carry a stated
migration path.
