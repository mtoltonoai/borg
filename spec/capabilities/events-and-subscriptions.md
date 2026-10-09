# Capability — Events And Subscriptions

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines publish, subscribe, and send; fan-out to inboxes; how a notification's priority decides whether
> it interrupts; why subscriptions are the platform's own; and how people's pages subscribe as viewers.
> Requirements realize [Core Principle II](../../constitution.md) and [Platform Principle
> P4](../../constitution.md) and trace to [overview §4](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The envelope's fields are pinned by the
> [event-envelope contract](../contracts/event-envelope.md).

## Purpose And Scope

Events are how anything reaches a session, and subscriptions are how a session says what reaches it.
This capability fixes the primitives, the ordering and fan-out guarantees, the interruption rule, and the
viewer path for people. The inbox application that makes delivery exactly once is in the sessions
capability.

## Primitives

### Three Primitives, Opaque To The Core

The platform MUST provide publish, subscribe, and unsubscribe as primitives.

The platform MUST provide send to one session or to a keyed definition with its key.

The core MUST NOT interpret a topic name or a payload.

A subscriber MUST be a session or a viewer.

### Subscriptions Come From Definitions, Sessions, And Grants

A standing subscription MUST come from the session's definition.

A session MUST be able to subscribe and unsubscribe itself through native tools.

A principal other than the session MUST manage the session's subscriptions only through a control
operation that passes the hook under a grant.

## Fan-Out

### Publish Latency Is Independent Of Subscribers

A publish MUST complete once the event is appended to its topic, before fan-out to subscribers.

Fan-out MUST deliver to subscribers' inboxes and doorbells in batches within the transaction key bound.

The latency of a publish MUST be independent of the number of subscribers to its topic.

### Each Subscriber Receives In Order

Each subscriber MUST receive a topic's events in the order they were published.

Each notification MUST carry an id the subscriber can dedupe on.

### Indexes Are Kept Both Ways

The platform MUST keep an index from each topic to its subscribers.

The platform MUST keep an index from each session to its subscriptions.

### Every Delivery Is Attributed

Every delivered event MUST carry its publisher's authenticated principal and chain, stamped by the
platform.

Every delivered event MUST name its causal parent.

## Priority And Interruption

### The Session Decides Whether A Notification Interrupts

A notification whose priority clears the session's current interrupt threshold MUST interrupt the work
in progress.

A notification whose priority does not clear the threshold MUST wait for the next turn boundary.

### The Threshold Is Dynamic And Configured

The interrupt threshold MUST be computed from the session's state, the work invested in the current
step, the expected remaining duration of the step, and the session's own priority.

The threshold function MUST be configuration.

An idle session MUST wake for any notification.

A session MUST be able to set its own focus level through a native tool.

### Interrupting A Turn Cancels Only The Model

Interrupting a model turn MUST cancel the stream and discard the partial output.

A running side-effecting tool MUST NOT be cancelled by a notification.

The result of a tool that was running when a notification arrived MUST be recorded before the
notification is delivered.

### Digests Are A Subscription Option

A subscription MAY ask for its events as a digest rather than one at a time.

## Subscriptions Are The Platform's Own

### Ordering And State Live In One Store

The inbox append and the doorbell write MUST commit in one transaction of the durable store.

The platform MUST NOT rely on an external queue for per-session ordering or for exactly-once
application.

### An External Queue Is At Most An Edge

An external queue or topic MAY buffer publishes at the edge for publishers that want independence from
the cluster's availability.

An edge MUST be drained by the platform's ingress into its own topics.

## Viewers

### A Viewer Is One Page's Connection

A viewer MUST be one live connection from one person's page, opened with a short-lived viewer token for
one person in one tenant.

A viewer MUST subscribe and unsubscribe on its connection as its page changes.

Closing a viewer's connection MUST end all of its subscriptions.

A viewer's subscriptions MUST NOT outlive its connection.

A reconnecting page MUST send its subscription set again.

### Nothing Missed, Nothing Replayed

A viewer that subscribes to a topic MUST NOT miss a change made between the page loading its view and
the subscription taking effect.

A viewer MUST NOT receive history.

A page that needs current state MUST load it from the adapter rather than from the viewer stream.

### Small And Lossy By Design

A viewer MUST receive change notices bounded by the publisher's payload bound rather than records.

A slow viewer, or a burst on a hot topic, MUST collapse to one reload notice per topic.

A viewer MUST NOT slow a publisher, a session, or another viewer.

### Only What Its Person May Read

A viewer subscription MUST pass a read check against the topic's access scope when it is made.

A viewer subscription MUST end when its person's access is revoked.

### Person-Addressed Routing Is The Adapter's

What is addressed to a person MUST be published by the adapter to that person's own topic, which the
person's pages subscribe to.
