# Capability — Governance

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the one governance mechanism: a fixed set of hook points, an ordered pipeline of deciders at
> each, a closed set of decisions, stop admission, and the rule that confidence comes from mechanism
> rather than from a click. Requirements realize [Core Principle VII](../../constitution.md),
> [Platform Principle P2](../../constitution.md), and [Governance Floor "Agents Never
> Approve"](../../constitution.md) and trace to [overview §8](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The decider kinds' engines and the request size bound are
> declared defaults; the request and response shapes are pinned by the
> [decision-hook contract](../contracts/decision-hook.md).

## Purpose And Scope

Authorization, content deciders, approvals, secret scrubbing, stop admission, and interruption are one
mechanism so that the core has a single enforcement path. This capability fixes the hook points, how a
decision request is built, how a pipeline runs, what each decision means, and how authority is widened
only by evidence. It states the mechanism's behavior, not the content of any tenant's policy.

## The Hook Points

### The Core Defines A Fixed Set Of Hooks

The core MUST define a fixed set of hook points and enforce only at them.

The hook points MUST be the model request, the model output, the tool call, the tool result, the
notification arrival, the stop request, and the control operation.

The core MUST NOT add a hook point without a code change.

### Deciders Guard Inputs As Well As Outputs

The tool-result and notification-arrival hooks MUST guard what enters a session.

An inbound event MUST NOT carry a tool call for the core to dispatch.

Only a session's own model output MUST dispatch a tool.

## Decision Requests And Pipelines

### The Core Builds The Decision Request

At each hook the core MUST build a decision request carrying the principal and chain, the hook and
call-type tags, the content, what the session is doing, and its usage.

A decision request MUST cut content past the configured size bound with an explicit marker.

A decision request's configuration MUST say whether a cut denies.

A decision request on the model-output hook MAY include the tool results the output cites.

### A Pipeline Runs Cheap Deciders First

A session's configuration MUST give an ordered pipeline of deciders per hook.

A pipeline MUST run cheaper deterministic deciders before more expensive ones.

A block MUST short-circuit the rest of its pipeline.

### The Core Only Enforces

The core MUST enforce the decision a decider returns rather than form a decision of its own.

A decision MUST be exactly one of allow, flag, block, defer, or transform.

## What Each Decision Does

### Allow And Flag

An allow MUST pass the subject on unchanged.

A flag MUST pass the subject on and record an event.

### Block

A blocked model output MUST be retried in the loop with the reason.

A blocked tool call MUST become an error result to the model.

A blocked control operation MUST be rejected.

A block's reason MUST NOT repeat the content it rejects.

A block's reason MUST pass the scrubbing decider, so that a block cannot put a secret back into context.

### Defer

A defer MUST park the deferred call rather than the session.

A session with other work MUST continue while a call is deferred.

A deferred call's decision MUST arrive as an inbox event.

A session with nothing else to do while a call is deferred MAY be evicted.

### Transform

A transform MUST proceed with a stated patch, redacting spans or setting fields.

A secret in a tool result MUST be removed by a transform before the result reaches the model.

## Retries And Streaming

### In-Loop Retry Is Bounded

A block's in-loop retry MUST be bounded by a budget.

A session whose retry budget runs out MUST block with the reason or escalate the block to an operator as
a defer.

### Output Is Gated Per Block

Model output MUST be gated per completed block.

Model output MUST NOT take effect before it passes its deciders, except for clearly marked unapproved
output viewed by an explicitly authorized privileged observer.

Privileged observation of unapproved output MUST enforce the same read grants and confidentiality
restrictions as approved output.

Anything that acts on the outside world MUST be a tool call gated before it runs.

A live attach that sees output before it is gated MUST mark that output as unapproved.

A live attach MUST require explicit privileged-observation authorization before showing unapproved
output.

## Stop Admission

### A Blocked Stop Keeps The Session Going

The stop hook MUST decide whether a session may stop or has work to go on with.

A blocked stop's reason MUST become the next turn's input.

Consecutive blocked stops MUST be bounded, so that a session cannot be held awake forever.

An external decider on the stop hook MAY run synchronously.

## Confidence By Mechanism

### Agents Produce Evidence And Never Approve

An agent MUST NOT hold a capability to approve a change.

Authority is widened only by a sound checker or by a person named by the tenant's governance, as the constitution's floor requires.

A checker that has a model inside it MUST be able to block or flag a change.

A checker that has a model inside it MUST NOT admit a change.

Evidence MUST be the checker's own record rather than an agent's account of it.

Weakening a property MUST be treated as a widening change.

### People Approve Policies, Not Instances

A class of change MUST be governed by a gate spec that states the evidence under which it may proceed.

A gate spec MUST be approved once.

A gate spec MUST run outside the agent's reach.

A gate spec MUST decide on reproducible evidence tied to the artifact.

### A Human Request Is The Exception

A decision MUST reach a human only when no mechanism can decide it.

A request that reaches a human MUST be batched with an evidence summary.

A request that reaches a human MUST NOT go to the operator who directed the work.

A human approver MUST be measured by time to approve and approval rate.

Seeded known-bad requests MUST reach approvers at a configured rate, so that an approver who is not
genuinely reviewing is detectable.

### Autonomy Is Earned

A temporal policy MAY widen what a role may do as its record of clean changes grows.

A temporal policy MUST narrow what a role may do after a rollback.

## Shadow, Caching, And Episodes

### A New Decider Runs In Shadow First

A new or changed decider MUST run in shadow, recording without enforcing, before it may block.

A shadow decision MUST record whether it would have changed the outcome.

### Decisions Are Cached By Content

A stateless decider's decision MUST be cacheable by the decider version and the input's content hash.

A stateful or temporal decider's decision MUST NOT be cached.

### Every Decision Is An Event

Every decision MUST be published as an event.

A fail-retry-pass episode, a shadow result, and an operator override MUST be recorded as training labels.

## Governance Changes Are Ordinary Operations

### Changing Configuration Passes A Hook

Every configuration change, including a session's own deciders, MUST be a control operation that passes
the control-operation hook.

The default policy MUST deny a session loosening its own governance.

A tool that writes a definition MUST declare the definition-write call type.

The tool-call hook MUST be able to deny a session writing its own definition or an ancestor's.

A narrowing or neutral change MAY apply by default.

A widening change MUST defer as a whole to the approver the tenant's governance names, unless the
source's binding is trusted for that class of change.

A policy change MUST pass automated analysis before it can enforce.

A policy change that claims to only narrow MUST prove it with a sound checker.

## The Trust Root

### Two Touchpoints Sit Outside The Pipeline

A tenant's genesis bundle MUST be installed when the tenant is created.

Binding, changing, or removing a definition source MUST be a governance-tier operation.

Governance policy itself MUST change only through a tier above ordinary governance.

An emergency-override path, checked by the core itself and fully audited, MUST be able to replace a
policy that wedges governance.

## Decider Kinds And Failure

### Deciders Come In A Closed Set Of Kinds

A decider MUST be one of a pattern, a classifier, a sandboxed program, a declarative policy, a model
judge, or an external decider.

A judge or an external decider on a latency-sensitive hook MUST run only as a defer.

A sandboxed program decider MUST be deterministic and bounded in time and memory.

A default decider MUST run behind the decider interface rather than as policy code built into the core.

### Failure Defaults To Closed

A decider's failure handling MUST default to blocking.

A classifier that is not required to block MAY run as a flag off the turn's path.
