# Capability — Tools And Dispatch

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the card as the tool registry, the dispatch of a tool call as a durable operation, the native
> tools the platform implements itself, sub-sessions and forks, and the rule that everything else is a
> service. Requirements realize [Boundary "The Platform Hosts No Programs For Its
> Tenants"](../../constitution.md) and [Platform Principle P6](../../constitution.md) and trace to
> [overview §7](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The wire shapes are pinned by the
> [tool-call contract](../contracts/tool-call.md); the tool protocol is a declared default.

## Purpose And Scope

Tools are services, and the agent's definition lists them. This capability fixes what the platform does
with that list: refuse anything not on it, gate and record every call, lease credentials by reference,
commit results as state, and recover by each tool's semantics. It also fixes the small set of native
tools and the shape of sub-sessions and forks.

## The Card Is The Registry

### Dispatch Refuses What The Card Does Not List

A definition MUST list the tool servers, and optionally the tools on each, that its sessions may call.

Dispatch MUST refuse a call to a server the card does not list.

Dispatch MUST refuse a call to a tool the card excludes.

The platform MUST NOT rely on a provider to enforce which tools a model has loaded.

The platform MUST NOT keep a tool catalog of its own.

### Specs Are Loaded, Hashed, And Checked

The platform MUST hash each tool spec a session loads.

A dispatch record MUST name the hash of the spec the model saw and the build the server reported.

A tool spec MUST pass the input deciders when it loads.

A call's arguments MUST be checked against the spec's schema at the tool-call hook.

### Changes Reach Sessions As Reconfiguration

A server's announced change to its tool list MUST reach running sessions as live reconfiguration, within
what the card allows.

The platform MUST list a server's tools afresh on every reconnect.

Adding a server to a card MUST be a widening change.

A revocation of a tool MUST apply at dispatch, so that a revoked tool cannot run even from a turn
already in flight.

## A Tool Call Is A Durable Operation

### The Dispatch Sequence

A tool call MUST be resolved against the session's card before anything else.

A tool call MUST pass the tool-call hook before it is recorded.

A dispatch record MUST be committed before the call goes out.

A credential a tool needs MUST be leased by reference for the session's principal, chain, and target, and
handed only to the executor.

A tool call's output MUST be streamed while it runs.

A tool result MUST pass the tool-result hook before the model sees it.

A tool result MUST be committed as a state transition.

### Recovery Follows The Tool's Semantics

An in-flight call found on recovery MUST be retried within its retry budget when its declared
idempotency guarantee covers the unrecorded outcome.

Recovery MUST NOT require an outcome check before retrying a call whose declared idempotency guarantee
covers the unrecorded outcome.

An in-flight side-effecting call found on recovery MUST return an unknown outcome to the model with
what was sent and when if its declared semantics do not guarantee safe repetition with an unrecorded
outcome.

A tool with a reconcile binding MUST be asked on recovery whether its effect landed if its declared
semantics do not guarantee safe repetition with an unrecorded outcome.

### A Superseded Owner Cannot Act

Every dispatch MUST carry the caller's lease generation.

A dispatch from a superseded owner MUST NOT take effect even when its receiver has not yet recorded
the replacement generation.

### Calls Leave Through One Egress Point

Every outbound tool call MUST leave through the egress point.

The egress point MUST admit only the destinations the tool's binding names.

### Dispatch Is Observable

Tool metrics MUST split only by executor kind and outcome.

The tool's name MUST ride in events as a field.

## Native Tools

### Native Tools Act Only On The Platform's Own State

The platform MUST implement natively the tools that act on its own state: subscribe and unsubscribe,
publish and send, start a sub-session, set focus, the timer operations, the key-value slot, loading a
deferred tool already on the card, and paging a stored output.

The platform MUST NOT implement natively a tool that does not act on its own state.

A function that does not act on the platform's own state MUST be provided by a service on the card.

### Mechanisms Are Not Tools

The platform MUST NOT require a model turn to report status, drain an inbox, re-arm a loop, send a
heartbeat, or request a compaction.

### The Sandbox Is Not A Tool Executor

The sandboxed program runtime MUST be used for deciders rather than as a tool executor.

## Executors

### Three Executor Kinds

The platform MUST support native tools, tool-protocol servers, and HTTP servers as executors.

A tool call MUST run from the partition owner's node.

### What Every Service Receives

Every service called on a session's behalf MUST receive the session's assertion.

A service MAY publish into the session that called it, under a grant for the service's principal.

A service MAY exchange a call's assertion for credentials of its own at the credential broker.

## Sub-Sessions

### A Sub-Session Is Never Wider Than Its Parent

A sub-session MUST run its parent's definition narrowed by the call, or another of the tenant's
definitions as a keyed instance with the parent as requester.

A sub-session MUST NOT hold authority its parent lacks.

A sub-session's budget is its parent's, carved as the identity capability requires.

### A Sub-Session Reports To Its Parent

A sub-session's lifecycle events MUST arrive in its parent's inbox.

A sub-session's final output MUST return as the result of the call that started it.

A sub-session MUST end with its parent unless the call said otherwise.

## Forks

### A Fork Starts From Its Parent's Transcript

A fork MUST take its parent's transcript up to the fork point and its parent's definition narrowed by
the call.

A fork MUST diverge from the fork point without touching its parent.

A fork's first request MUST follow its parent's route, so that forking costs a cache read rather than a
rewrite.

A fork MUST pin its parent's frozen blobs and copy only the unfrozen head.

A fork's grants and budget MUST be its parent's, narrowed.

A fork that needs a workspace MUST take a snapshot fork from the workspace service.

### A Fork Is Its Own Principal

A fork's assertion MUST name the fork rather than its parent.

A fork MUST inherit none of its parent's timers.

### Fork Queries Are Throwaway

A fork started to answer a question about its parent MUST run with model-only tools.

A fork started to answer a question MUST be discarded once it answers.

A fork query MUST leave the original session's state unchanged.

### Recurring Work Prefers A Fresh Sub-Session

The sub-session tool SHOULD steer recurring work, and work that needs only a brief, toward a fresh
sub-session rather than a fork.

## Long Tool Runs

### A Session May Sleep While A Tool Runs

A tool result MAY arrive as an inbox event after the call returned.

A session MUST be evictable while a long tool run is in progress.
