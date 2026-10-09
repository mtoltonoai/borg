# Capability — Model Turns And Scheduling

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how every model call is scheduled, prioritized, and routed; how capacity is shared across a
> cluster; and the properties of the inference layer that renders a session's transcript for a provider
> and decodes its reply. Requirements realize [Platform Principle P1](../../constitution.md) and
> [Core Principle III](../../constitution.md) and trace to [overview §5](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. Providers, their endpoints, and their limits are declared defaults.

## Purpose And Scope

Colocating many sessions on a node is what lets something see every waiting turn. This capability fixes
what that something does: order by priority, turn throttling into queueing, lease quota across the
cluster, route across capacity sources, and keep each session's route sticky. It also fixes the inference
layer's invariants: a provider-neutral transcript, opaque provider state, a request that is a pure
function of the log, aligned cache boundaries, and structured error classification.

## Every Call Goes Through A Scheduler

### No Call Goes Straight To A Model Endpoint

Every model call MUST pass through a scheduler on the partition owner's node.

A session MUST NOT call a model endpoint directly.

### Priority Follows The Work

A session's effective priority MUST be the highest of its base priority, the priority of the work it is
handling, and any priority inherited from a session waiting on it.

A session's effective priority MUST fall back when the work that raised it is done.

Raising a session's priority MUST be a granted capability checked at the control-operation hook.

A definition's base priority MUST be bounded by its binding's ceiling.

### Priority Orders Everything That Queues

Model admission MUST be ordered by priority.

The order in which a partition processes its dirty sessions MUST be by priority.

Eviction MUST remove the lowest-priority sessions first.

The deciders' compute pool MUST serve by priority.

## Throttling Becomes Queueing

### Admission Adapts To Capacity

A scheduler MUST admit waiting turns per model and endpoint in priority order, breaking ties by deadline.

A scheduler's concurrency MUST rise until throttles or latency climb and then back off multiplicatively.

A throttle MUST narrow the admission window rather than fail a session that is inside its deadline.

### Quota Is A Cluster Resource

Each node MUST lease a share of each model's quota per account from a quota coordinator.

Quota shares MUST rebalance by demand.

A share of quota MUST be reservable for a priority class, so that urgent work is admitted while
background work saturates the rest.

The quota coordinator MUST NOT sit on the critical path of a model call.

### Fair Share

A scheduler MUST weight admission per tenant and, within a tenant, by priority, from configuration.

Budgets MUST be applied at the model-request hook.

## Latency Classes

### Three Classes

The platform MUST offer interactive, background, and batch latency classes.

A batch request's result MUST arrive as an inbox event, so that its session can be evicted while it
waits.

Compaction, distillation, and retrospectives MUST default to the background or batch class.

## Capacity Sources

### A Source Is An Account, A Region, And A Model Endpoint

A capacity source MUST be an account, a region, and a model endpoint, reached through a role.

A scheduler MUST route across capacity sources by headroom and cost.

The platform's own accounts MUST form the default capacity pool, so that a tenant onboards with no
account setup.

### A Tenant's Own Capacity Comes From Tenant Configuration

A capacity source MUST come from the tenant's configuration rather than from an agent definition.

A tenant-supplied role MUST be used only if it is on the tenant's trusted list.

A tenant-supplied role MUST be used only if its trust policy requires the tenant as the external id.

### Routes Are Sticky

A session's route to a capacity source MUST be sticky, so that its cached prefix stays warm.

## Connections

### Connections Are Shared And Bounded

Connections to a model endpoint MUST be a pool shared across the node's sessions.

The transport MUST enforce a provider's per-connection concurrency limit before sending rather than
discover it by refusal.

The transport MUST detect a dead connection with keepalives rather than wait for a request to fail.

The transport MUST enforce the configured request body cap before a body is sent.

## The Inference Layer

### One Provider-Neutral Transcript

A session's transcript MUST be held in one provider-neutral form from which each provider's request is
rendered.

A provider-bound payload MUST be carried opaquely, tagged with its provider, and interpreted only by that
provider's edge.

Provider state such as reasoning or a compaction item MUST be round-tripped unmodified.

### A Request Is A Function Of The Log And Policy

A model request MUST be a pure function of the session's durable log and its policy.

A rendered segment MUST depend only on its content and a render version, so that it is cached by content
and shared across sessions.

### Cache Boundaries Coincide

A cache breakpoint MUST be placed at a segment boundary, so that the client's render cache and the
provider's prompt cache invalidate the same suffix.

A breakpoint that no block can carry MUST be dropped and reported rather than fail the turn.

### Changes To The Prefix Preserve The Cache

A change to the cached prefix MUST be made through the provider's cache-preserving path or at a
compaction.

The system prompt MUST NOT be edited in place outside a compaction.

### A Model Switch Never Fails A Turn

A switch of model mid-session MUST NOT fail a turn because of reasoning produced by another model.

### Reasoning Binding Is Explicit

Where a provider binds reasoning to the history it was produced in, the platform MUST set the binding
policy explicitly rather than rely on the provider's default.

### Failures Are Classified Structurally

A provider failure MUST be classified from the provider's structured fields rather than by matching
message text, except where a provider offers no structured field for the condition.

A context-window overflow MUST be classified as such, so that the turn loop compacts rather than retries.

### Retention Terms Are Honored

A scheduler MUST route a session only to a model whose data-retention terms the tenant's policy permits.

## Instrumentation

### Every Turn Tells Its Story

Every turn MUST publish one outcome event carrying its request id, model, stop or failure class,
latencies, and token totals.

Every stage of a turn MUST be an event at a level matching its frequency.

A turn abandoned by its caller MUST be reported with the stage it reached.

### Metrics Split By Closed Sets

Turn metrics MUST split only by model, dialect, endpoint, signing mode, error class, stage, stop
reason, segment kind, and cache lifetime.

Every token count MUST be split by model.

### Scheduling Is Observable

A scheduler MUST publish queue depth, wait time, admitted concurrency, and throttles per model and front
door.
