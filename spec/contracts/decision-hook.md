# Contract — The Decision Hook

> **CONTRACT.** This document pins the interface between the platform and an external decider: a service
> the platform asks, at one of its hook points, for a decision it then enforces. Stop admission, a human
> approval routed through an adapter, and a decision service called over the network are all instances.
> It is honored across releases by the platform and by every adapter that answers it. Its requirements
> realize [Core Principle VII](../../constitution.md) and [Governance Floor "Agents Never
> Approve"](../../constitution.md) and trace to [overview §8](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The deadline bounds and the request size bound are declared defaults.

## Purpose And Scope

An external decider is declared by a definition, never by a model, and points at a destination the
tenant registered. Unlike an event hook, it decides, so it has a deadline and a failure outcome. This
contract fixes what the platform sends, what the decider answers, and what each party guarantees. The
pipelines that order deciders and the semantics of each verdict inside the session are the governance
capability.

## Declaration

### A Decision Hook Is Declared By Configuration

A decision hook MUST be declared by the session's definition rather than by the model.

A decision hook MUST point at a destination registered with the tenant and referenced by name.

A decision hook MUST declare a deadline within the configured bounds.

A decision hook MUST declare a failure outcome, which defaults to block.

### Latency-Sensitive Hooks Admit An External Decider Only As A Defer

An external decider on the model-output, tool-call, or tool-result hook MUST run only as a defer.

An external decider on the stop-request hook MAY run synchronously.

## The Request

### The Request Identifies Itself

A decision request MUST carry the contract version.

A decision request MUST carry a decision id that is the same when the same question is asked again.

A decision request MUST carry the tenant and the session.

A decision request MUST carry the hook point and the call-type tags in play.

A decision request MUST carry the name of the decider being asked.

### The Request Names The Actors

A decision request MUST carry the session's principal and on-behalf-of chain.

A decision request acting on another principal's event MUST also carry the requester's principal and
chain.

### The Request Carries The Subject

A decision request MUST carry the subject as the deciders before it left it.

A decision request MUST carry what the session is doing and its usage.

A decision request MUST cut content past the configured size bound with an explicit marker.

A decision request on the model-output hook MAY include the tool results the output cites.

### The Request Is Attributable

The platform MAY sign a decision request under its own identity so the destination can verify the
question is the platform's.

## The Response

### A Decision Is One Of A Closed Set

A decision MUST be exactly one of allow, flag, block, defer, or transform.

A block MUST carry a reason written for the model.

A reason MUST NOT repeat the content it rejects.

A transform MUST carry the patch to apply as redacted spans or set fields.

A defer MUST carry the principals allowed to resolve it, a deadline, and an outcome on timeout.

### Silence Is The Failure Outcome

A decider that gives no verdict within its deadline MUST be treated as having answered its declared
failure outcome.

A response that is not a verdict of the closed set MUST be treated as having answered the failure
outcome.

### A Decider Is Idempotent By Decision Id

A decider asked the same decision id again MUST answer the same verdict or report the verdict already
given.

## Stop Admission

### The Stop Hook Asks Whether Work Remains

A stop-admission request MUST name the session whose stop is proposed.

A stop-admission block MUST carry a reason that names the open work that keeps the session going.

A stop-admission decider MUST allow a stop for a session whose definition is paused or retired.

## Resolution Of A Defer

### Only A Listed Principal Resolves

A defer MUST be resolved only by a stamped principal on its allowed list.

A defer MUST be resolved at most once.

A defer past its deadline MUST take its timeout outcome.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed deciders, or else carry an explicit
version increment.

A change to this contract that is not additive with respect to deployed deciders MUST carry a stated
migration path.
