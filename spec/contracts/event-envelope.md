# Contract — The Event Envelope

> **CONTRACT.** This document pins the shape of an event as a publisher hands it to the platform through
> publish or send, and as the platform delivers it to a subscriber. Adapters publish under it, sessions
> and viewers receive under it, and it is honored across releases. Its requirements realize
> [Platform Principle P4](../../constitution.md) and [Governance Floor "Scopes Are Recorded From The
> First Event"](../../constitution.md) and trace to [overview §4](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The payload bound and the dedupe window are declared defaults.

## Purpose And Scope

The core never interprets a topic or a payload. What it needs from an event is addressing, ordering,
deduplication, priority, confidentiality, and attribution, and those are the fields this contract pins.
Everything else is the adapter's.

## Addressing

### An Event Names Where It Goes

A published event MUST name a topic.

A sent event MUST name a session, or a keyed definition together with its key.

A topic MUST be scoped within the publisher's tenant.

## Identity And Deduplication

### Every Event Has A Dedupe Id

Every event MUST carry an id that dedupes retries.

The platform MUST apply an event with an id it has already applied for the same destination at most
once.

A publisher that publishes from an ordered log SHOULD derive each event's id from its position in the
log, so that a retry is a duplicate by construction.

## Priority

### Every Event Has A Priority

Every event MUST carry a priority.

The core MUST NOT interpret a priority beyond ordering.

## Confidentiality

### Every Event Carries Its Scope

Every event MUST carry the access scope its source allowed.

## Attribution

### The Publisher Is Stamped, Never Read

The platform MUST stamp every delivered event with its publisher's authenticated principal and chain.

The platform MUST NOT take a publisher's identity from the payload.

Every delivered event MUST name its causal parent event.

## Payload

### The Payload Is Opaque And Bounded

The core MUST NOT interpret a payload.

A payload MUST NOT exceed the configured payload bound.

Content larger than the payload bound MUST be carried by reference.

### An Inbound Event Never Dispatches A Tool

An inbound event MUST NOT carry a tool call for the core to dispatch.

## Attachments

### Attachments Ride Beside The Payload

An event MAY carry attachments, each a content type and a blob reference.

The platform MUST render an attachment into a turn as a content block without reading the payload.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed publishers and subscribers, or else
carry an explicit version increment.

A change to this contract that is not additive with respect to deployed publishers and subscribers MUST
carry a stated migration path.
