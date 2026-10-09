# Borg — Declared Defaults

> **What this document is.** The one place a concrete choice is recorded that a normative requirement
> deliberately leaves open. The constitution, the contracts, and the capability specifications name no
> engine, store, provider, library, numeric width, or path; where behavior depends on one, the
> requirement says "the configured X" or "the declared default", and the value lives here. This document
> is descriptive: it carries no RFC-2119 requirements. Changing a value here is a configuration change,
> not a specification change, unless a requirement's wording depends on it.
>
> These are the defaults a conforming realization adopts. A deployment records the concrete products that
> back each category; this document names categories and open standards, not a vendor or an account.

---

## Durable substrate

- **The durable store** is a transactional key-value store with durable actors: atomic multi-key commits,
  read-validation of named keys, and a watch that wakes a reader when a key's version advances.
- **The shared-framework components** (the partitioned actor set, reads at the woken version, the deadline
  queue, the per-node simulated wall clock, envelope encryption, the blob store, the change feed, the
  live-stream hub, rate limiting, caller authentication, and the write-ahead log) are built against that
  store and shared across the products.
- **Partition bit count** k = 12 (4,096 partitions) by default, node configuration, split by doubling.
- **Transaction key bound** 100 keys per transaction, advisory.
- **Per-session key count** two keys for an active session (the inbox queue; and state, config, and
  cursor together); a frozen session is an element of its partition's index key.
- **Transcript head bound** 64 KiB.
- **The durability backstop** is a wide-column store attached to the cluster, carrying sessions across a
  cluster replacement until the store's own write-ahead log makes it unnecessary.

## Models and the inference layer

- **Providers** the inference layer renders to two provider wire dialects behind one provider-neutral
  transcript, reached through a model-inference service.
- **Transport** HTTP/2, token-bearer auth by default, request bodies streamed, no request or response
  compression on the native routes.
- **Compaction method** the provider's native compaction, with the platform's own summarizer as a
  fallback.
- **Prompt-cache lifetimes** a short and a long lifetime where the provider offers them; breakpoints at
  the provider's cap, placed at segment ends.
- **Compaction thresholds** a soft threshold well below the model's context window, with the exact value
  settled by measurement.

## Keys and encryption

- **Cipher modes** AES-256-GCM for values, AES-256-GCM-SIV for immutable blobs.
- **Key hierarchy** a root key per tenant in a managed key service (the tenant's own, or one the service
  holds for it), branch keys per tenant per epoch under a service-held root, and a random data key per
  session.
- **Hash** a collision-resistant hash over stored bytes names a blob and keys content.
- **Decrypt-cache lifetime** five minutes, which is the revocation window.
- **Key-service client** a pool idle timeout under the service's idle-close window, a fresh connection
  before its per-connection request cap, a connect timeout under a second, a hedge in the tens of
  milliseconds, and a deadline of one to two seconds; keys resolve when a session's doorbell wakes it.

## Identity

- **People** authenticate through the people identity provider; **services** through request signing;
  **agents** through the platform's signed assertion.
- **The assertion** is a short-lived asymmetrically signed token (an ES256 JWT is a fit); its signing-key
  custody and rotation are a deployment choice.
- **Acting for operators** goes through an on-behalf-of exchange where a target system accepts it, and as
  the platform's own identity otherwise.
- **The operator identity** is carried into a role session in a colon-free form of at most 64 characters,
  so the external audit trail names the operator.

## Governance

- **Decider engines** pattern deciders on a linear-time regular-expression engine; a classifier as a
  small cross-encoder on CPU behind a deterministic pre-filter; program deciders as sandboxed,
  fuel- and memory-bounded modules; policy deciders as a declarative, analyzable policy engine with a
  temporal extension; a judge through the inference layer; and an external decider over the network.
- **The sandbox toolchain** is the one open question that can force the builtins and the policy engine to
  run behind the decider interface natively until the sandboxed-module toolchain is available.
- **Builtin deciders** structured-secret detection, non-ASCII and emphasis checks, typed-reference
  checks, and an attribution check.
- **Decision-request size bound, per-decider cut-denies, and retry budgets** are per-session configuration
  with platform defaults.

## Frontend

- **Session configuration.** The end state is that a session's standing configuration comes from a
  definition version served by a source, not written through the API; a realization may write instance
  configuration through the API as an interim before definition sources exist.

## Timers

- **Minimum intervals** an unguarded recurring timer at most every 15 minutes; a guarded one every 5
  minutes with a decision hook, and every minute with only local conditions.
- **Per-session caps** 32 timers, 8 of them recurring; backoff doubles up to 8 times; a note is at most
  1 KiB; up to four guard conditions.
- **Clock-skew bound** a step over 1 second is an event; a disagreement with the store's commit
  timestamps over 30 seconds holds a session's timer fires.

## Scale and deployment

- **Scale target** hundreds of thousands to millions of sessions, hundreds of operators across dozens of
  projects, with thousands of viewers.
- **The deployment control plane** operates the platform as its own cluster type, never sharing a cluster
  with another workload.
- **The first tenant** is bootstrapped with a stage's first cluster rather than registered.
- **Stages** development, two pre-production stages, and production, each paired with a stage of the
  collaboration platform.

## The collaboration platform

- **Store** a relational database that runs on many servers; **live updates** an outbox at a gapless
  per-tenant revision with a doorbell and a server-sent-event stream; **deployment** a container service
  behind a load balancer with its own pipeline; **search** the database's own full-text and trigram
  matching.

## The workspace service

- **Substrate** a host substrate that can run tools unprivileged, keep them off the instance metadata
  path, confine egress to one point, and kill a call's processes when the call ends.
- **Snapshots** content-addressed packs chained to the previous snapshot; **credentials** a per-call
  container-credentials endpoint with short claimed expiries.

## Spec-driven design

- **The requirement-gate tool** extracts every RFC-2119 sentence and maps it to the code and test that
  cite it, exporting a stable, content-addressed form that citations resolve against.

## Simulation

- **The simulation framework** drives every component deterministically; **the entropy source** supplies
  inputs and faults; a contract test pairs each dependency's simulator with a probe against the real
  dependency.

## This specification's own gate

- **The gate configuration** is `.duvet/config.toml`: the specification side is hand-owned and lists the
  normative files; the source side is the globs of whatever implementation cites them, which lives
  outside this repository. The descriptive files (this one, the overview, the glossary, and the
  traceability map) carry no RFC-2119 requirements and are not listed as specifications.
