# Contract — The Signed Assertion

> **CONTRACT.** This document pins the statement of identity the platform presents to every service it
> calls on a session's behalf: tool servers, definition sources, hook destinations, the credential broker,
> and the workspace service. These parties cannot read the durable store, so the assertion is how they
> learn who is calling and what the call may do. It is versioned and changed only by the coordinated act
> the constitution's Governance Floors describe. Its requirements realize
> [Governance Floor "Identity Comes From Authentication"](../../constitution.md) and trace to
> [overview §9](../overview.md) and [overview §7](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading. The signature scheme, the key custody, and the lifetime are declared defaults, not requirements.

## Purpose And Scope

A service the platform calls must scope, dedupe, and fence what it does to the session that called it,
without trusting an argument the model wrote. The assertion is a short-lived, signed, call-bound
statement derived from the session's grants at the moment of the call. It is a capability-style token
that lives only at the edges: it is never the source of truth, which is the grant data in the durable
store, and it is never stored by the platform beyond the call it was minted for.

## Claims

### An Assertion Names The Call It Is Bound To

An assertion MUST carry the call id of the one call it was minted for.

An assertion MUST carry the audience that names the one service it is addressed to.

### An Assertion Names The Actor

An assertion MUST carry the tenant the session belongs to.

An assertion MUST carry the session's principal.

An assertion MUST carry the session's on-behalf-of chain in order from the outermost delegator to the
session.

An assertion MUST carry the session id.

An assertion minted for a fork MUST name the fork rather than its parent.

### An Assertion Carries What The Call May Do

An assertion MUST carry the scopes in play for the call, derived from the session's grants at the moment
of issue.

An assertion MUST carry the session's lease generation.

An assertion MUST carry an expiry no later than the configured assertion lifetime after issue.

An assertion MUST NOT carry a credential, a secret, or transcript content.

## Signature And Keys

### The Signature Is Asymmetric

An assertion MUST be signed with an asymmetric scheme, so that a verifier holding the verification key
cannot mint an assertion.

Verifying an assertion MUST NOT require a call to the key service.

### Keys Are Published And Rotated Without A Gap

The platform MUST publish the set of verification keys at a location every verifier can fetch.

A rotation MUST keep the previous signing key verifiable until the last assertion signed under it has
expired.

A rotation MUST NOT fail a call that was in flight when it began.

## Verification

### A Verifier Refuses What Does Not Match

A verifier MUST refuse an assertion whose expiry has passed.

A verifier MUST refuse an assertion whose audience is not the verifier.

A verifier MUST refuse an assertion presented for a call id other than the one it carries.

A verifier MUST refuse an assertion signed by a key outside the published set.

A verifier MUST refuse an assertion that names a tenant other than the one the call is addressed to.

A verifier that cannot fetch the key set MUST fail closed.

### The Assertion Scopes The Call

A service MUST scope everything a call does to the assertion's tenant, session, principal chain, and
scopes before the service's own authorization runs.

A service MUST take the acting identity of a call from the assertion rather than from an argument or a
header the caller controls.

A service MUST refuse a call whose lease generation is older than the highest it has recorded for that session.

That refusal MUST be distinct from an authorization failure.

## Where An Assertion Is Presented

### Every Outbound Call Carries One

Every tool call the platform makes MUST carry an assertion addressed to the tool server.

Every read a definition controller makes MUST carry an assertion that names the binding's tenant.

Every credential lease the platform requests MUST carry an assertion addressed to the credential broker.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed verifiers, or else carry an explicit
version increment.

A change to this contract that is not additive with respect to deployed verifiers MUST carry a stated
migration path.
