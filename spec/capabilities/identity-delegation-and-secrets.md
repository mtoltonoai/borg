# Capability — Identity, Delegation, And Secrets

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines the principal on every action, the on-behalf-of chain, grants and the narrowing-only rule,
> per-request authority for shared sessions, the credential broker, attribution into external systems, and
> what the substrate owns versus what higher levels supply. Requirements realize
> [Governance Floor "Identity Comes From Authentication"](../../constitution.md),
> [Governance Floor "Secrets Never Enter The Model's World"](../../constitution.md), and
> [Core Principle V](../../constitution.md) and trace to [overview §9](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The assertion is pinned by the
> [signed-assertion contract](../contracts/signed-assertion.md); identity providers and token exchanges
> are declared defaults.

## Purpose And Scope

The substrate owns the invariants and higher levels own the policy. This capability fixes the invariants:
a principal everywhere, a chain that travels, grants as data that only narrow, assertions at boundaries,
secrets kept out of the model's world, and honest attribution. Identity providers, policy content, the
broker's backends, and delegation flows plug in.

## A Principal On Every Action

### Every Action Has A Principal

Every action MUST be attributed to a principal.

A session MUST run as a principal.

A session's configuration MUST name whom it acts on behalf of.

The on-behalf-of chain MUST travel with every model call, tool call, outbound message, control
operation, and event.

### Principal Ids Name Their Provider And Tenant

A principal id MUST name its identity provider.

A principal id MUST name its tenant.

A person MUST be able to belong to more than one tenant under one provider-qualified id.

### A Definition Has Its Own Principal

A definition's principal MUST be its tenant, source, and name.

A definition MUST name no principal of its own beyond this.

A definition MUST carry no grant or credential.

A keyed instance MUST act for its requester with the intersection of the definition's grants and the
requester's.

## Enforcement And Fail-Closed

### Enforcement Is At The Fixed Hooks

Identity and authorization MUST be enforced at the control-operation, tool-call, output, and tool-result
hooks.

A hook that cannot obtain an authorization decision MUST fail closed.

## Grants

### A Grant Is Data

A grant MUST record who delegates, to which session or definition, which actions and resources, its
expiry, its conditions, and its tenant.

A grant MUST be replicated configuration cached in the partition, so that checking it is a local
evaluation rather than a network call.

Deleting a grant MUST revoke it at the next decision.

Every grant MUST be visible to audit and to analysis.

### Delegation Only Narrows

A sub-session's grants MUST be references to its parent's rather than copies.

Every decision MUST walk the chain of grant references.

Revoking an ancestor's grant MUST revoke its descendants'.

A grant from an operator MUST NOT exceed what that operator holds or the role's ceiling.

A sub-session's budget MUST be carved from its parent's.

The model-request hook MUST check the shared budget before every call.

### A Shared Session Acts For Whoever Asked

A decision for a session acting on another principal's event MUST carry the requester's principal and
chain.

The effective authority of a session acting for a requester MUST be the intersection of the session's
grants and the requester's.

A requester MUST NOT borrow the session's reach.

## Assertions

### Services Learn Identity From An Assertion

For each call or lease, the platform MUST present a short-lived signed assertion bound to that call, of
the session's principal, chain, tenant, scopes, and lease generation.

A capability-style token MUST live only at the edges and be short-lived.

A capability-style token MUST NOT be the source of truth.

## Secrets

### Secrets Stay Out Of The Model's World

A secret MUST NOT appear in model context, a transcript, session state, an event, an observation record,
a metric, or a log.

A tool spec MUST name a credential it needs by reference.

At dispatch the core MUST obtain a short-lived credential from the broker for the principal, chain, and
target, and hand it only to the executor.

A tool result and an event payload MUST pass the structured-secret decider.

A block's reason MUST NOT reintroduce a secret into context.

## The Credential Broker

### The Broker Is Its Own Trust Boundary

The broker MUST decide on the assertion, since it cannot read the durable store.

The broker's own base credentials MUST carry no delegated identity of their own.

A credential the broker issues MUST be scoped to the principal, its chain, and the target.

A credential MUST be leased for one call.

A credential MUST NOT be reused for another call.

Every credential issued, and every broker call that fails, MUST be an event naming the grant and the
identity requested, never the secret.

## Attribution Into External Systems

### No Human Credential Runs Automation

A node's own access to its dependencies MUST come from an instance role that refreshes without a person.

A human sign-on credential MUST NOT be used to run automation.

### The Operator Is Carried Into The Audit Trail

Each operator's sessions MUST act with credentials issued for that operator's delegation, attributable
and revocable per operator.

Where a target system offers an on-behalf-of exchange, the platform MUST present the operator as the
subject and itself as the actor.

Where a target system offers no on-behalf-of path, the platform MUST act as itself and record the
operator in its audit and in the visible text.

The platform MUST NOT present an operator to a target through a path the target does not verify.

## Human-Only Actions

### Actions Policy Reserves For People Defer

An action company policy reserves for a person MUST carry a call-type tag.

A substrate default MUST defer such an action.

An action on the never-automate list MUST be blocked by a substrate policy no definition can loosen.

A change MUST record whom it was authored for.

A change MUST be approved only by a person who is not in its authored-for set.

## Audit

### Every Decision And Credential Is Audited

Every authorization decision MUST be published as an event naming the grant or policy that decided it.

An audit event MUST hold ids, principals, and decisions.

An audit event MUST NOT hold content.

Audit events MUST be kept apart from session data, with their own retention, so that destroying a
tenant's keys deletes its content but not the record of who did what.

## The Substrate And Higher Levels

### Higher Levels Supply Providers, Policy, The Broker, And Flows

The mapping from external identities to principals MUST be supplied by an identity adapter at a higher
level.

Policy content MUST be supplied by the tenant rather than carried by the core.

The credential broker's backends MUST plug in through one interface, like an executor.

The identity adapter MUST write an operator's grant when the operator starts a session, capped by the
operator's permissions and the role's ceiling.
