# Implementation Assumption Audit

Review draft only. Implementation writes are paused at Matthew's request. This
file records findings and alternatives; it approves nothing and adds nothing to
`spec/implementation.md`. Existing code is preserved, including unfinished work.

## Preserved State And Evidence Boundary

The last complete checkpoint is `borg-runtime/artifacts/conformance/run-_pijyq0s`:
199 runtime Rust tests, two compile-fail API tests, 50 Python tests, formatting
and Clippy passed. Actual Duvet 0.4.3 reports 134/1,817 paired links, 1,683 missing
implementation links and 1,683 missing test links, and zero exact stale citations.
124 scope-reviewed implemented requirements have local line-execution witnesses;
31 remain partial and 11 concretely unimplemented, with others unassessed. This
is local evidence, not real-dependency parity or trusted conformance.

Only `components/borg-sim/src/dispatch.rs` differs from that checkpoint's input
manifest. Its preserved SHA-256 is
`51348b4cb5108e8d2c36bb6b19e873a337a2b0e46f517aea2157a012ad679a72`.
The unfinished change adds sticky non-final state/codec data, deadline recovery,
provisional-unknown handling and a receiver interruption fixture. The writer ran
the existing 17 dispatch tests successfully before stopping. It did not add the
new deadline/fault/late-result test matrix or run a new full conformance check.
Do not apply the prior checkpoint's witnesses to this later source.

No X transcript/freeze code was written. Its messages were design proposals only.
All workers stopped implementation writes and completed read-only audits. There
are no reported pending check processes. No files were reset, reverted or
committed. All 58 original pinned source files still match their hashes; the
approved implementation specification and external proposals are separate from
that original pinned set.

## What Is Approved, Required, Or Merely Experimental

Directly approved implementation decisions remain dedicated one-tenant hosts and
stores, and direct execution. Approved process sections cover development Duvet
checks, DAG-driven parallel development and single-source top-level capabilities.
There are zero approved capability entries. The 25-capability/81-edge declaration
draft is unapproved; its JSON, diagram and order are generated review outputs.

The original spec already requires durable state, disposable node memory,
transactional state/cursor changes, generation fences, dedupe, ambiguous-commit
recovery, owned immutable blobs, pin-before-reference and no referenced-blob
reclamation. It also already requires real authentication, client sealing,
compatible stored forms, deadline/retention rules and real-probe parity. Missing
those behaviors is not an invitation to approve their omission.

[What The Implementer Decides Without Asking](../../../decisions/README.md#what-the-implementer-decides-without-asking)
normally delegates names, layout, language/library choices within declared
defaults, mechanisms and testing. Deterministic containers, seeded tests, checked
arithmetic, private fixture IDs, test bounds and code organization are ordinary
local details when confined to a disclosed experiment. The current explicit pause
overrides that general delegation. No further source changes should be made just
because a detail would ordinarily be delegated.

The decision boundary is whether a choice changes externally observable
behavior, establishes a production dependency/format/trust or persistence
contract, weakens a required guarantee, or commits an interface other components
must rely on. Existing code is evidence of an experiment, not evidence of direct
approval of such a choice.

## Concrete Review Items

| ID | Existing experiment or assumption | Required boundary and alternatives for review |
|---|---|---|
| D1 | `topics.rs` sets a new/rejoined subscription after the current topic head and reevaluates active membership on each fan-out batch. Unsubscribe can suppress already-published, undelivered work. | The original session-topic rules specify order, retention and indexes, but do not select these join/removal boundaries. Review publish-time membership versus delivery-time membership, and current-head start versus an explicit retained position. The viewer-only no-history rule does not decide session semantics. Recommendation: specify the session subscription/resume contract before extending this API. |
| D2 | `TimerEdit` requires an observed revision; cancellation retains a name record; the fixture cap counts canceled names. | Stale-fire rejection is required; this CAS API and capacity accounting are not uniquely prescribed. Compare version-checked edits with stable request receipts, and separate active-timer capacity from bounded invalidation history. Do not expose canceled-name exhaustion as an accidental product rule. |
| D3 | C accepts caller-supplied loaded definitions, grant snapshots and pipelines. It gates before recording, then evaluates the tool-call pipeline again at send and rejects changed transformed arguments. | Current grants/revocation and gated arguments are required. Review a prepared authorization bound to authoritative configuration/grant identities versus a specified re-evaluation path with stable transform/question identity. A numeric revision and `VerifiedCaller::synthetic` do not establish freshness or authentication. |
| D4 | C uses mutually exclusive `Idempotent`/`SideEffecting` variants, committed-send attempt counting, fixed retry delay and a combined retained-record bound. | The original contract states idempotency and side effects independently; the reduced enum is an implementation gap, not a valid replacement contract. Review retry-budget charging at preparation/presentation/admission and retention accounting separately. Required open/unknown bounds and original semantics must remain; do not ask for approval to omit them. |
| D5 | `configure_processing` immediately changes a synthetic revision/lease and invalidates work. D0 validates immutable, tool-only grant fixtures eagerly. | Production configuration must adopt at the next turn boundary and not between a tool call and result. A pending-revision/adoption protocol and current-authority read boundary need design before real integration. The fixture method is not approved live reconfiguration; missing grant expiry/conditions/issuance are missing required work. |
| D6 | Usage uses pending work plus a once-only final stream record; its sealed transaction plan permits one usage action per transaction. | One record per work identity is required. Pending/committed/emitted accounting, budget visibility and multi-decider composition are not settled by this fixture. Compare a specified finalization protocol with a versioned single-record protocol if compatible with the original stream rules. Retain the one-action limitation explicitly until concrete transaction semantics are agreed; do not construct a universal abstraction speculatively. |
| D7 | Serial in-memory commits, aggregate tenant records, whole-partition lease updates, pinned routes, and separate synthetic receiver state model ownership. | Fences and atomic effects are required; this topology/key packing is not a selected production store/coordinator. Retain it as a bounded model, or review production layouts/composition against a concrete durable adapter's key/conflict/ownership contract. Large-partition refusal is a fixture limit, not a scalability decision. |
| D8 | `OutcomeUnknown` is injected only after apply; other `StoreError` outcomes promise no commit. Snapshots and many histories/tombstones are retained for the entire bounded scenario. | Real timeouts/partial failures cannot inherit definite non-commit certainty without evidence. Expand the fault/probe contract to cover uncertainty on either side of application and implement retention under the original guarantees. These are missing tests/behaviors; they do not reopen whether committed state must survive. |
| D9 | Positional little-endian fixture codecs, numeric IDs/digests, plaintext fake content and logical blob deletion are used locally. | Keep these strictly disposable. Before real durable data, review a conforming tagged format/compatibility strategy, sealing/key lifecycle and actual durable substrate. Current codecs cannot simply be adopted as the required stored form; logical deletion is not key destruction and transfer currently lacks re-sealing. A production restriction on transfers with live pins would require a justified behavioral decision. |
| D10 | New unfinished deadline code permanently records having received a non-final answer and maps unresolved reconciliation to provisional unknown. | The source forbids rerun at the deadline and requires reconcile/unknown handling. Review the exact state transitions and their interaction with subsequent recovery before completing the change; preserve call identity, late-result horizon and once-only usage. The source-only edit is not a tested acceptance result or an approved durable schema. |
| D11 | X proposed bounded transcript append/range freeze on one store, followed later by whole-session freeze. Existing B owns its own store and there is no delete mutation. | No X implementation exists. Compare retaining independent models with an explicitly approved same-store range-publication seam. Decide the initial acceptance slice: range freezing versus full zero-session-key freeze. Neither option may lose racing appends, reclaim referenced bytes, or trim inboxes before their applied history is preserved. No generic transaction abstraction is approved. |
| D12 | The draft graph treats prerequisites as required interfaces/invariants rather than completion of every predecessor acceptance criterion; C currently depends on the whole timer capability T. | Review those dependency semantics and the specific 81 edges. In particular, C currently needs an explicit clock/deadline interface, not session timer recurrence/delivery; compare a narrower SIM/deadline dependency with the existing C-from-T edge. Keep the generated draft as a review baseline until the choice is approved. Neither Duvet nor a topological sort proves these edges. |

These are review items, not assertions that every fixture detail needs an
operator vote. D1-D6 and D10 describe behavioral/interface questions or gaps most
relevant before further integration. D7-D9 separate prototype conveniences from
future production commitments. D11 is an unimplemented next-slice proposal. D12
is pending development-structure approval. Group ordinary delegated details in
notes rather than inflating the implementation specification with each variable,
container, numeric width or test parameter.

## Original Sources To Use In Decisions

- [Session-topic membership, order, retention and indexes](../../../core/events-and-subscriptions.md#subscriptions-come-from-definitions-sessions-and-grants); do not generalize viewer-only behavior to sessions.
- [Timer identity and cancellation](../../../core/timers.md#a-timer-s-name-is-unique-within-its-session), [fire invalidation](../../../core/timers.md#a-fire-is-exactly-once), and [configured timer bounds](../../../core/timers.md#empty-wakes-are-bounded).
- [Independent tool semantics](../../../contracts/tool-call.md#the-server-declares-the-semantics), [dispatch gating](../../../core/tools-and-dispatch.md#the-dispatch-sequence), [revocation](../../../core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration), and [long-run recovery](../../../core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs).
- [Configuration adoption](../../../core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary), [grant narrowing](../../../core/identity-delegation-and-secrets.md#delegation-only-narrows), and [usage identity](../../../contracts/usage-record.md#every-attempt-is-recorded-once).
- [P5](../../../../constitution.md#p5-durable-state-disposable-memory), [durable store guarantees](../../../core/shared-framework.md#durable-store-durability), [self-durable outcome](../../../decisions/durability/self-durable.md#the-store-survives-the-loss-of-every-node), [client sealing](../../../contracts/stored-form.md#scheduling-fields-are-readable-without-keys), [durable compatibility](../../../contracts/stored-form.md#every-durable-format-is-self-describing), and [simulator/probe parity](../../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator).

## Suggested Review Order, Not Implementation Authorization

1. Review the additional top-level contract wording, 25 capability entries and
   prerequisite meanings/edges. Only the single-source process is already saved.
2. Record the approved scope of the existing models and settle D1-D6/D10 for the
   next intended integration slice; correct source-mandated gaps rather than
   treating them as optional architectural alternatives.
3. Before any real adapter, select the concrete store candidate and permitted
   persistent test substrate/survival boundary. An existing qualified store versus
   building/owning the mechanism is the material choice. Self-durable survival is
   already required; no fsync/quorum/replication policy is selected here.
4. Review sealing/key/format and any necessary runtime/dependency choices before
   irreversible data or production integration. Keep X and other new slices paused
   until their relevant boundary is approved or shown to be an ordinary detail
   entailed by the original requirements.

The earlier `proposals/implementation-spec.md` still contains stale pending-
execution wording. It is an unapproved historical proposal and does not override
the explicit direct-execution adoption in `spec/implementation.md`. No stale
proposal is silently promoted into the approved specification.
