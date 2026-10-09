# Contract — The Usage Record

> **CONTRACT.** This document pins the one record the platform writes for every model attempt, tool call,
> and decider call. Budget deciders read it now, experiments compare versions on it, and metering will
> read the same stream later, so its shape is honored across releases. Its requirements realize
> [Platform Principle P7](../../constitution.md) and [Core Principle III](../../constitution.md) and
> trace to [overview §15](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The retention period and the sink are declared defaults.

## Purpose And Scope

The usage stream is the one place the platform's cost and causes are visible together, so that a change
to a definition can be judged on the same data its predecessor ran on. A record names who, for whom,
why, and what it cost, and never what was said.

## One Record Per Unit Of Work

### Every Attempt Is Recorded Once

Exactly one usage record MUST exist for every model attempt.

Exactly one usage record MUST exist for every tool call.

Exactly one usage record MUST exist for every decider call.

## Fields

### A Record Names Its Tenant And Actors

A usage record MUST name its tenant.

A usage record MUST name the session's principal chain.

A usage record MUST name the session and its parent session, if any.

A usage record MUST name the digest of the definition version the session ran.

A usage record MUST carry the attribution tags the definition configured, such as a project or a task.

### A Model Attempt's Record Names Its Cause And Outcome

A model attempt's record MUST name what caused the turn: an event, a timer, an operator message, or a
stop-admission continuation.

A model attempt's record MUST name the turn's outcome as no action, or the actions it took.

A model attempt's record MUST carry the cache reads and cache writes the attempt incurred.

A model attempt's record MUST carry the prompt size.

A model attempt's record MUST name the model and the capacity source that served it.

### A Record Carries No Content

A usage record MUST NOT carry transcript content.

## Retention And Delivery

### The Stream Has Its Own Retention

Usage records MUST keep a retention of their own, apart from session data.

A partition MUST ship its usage records to their sink within the configured interval.

A drain MUST flush pending usage records before the final session records are written.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed readers, or else carry an explicit
version increment.

A change to this contract that is not additive with respect to deployed readers MUST carry a stated
migration path.
