# Contract — The Tool Call

> **CONTRACT.** This document pins what passes between the platform and a tool server: the spec and
> semantics a server declares, the call the platform makes, and the guards a server honors so that a
> call is a durable operation whoever makes it. It is honored across releases by the platform, by the
> services the platform's operators run, and, where they honor an idempotency key, by remote targets. Its
> requirements realize [Platform Principle P6](../../constitution.md) and trace to
> [overview §7](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The tool protocol and the result bounds are declared defaults.

## Purpose And Scope

Tools are services. A tool's spec comes from its server, its semantics default to the server's
annotations, and a call carries the identity of its caller and a key that makes it safe to redo. This
contract fixes those shapes; how the platform resolves, gates, and commits a call is the tools
capability.

## The Spec

### The Server Declares The Spec

A tool's spec MUST consist of a name, a description, and an input schema, declared by the server that
provides it.

A server MUST list its tools on request and announce a change to the list.

The platform MUST treat a spec as untrusted input, since its description enters the model's context.

### The Server Declares The Semantics

A tool's semantics MUST include whether it is idempotent, whether it has side effects, its timeout, its
retry policy, and whether it is cancellable.

A tool MAY declare a reconcile binding that reports whether an in-flight effect landed.

A tool's semantics MUST default to the server's declared annotations.

A definition MUST only make a tool's semantics more conservative than the server declared.

A tool that is not declared cancellable MUST be treated as not cancellable.

### The Spec Declares Call Types And Credentials

A spec MAY declare call-type tags that hook policies are written against.

A spec MUST name any credential it needs by reference rather than by value.

## The Call

### Every Call Carries Its Identity

Every call MUST carry a call id.

Every call MUST carry the caller's assertion, addressed to the server.

Every call MUST carry the caller's lease generation.

### The Call Id Is The Dedupe Key End To End

The call id MUST be given to the executor and to any target system that accepts an idempotency key.

A re-issued side-effecting call that follows an unknown outcome MUST reuse the original call id.

## Server Guards

### A Service Dedupes By Call Id

A service the platform's operators run MUST keep a receipt keyed by call id.

A service that receives a call id it holds a receipt for MUST return the recorded outcome without
running the call again.

A receipt MUST commit in the same transaction as the call's effect.

### A Service Answers Reconcile

A service with a reconcile binding MUST answer, for a call id, whether that call landed and with what
outcome.

### A Service Fences A Superseded Owner

A service the platform's operators run MUST refuse a call whose lease generation is older than the
highest it has recorded for that session.

The refusal of a superseded owner MUST be distinct from any other error.

A remote target outside the operators' control MAY be guarded only by the call id.

## Results

### A Result Is Bounded In The Transcript

A result MUST reach the transcript as a bounded view whose bounds come from the tool spec.

The full output of a call MUST be stored once, so that a session can page through it.

### A Tool Error Is A Result, Not A Failure

An error a tool returns MUST reach the model as an error result rather than as a failed turn.

### An Interrupted Side Effect Reports Unknown

A side-effecting call interrupted before its outcome was recorded MUST return an unknown outcome that
names what was sent and when.

### A Long Call May Finish Later

A call MAY return before its work completes and deliver its result later as an inbox event.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed tool servers, or else carry an
explicit version increment.

A change to this contract that is not additive with respect to deployed tool servers MUST carry a stated
migration path.
