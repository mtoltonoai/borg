# Unapproved Implementation Specification Proposal

Nothing in this draft is approved for inclusion in the implementation specification.
It preserves the material written before Matthew required direct approval for every
addition. Statements labeled selected describe local development choices already
made under the original implementation authorization; they are not approved
architecture or approved specification prose. Matthew's three tenancy answers are
recorded exactly below, but approval of those answers does not approve surrounding
prose. The implementation specification now contains only the directly approved tenancy choice.

Review the entire draft before admitting any part. No original normative file,
record, or gate selection was changed. New research findings belong in separate
proposal drafts until directly approved.

---

# Borg Implementation Specification

> Descriptive implementation record, maintained alongside the implementation.
> This file adds no normative requirements and is not a gate input. It does not
> replace or amend the constitution, contracts, core, outcome files, or their
> decision record. Those documents remain authoritative.

## Scope And Sources

Matthew requested a new, standalone implementation on borg-builder and a
separate implementation specification in this repository. This work is separate
from the Fleet-owned Borg/Paperclip comparison. Local implementation and tests
are authorized. Fleet enrollment, Agent Lab enrollment, other-host operations,
Fleet service changes, publishing, deployment, and new access grants are outside
this task.

The source baseline is commit
`64bf7942d5c4da0e878f32b798cdf1ce6ea13186`. The source checkout was clean before
this work. Implementation code belongs in the separate `borg-runtime` repository,
beside this repository, following [the entry point](../../../../AGENTS.md). Its
`spec-snapshot/` preserves the source baseline byte for byte; `spec-lock.json`
records the source commit, each file's SHA-256, and an aggregate manifest digest.
The original record and gate selection are preserved, including their explicit
unconfirmed defaults. This document records this implementation's answers and
decisions separately at Matthew's request.

Read before implementation: the entry point, constitution, all fourteen core
files, all nine contracts, the decision method, both decision documents, the
declared defaults, the deferred register, and the active outcome requirements.
No Borg-local or home `.agents/skills` exists. The nearby monorepo's skill
directory reserves its skills for explicit invocation; none is needed for this
standalone Rust milestone. The `s2n-quic` fork was inspected without changes.

## Decision States

- **Selected**: a reversible implementation choice within the implementer's
  authority, or a choice explicitly confirmed by Matthew. The entry identifies
  which of those applies.
- **Proposed**: a recommendation, with alternatives and rationale, not adopted.
- **Pending**: a material question still requiring information or confirmation.
- **Deferred**: the original register's trigger has not occurred. Its invariant
  is preserved; no additional mechanism is selected.

A passing local test does not convert a proposal into a selected architecture.
This document is updated whenever implementation changes a choice, reveals a
fork, completes a milestone, or changes a blocker. Each change keeps the source
requirement links, rationale, actual status, and validation current.

## Operator Decisions

### Tenancy Scope

**Choice directly approved: one-tenant with dedicated hosts and stores. The additional prose in this draft remains unapproved.**

Matthew's screening answers on 2026-10-10:

1. One team; no other has asked.
2. Data must not share hosts or stores with other teams.
3. Only platform operators create the initial tenant.

One-tenant satisfies these facts without a tenant registration path or a router.
Registration in a shared cluster would conflict with the isolation answer if a
second team joined. Cells provide that stronger boundary for multiple tenants,
but add unused infrastructure while only one tenant exists. A known second team
reopens this decision before admission; recommend cells while the isolation
requirement remains. Matthew answered the confirmation with "Adopt". Only the directly approved
choice has been recorded in `spec/implementation.md`; this explanatory prose has
not been approved for inclusion.

References: [tenancy decision](../../../decisions/tenancy-scope.md),
[one-tenant outcome](../../../decisions/tenancy-scope/one-tenant.md),
[tenant boundary](../../../core/tenancy-and-confidentiality.md#the-tenant-is-the-boundary),
and [irreversible choices](../../../../constitution.md#irreversible-choices-are-decided-first).

No deployment tenant, tenant key hierarchy, production durable format, or
registration path has been created. Two synthetic tenants in future simulation
tests remain necessary even under one-tenant.

### Execution

**Proposed: direct. Recording held pending the latest clarification instruction.**

No Borg harness implementation exists in either inspected checkout. That does
not establish whether Matthew has a harness elsewhere that should be preserved.
The alternatives are direct execution and hosted harness. Direct is the
development default; preserving a harness that accepts per-call credentials,
routes tools through dispatch, and reports transcript and usage could favor
hosted harness. Matthew answered that agents are configuration or not yet running, and that
existing credentials are fixed on the host or in the program and cannot be
changed. Those answers favor direct and rule out hosted harness under this
decision document. A separate Adopt answer was received, followed by a
clarification request explicitly instructing the agent to keep recording pending.
The direct-execution entry has therefore not been written. No model loop or
hosted process has been implemented and no model credential provisioned.

References: [execution decision](../../../decisions/execution.md),
[direct outcome](../../../decisions/execution/direct.md), and
[hosted-harness outcome](../../../decisions/execution/hosted-harness.md).

## Fixed Defaults

No trigger to change a fixed default has been established for the platform being
built. The following remain the original defaults; this is not a new operator
confirmation or a claim of implementation:

| Area | Default | Trigger to revisit |
|---|---|---|
| Collaboration product | None | A needed existing collaboration tool or a collaboration product serving the described population |
| Root-session ownership | Imperative | No tenant system owns and reconciles the root-session list, or reviewed live definitions require controllers |
| Agent code execution | None | A Borg agent must run shell commands or builds |
| Authority widening | Checkers only | Repeated runtime human decisions or record-based autonomy needs described by the register |
| Durability | Self-durable store | A full-cluster-loss drill fails the required loss or restore bounds |
| Conformance | Gate only | The independent consumers, approval, or evidence conditions in the register occur |

The implementer's own use of a shell to build Borg is not a decision that Borg
will host tenant shell execution. Definitions, collaboration products, workspace
services, billing, memory services, and fair-share policy remain governed by
[the deferred register](../../../decisions/deferred.md).

## Selected Implementation Choices

### Repository And Language

**Selected by implementer, 2026-10-10:** a separate Cargo workspace with a
`components/borg-sim` Rust library; initially standard library only. The installed
toolchain is Rust 1.99.0, edition 2024. The package is not publishable. This keeps
the first milestone reproducible offline without selecting a transport, store
vendor, cryptography library, or provider. The simulation library is not a
released shared framework and is not consumed by a product yet.

References: [entry point](../../../../AGENTS.md),
[implementer discretion](../../../decisions/README.md#what-the-implementer-decides-without-asking),
and [shared framework](../../../core/shared-framework.md).

### Deterministic Time And Deadlines

**Selected by implementer, 2026-10-10:** explicit monotonic simulation time with
per-node wall-clock offset, drift, and step injection. An ordered deadline queue
supports insertion, cancellation, replacement, and earliest-deadline access;
equal instants have deterministic order. Tests advance time directly rather
than sleep on an operating-system clock. Simulator integer widths and clock
units are internal test representations, not a stored-format decision.

References: [time as a dependency](../../../core/simulation-and-conformance.md#time-is-a-dependency)
and [framework deadlines](../../../core/shared-framework.md#deadlines-on-the-environment).

### Transaction Simulator

**Selected by implementer, 2026-10-10:** in-memory versioned snapshots and atomic
multi-key transactions with validation of explicitly selected keys, generation
fences, append/trim queue operations, and version-addressed watch notifications.
Mutations apply to the current value only after all checks succeed. A publisher
append is preserved when a concurrent consumer trims an earlier queue prefix.

Failure injection distinguishes a definite failure before commit from a lost
reply after commit. A fence read failure is not reported as owner supersession.
The caller can resolve an ambiguous result by reading committed state while
checking the fence. These are simulator primitives, not a session executor or a
claim that process loss preserves data. The simulator has no file or network
I/O, no tenant key material, no cryptography, and no production tenant API.

References: [ownership fence](../../../core/sessions-and-partitions.md#ownership-is-fenced-by-generation),
[commit conflicts](../../../core/sessions-and-partitions.md#a-commit-conflicts-only-on-the-fence),
[inbox application](../../../core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order),
[store failures](../../../core/failure-handling.md#a-store-conflict-or-outage), and
[chosen transaction failure](../../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator).

### Requirement References And Coverage

**Selected by implementer, 2026-10-10:** exact file, section slug, and quoted
sentence citations using the original Duvet markers, with `type=test` for test
citations. A local inventory checks the snapshot and citations and reports
missing pairs. It never treats presence as witnessed behavior or promotion
evidence. The full source selection remains 32 active normative files and 14
inactive outcome files. No outcomes are removed to make the milestone pass.

`duvet` is absent from the devbox. Its installation and full report remain
outstanding. The fork's compliance wrapper is not used: it installs tooling and
disables required test checks. A trustworthy release gate additionally needs
executed-test linkage, the reviewed governing-requirement exclusions, and a
trusted runner outside the agent's writable workspace.

References: [requirement gate](../../../core/simulation-and-conformance.md#the-requirement-gate)
and [evidence provenance](../../../contracts/evidence-record.md#evidence-is-the-checkers-own-record).

## Material Forks Before Further Implementation

| Choice | Status and alternatives | Recommendation and reason | Decision boundary and source |
|---|---|---|---|
| Durable substrate | Pending: existing transactional store, or purpose-built replicated store | Compare candidates using the same simulator/probe suite before selecting; a local key-value file alone is insufficient | Before persistent tenant data; [durable substrate](../../../record.md#durable-substrate), [self durability](../../../decisions/durability/self-durable.md) |
| Store transaction semantics | Selected for simulator; production capability pending | Explicit fence validation plus atomic append/trim, pinned-version reads, watches | Candidate must demonstrate these without conflicts on concurrent inbox appends; [sessions](../../../core/sessions-and-partitions.md) |
| Transport and s2n-quic | Deferred selection: fork or another authenticated multiplexed transport | Do not add a transport until a real cross-node/store contract exists; inspect fork support then | No concrete transport is required by the spec; [authenticated peers](../../../core/shared-framework.md#node-transport-is-authenticated) |
| Durable IDs and encoding | Pending: tag registry, exact canonical text/binary grammar, evolution strategy | Pin the registry and fixtures before first persisted ID; preserve unknown tags | [stored form](../../../contracts/stored-form.md#format-evolution), [portable IDs](../../../core/tenancy-and-confidentiality.md#ids-are-portable) |
| Encryption and deletion | Pending: concrete AEAD modes, owner-key destruction design, managed key service | Prove deletion through backups and caches before a real tenant is accepted; no home-grown crypto | [encryption](../../../core/encryption-and-blobs.md), [key hierarchy](../../../contracts/stored-form.md#the-key-hierarchy) |
| Governance engines | Pending: policy engine and sandbox, builtin decider hosting | Keep the fixed hook interface; prove deterministic bounded execution and fail-closed behavior before external effects | [governance](../../../core/governance.md) |
| Identity and inference | Pending: identity providers, tenant capacity sources, broker backends, provider formats | Use simulated dependencies first; prepare concrete integrations for Matthew before any access grant | [identity](../../../core/identity-delegation-and-secrets.md), [capacity](../../../decisions/execution/direct-model-turns-and-scheduling.md#capacity-sources) |
| Root-session controller | Default imperative; actual owning tenant system unknown | Verify a system can create, reconcile, and receive lifecycle events before product integration | Reopen declarative only if register trigger holds; [imperative](../../../decisions/session-management/imperative.md) |
| Backup and deployment | Pending: dedicated deployment boundary, control plane, storage domains, loss/recovery objectives | Implement drills before deployment; standalone builder is not a production node | [durability and deployment](../../../core/durability-and-deployment.md) |
| Conformance tooling | Pending: Duvet availability, governing list location, trusted execution evidence | Retain all uncovered requirements and refuse promotion; use local inventory only for development | [conformance](../../../core/simulation-and-conformance.md), [evidence](../../../contracts/evidence-record.md) |

The specification's category defaults are not enough to choose a production
engine or provider. Those are proposed and discussed before an irreversible
implementation depends on them. The original definition-source contract is in
the active selection while its prose makes it conditional on a deferred
declarative outcome; retain the selection and flag applicability for gate design,
rather than silently exclude requirements.

## Declared Defaults Used By This Milestone

This milestone builds only to the following groups from [part B](../../../record.md#declared-defaults):

- **Durable substrate:** transactional key-value semantics, explicit selected-key
  validation, and watch-version reads; a configurable 100-key transaction bound
  in scenarios, with smaller bounds in boundary tests. This does not confirm a
  concrete store, on-disk layout, partition hash, or data envelope.
- **Simulation:** deterministic input and faults, 1,000 seeded interleavings for
  the generated scenario. No full dependency simulator/probe parity is claimed.
- **Requirement gate:** exact quoted-sentence identity, the unchanged default
  selection, and a content-addressed snapshot. Duvet itself remains unavailable.

Other part-B values have been read but are not applied or silently marked
confirmed. Before the milestone that uses them, present the full affected groups
to Matthew together and record adjustments. Unknown provider endpoints, key
custody, retention objectives, thresholds left to measurement, and deployment
limits remain open; a test fixture is not a production default.

## Implementation Sequence And Validation

1. **Implemented initial subset: simulation foundation.** Clock/deadline primitives, transactional
   store simulator, failure injection, snapshot/watch behavior, exact citations,
   and adversarial tests. No production runtime conformance is claimed.
2. **Next: session transition kernel.** Opaque inbox processing, atomic
   state/cursor commits, durable dedupe semantics, ambiguous-commit recovery,
   concurrent append/trim, owner moves, and two synthetic tenants against a
   reference model. Do not claim tool/model/governance behavior from this kernel.
3. **Pending architecture: real store and sealed records.** Select the substrate,
   settle durable format and tenant boundary, verify simulator against the real
   store, add encryption/key deletion and recovery fixtures, then restart drills.
4. **Pending: governed sessions and interfaces.** Authentication, grants, fixed
   hooks, safe configuration, tool dispatch, timers, usage/audit, frontend, and
   the selected execution loop. Every external dependency gets a simulator and
   the same expectations exercised through its real probe.
5. **Pending: integration and release gate.** Full failure-table scenarios,
   deterministic generated sequences, actual two-tenant isolation, backup and
   restore, requirement completeness, trusted evidence, and authorized release.

Local validation commands live in the implementation repository's README.
Results are recorded below only after execution. No local result is a production
deployment, an access grant, or evidence from a trusted promotion runner.

## Runtime Verification

The local Codex app-server's supported read APIs identify Codex Desktop
`0.163.0-alpha.5`. `config/read` for `/home/dev-user/borg` reports `gpt-6-astra`
and `ultra`. `experimentalFeature/list` reports `code_mode=false`,
`code_mode_only=false`, and `code_mode_host=true`; host support is not code mode.
`thread/loaded/list` reports no loaded threads; `thread/read` for the delegated
thread returns `-32600: thread not loaded`. Therefore active turn model, effort,
and code mode cannot be certified by this metadata. This is reported explicitly;
no persistent configuration or credentials were changed to obtain the mode.

## Progress Log

- **2026-10-10:** Began standalone implementation after reading the original spec.
  Verified clean Borg and s2n-quic checkouts. Recorded the runtime verification
  limitation. Created a separate local Rust workspace and immutable source
  snapshot. Started parallel clock, store-simulator, and citation-inventory work.
- **2026-10-10:** Added this maintained implementation specification at Matthew's
  explicit request. Recorded the three tenancy answers; confirmation and
  execution screeners remain pending. No original normative text was changed.

## Validation Record

The initial subset passes 32 Rust tests (1 unit, 14 store, 17 time), formatting,
and Clippy with warnings denied. The 19 Python inventory tests pass. Generated
deadline tests cover 1,000 seeds of 200 operations each against a vector model.
The local inventory reports 32 active files, 1,816 requirements, 1,768 absolute
requirements, six paired citation presences, and 1,762 absolute presence gaps.
These are presence counts, not full behavioral coverage. Strict inventory exits
1 as expected; `duvet report` exits 127 because Duvet is absent. See the separate
implementation repository's `docs/validation.md` for scope and limitations.

DC QUIC research is staged in `proposals/dc-quic-patterns.md`, entirely unapproved
for adoption or inclusion in the implementation specification.
