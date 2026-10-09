# Capability — The Collaboration Platform

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the collaboration platform where people and agents work together, and its role toward the agent
> platform as the first definition source, the first tool server, and the adapter that gives events their
> meaning. It is a product built beside the platform, so it inherits the monorepo tenets and must not
> contradict the constitution. Requirements realize [Core Principle I](../../constitution.md) and
> [Platform Principle P4](../../constitution.md) and trace to [overview §18](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The store, the live-update transport, and the identity
> providers are declared defaults.

## Purpose And Scope

The collaboration platform is where work is organized: boards, documents, chat, a wiki, and questions
anyone can pose. Toward the agent platform it is a client and an adapter, never a client of the durable
store. This capability fixes the properties a valid implementation has, independent of which store or
transport it chooses.

## Scale And Shape

### It Runs On Many Servers With No Node State

The service MUST run on many servers behind a load balancer.

The service MUST hold no state that cannot be lost with a node.

The service MUST scale to the agent platform's session count with thousands of people's viewers.

A change MUST NOT be ordered across the whole tenant.

Changes MUST be ordered per record or per scope.

### A Durable Store Behind It

The service MUST keep its durable state in a store that survives a node, with documents in the blob
store.

## Identity And Lifecycle

### Identity Comes Only From Authentication

The acting identity of a call MUST come from authentication rather than from an argument.

A write MUST record the authenticated principal as its author.

An identity argument MUST be accepted only when it equals the authenticated caller.

A mismatch between an identity argument and the authenticated caller MUST be refused with an event.

Authorization MUST fail closed.

### Lifecycle Intent Is The Only Pause Path

An agent's run, paused, or retired intent MUST be the only way to pause its sessions.

A pause by any path MUST resolve to that intent.

A check MUST report any agent whose intent and reported state disagree.

## Toward The Agent Platform

### It Is A Client And An Adapter, Not A Store Client

The service MUST reach the agent platform through the platform's API rather than through the durable
store.

### It Publishes Every Record Change

Every change to a record MUST publish to that record's topic.

A published event's id MUST derive from the service's own ordered revision, so that delivery is exactly
once.

An event payload MUST be bounded.

An event payload MUST carry the access scope its source allowed.

### It Serves Definitions

The service MUST serve agent cards as a definition source over the definition-source contract.

The service MUST compose a card only from approved document versions.

### It Manages Subscriptions And Answers Hooks

The service MUST subscribe an agent's session to a task when it assigns the task and unsubscribe it when
the work is done.

The service MUST answer stop admission from whether the agent has open, unblocked work.

The service MUST receive the agent platform's session status hooks, replacing agents reporting their own
status.

The service MUST broker a short-lived tail token rather than relay a session's tail itself.

### Its Own Notification Machinery Goes Away

The service MUST NOT keep an agent inbox, a notification poll, or a liveness watchdog of its own.

The service MUST keep only what is addressed to a person, publishing each new notification to that
person's own topic.

## Coordination

### Coordination Lives Here Until It Generalizes

Fanning work out, collecting results, looping with a cap, and re-delegating MUST live here as tasks and
assignments rather than as a layer in the agent platform.

A coordination mechanism MUST move into the shared framework only once working cases show what
generalizes.

## Migration

### Every Cited Id Survives

Every id people cite MUST resolve to the same object after a migration from a prior system.

A migration MUST follow a shadow comparison and need only a brief write freeze.

Delivery and identity MUST move one agent at a time behind a per-agent delivery switch.

### The Switch Keeps One Writer

At most one runtime MUST be able to act as a given agent at any moment.

A straggling old session MUST be fenced from writing as the agent after the switch flips.

A rollback MUST keep the session's state and lose no event.

## Growth

### Growth Is One Document Model

Code reviews, notebooks, and dashboards MUST be block types on one typed-block document model, beside
requirement blocks.
