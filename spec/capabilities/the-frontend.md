# Capability — The Frontend

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the one authenticated API through which anything calls the platform: onboarding, governance,
> emergency controls, the data plane, the tail, webhooks in, and event hooks out, and what the API
> deliberately does not accept. Requirements realize [Platform Principle P4](../../constitution.md),
> [Boundary "The Platform Hosts No Programs For Its Tenants"](../../constitution.md), and
> [Governance Floor "Identity Comes From Authentication"](../../constitution.md) and trace to
> [overview §14](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The transport, the request caps, and the trace format are
> declared defaults.

## Purpose And Scope

The frontend is small on purpose: tools, definition sources, and hook destinations are services the
platform calls, so the surface that calls the platform is a narrow API scoped to a tenant. This
capability fixes who may call, what the API accepts, and what it refuses to let a caller write. Where the
realized platform still writes an account's configuration through the API, that is an interim recorded as
a declared default in [spec/defaults.md](../defaults.md); the requirements here state the
definition-source end state.

## Every Call Is Authenticated And Scoped

### A Principal Makes Every Call

Every call to the frontend MUST be made by an authenticated principal.

Every call MUST be scoped to one tenant.

Every call MUST pass the control-operation hook, so that an external caller meets the same policy a
session does.

The acting identity of a call MUST come from authentication rather than from an argument.

### Configuration Is Never Written Through The API

A session's configuration MUST NOT be written through the API.

The desired set of sessions MUST NOT be written through the API.

A budget cap or a standing subscription MUST NOT be written through the API.

The platform MUST take session configuration from definitions rather than from the API.

## Onboarding

### A Tenant Is Bootstrapped, Not Registered

A tenant's genesis bundle MUST be installed when the tenant is bootstrapped.

Binding a definition source or a hook destination MUST be a governance-tier operation.

## Governance

### Governance Operations Are Governance-Tier

Writing a policy, a grant, or a catalog entry MUST be a governance-tier operation.

A grant MUST be written by the identity adapter rather than by an arbitrary caller.

## Emergency Controls

### Emergency Controls Work Without The Source

The API MUST expose pause, stop, quarantine, and resume, per session, per definition, and per tenant.

An emergency control MUST work when the definition source is unreachable.

## The Data Plane

### What The Data Plane Accepts

The API MUST accept a publish to a topic.

The API MUST accept a send to one session, or to a keyed definition with its key, creating the instance
on first use.

The API MUST accept resolution of a defer, from a principal the defer allows.

The API MUST accept management of a session's dynamic subscriptions on its behalf.

The API MUST accept a question to a session answered through a throwaway fork that does not touch the
original.

The API MUST accept a read of a session's state, and of its transcript ranges with a read grant.

### An Event Is An Opaque, Deduped Payload

An event accepted by the API MUST carry a topic, a priority, and an id that dedupes retries.

An inbound event MUST NOT carry a tool call.

An event MAY carry attachments, each a content type and a blob reference, rendered into a turn without
reading the payload.

## The Tail

### A Tail Streams Gated Activity

The API MUST stream one session's activity as events that resume by id and can start from any point in
the transcript.

A tail MUST show output only once its deciders have passed.

A viewer of a tail MUST hold a read grant on the session.

A tail MUST be relayable from any node through a bounded buffer, so that a slow viewer is dropped and
never slows the session.

Live ungated output on a tail MUST be a privileged option marked as such.

## Webhooks And Event Hooks

### Webhooks Let An External System Publish

A webhook MUST be an authenticated endpoint mapped to a principal.

### Event Hooks Deliver A Session's Status

The API MUST deliver a session's lifecycle and status events to the destinations its definition names.

An event hook's destination MUST be registered per tenant and referenced by name.

An event hook MUST NOT affect the session whose events it carries.

An event hook delivery MUST leave through the egress point.

## Protocol Hygiene

### Replies Are Legible And Traceable

A refusal MUST carry a stable error code that does not change with its wording.

A request over a size cap MUST be refused with a status that names the cap it exceeded.

The API MUST propagate distributed trace context, continuing a caller's trace or minting a fresh one.

### Development Modes Stay On Development Stages

A development-only authentication or transport mode MUST refuse to start outside the development stage.
