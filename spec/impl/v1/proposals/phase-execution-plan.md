# Phase-Specific Execution Plan Proposal

Status: the [seven-stage structure](../implementation-phases.md), linked
capability-document layout and stable capability-ID references are approved.
The detailed M01-M18 milestones, criteria and readiness dependencies below remain
UNAPPROVED. The minimal declaration schema and execution-plan tooling are now
approved separately in [Duvet Cap](../implementation.md#duvet-cap-declaration-contract).
This proposal does not adopt substantive declarations or authorize runtime work.

The complete current requirement allocation and structured milestone declarations
are now in the [linked draft planning packet](implementation-draft/README.md).
The M01-M18 list below is retained as an earlier illustrative proposal, not a
second authored input to the current generated plan. Its details remain
unapproved; the current draft uses the same approved seven-stage structure.

## Recommendation

The approved seven stages are milestone views, not seven indivisible work
batches. The finer candidate milestones below provide shorter feedback cycles.
Their count is not fixed, and their detailed criteria are not approved by the
seven-stage decision.

Generate a phase-specific execution view from the authoritative implementation
specification set once its capability and phase-milestone decisions are approved.
Keep one authoritative definition of each capability, prerequisite and approved
phase criterion; the generated plan is an output, not a second authored plan.

Use the [phased implementation specification](../implementation-phases.md)
for the approved stage structure and the
[main implementation specification](../implementation.md#phased-delivery-and-coverage)
for the coverage and original conformance gates.
Use the linked [capability specification](../implementation-capabilities.md)
for capability IDs and prerequisites. It currently has zero approved entries;
the [25-capability draft](top-level-dag-contract.md) cannot supply approved entries
by default. Concrete phase criteria still require review before generation can
yield an actionable approved plan.

## Possible Generated View

For each phase, show approved capability IDs with their approved milestone,
required predecessor/interface conditions, dependency order, and applicable
behavior, persistence/recovery, coverage, real-probe and conformance gates.
Distinguish prerequisites that are satisfied, unsatisfied or not yet defined.
Missing criteria cannot be inferred as complete from source code or test counts.

Keep progress, evidence-run references and worker assignments in a separate
overlay keyed to stable capability IDs and the relevant approved phase. Changes
to that overlay do not change capability definitions, prerequisite edges or
acceptance criteria. A generated view can display readiness against the overlay;
it does not grant approval to start work or certify conformance.

The minimal first durable milestone remains different from completing the
staged implementation across every applicable component. The earlier
[durable-flow proposal](simulation-coverage-contract.md#earliest-durable-milestone-one-persistent-event-to-state-flow)
is still a proposal and is not silently adopted as a milestone here.

## Decisions Needed Before Implementation

- Which concrete milestone and acceptance criteria apply to each capability in
  each phase. No per-capability criteria are approved by the layout decision.
- Whether each dependency needs a specified interface/invariant milestone or
  completion of a predecessor's phase milestone; this is not inferred from an
  unreviewed edge or from a whole-capability completion flag.
- The actual milestone declarations and required interface conditions using the
  separately approved minimal schema; tooling does not infer these decisions.

## Current Tooling Limits

The separately authorized tooling now resolves the main specification's typed
links to the authoritative capability and phase documents, pins their hashes,
and generates phase views from explicit declarations. It does not import this
proposal's M01-M18 prose. Duvet supplies native requirement/citation links and
coverage; it does not infer milestone decisions. With zero approved capabilities
and milestones, the authoritative execution view is approval-blocked. Substantive
criteria, interface dependencies and real evidence remain necessary before any
milestone can be ready; generation alone grants no runtime authorization.

## Candidate Contract-First Gate

The high-level contract-first position is now approved in the seven-stage
structure. The detailed gate criteria below remain UNAPPROVED. The earlier
two-part renumbering alternative is superseded by the authoritative seven stages;
this section does not redefine their boundaries.

Recommendation: review the contracts needed for the first durable vertical
slice, then integrate that slice promptly to test those contracts. Do not wait
for every internal interface to be frozen. Later slices can define their needed
contracts before implementation; unknowns remain explicit decisions rather than
speculative abstractions or engine selections.

The candidate gate requires more than function signatures. A reviewable and
executable contract would identify:

- Component boundaries and responsibilities, linked to original requirement
  identities and approved capability IDs when those exist.
- Inputs, outputs, errors, cancellation and retry/outcome behavior, with concrete
  examples of allowed and forbidden results.
- Atomic commit boundaries, ownership/epoch fences and the stated durability
  and recovery guarantees, including ambiguous commits.
- How time, I/O and randomness enter the component through controllable
  dependencies for Bach, without selecting a runtime or storage engine.
- Discriminating contract tests with explicit assertions and applicable
  simulator/real-dependency probe obligations from the original specification.

Acceptance would mean the slice's material contract decisions are directly
approved, its required behavior has executable test cases or a runnable contract
harness, and each unverified real-probe obligation is visibly outstanding.
A passing simulator substitute alone does not validate the real dependency.
Prompt durable integration then exercises actual persistence, restart recovery
and the cross-component commit boundaries; contract changes exposed by that
integration return to review where they alter approved decisions.

These detailed gate criteria remain proposals only. Stage numbering is defined
in the authoritative phased specification.
No new production architecture or runtime implementation is adopted or started
by these criteria. The schema and generator are authorized separately; the
existing original conformance and promotion gates remain.

## Finer Candidate Milestones And Readiness

UNAPPROVED decomposition. M01-M18 are local review labels, not approved phase
numbers or capability IDs. Phase references will use approved capability IDs
when those exist; the authoritative capability document remains empty. This is
an initial decomposition, not a complete mapping of the original requirements.
The approved seven-stage structure does not adopt this detailed decomposition.

Repeat the contract-first checkpoint for the NEXT slice only: approve its
material boundary, inputs/results/errors/cancellation, commit and recovery
invariants, and controllable time/I/O/entropy contract. Then implement and test
that slice when coding is authorized. Do not require every component's contract
or every previous row to finish first. Contract changes discovered by integration
return to review where they change an approved decision.

Readiness below means the named interface/invariant is available and its material
decisions are approved; it does not mean every unrelated acceptance criterion of
the providing component has finished. Each such dependency boundary still needs
review. Row ordering is explanatory, not a serial execution schedule.

| Candidate milestone | Needed interfaces/invariants and readiness | Observable acceptance for this bounded milestone | Original requirement anchor |
|---|---|---|---|
| M01 Real store commit and process restart | Store candidate, test persistence boundary, atomic transaction/outcome contract approved. No session model or model provider required. | Commit a bounded multi-key change, terminate the writer and relevant store process without graceful flush, reopen real storage in a new process, and read the acknowledged values. Inject a pre-apply failure and observe no partial change; distinguish a lost reply after apply. | [Self-durable: A Crash Loses No Committed State](../../../decisions/durability/self-durable.md#a-crash-loses-no-committed-state): "A crash MUST lose no committed state." |
| M02 Sealed, versioned record recovery | Approved format/envelope/key-lifecycle contract; codec and key tests can proceed beside M01, with its adapter needed for persistent integration. | Write a sealed test resource, discard process memory, recover/decrypt it under the permitted binding; wrong tenant/owner binding fails. Preserve an unknown field through read/rewrite. No fixture plaintext codec becomes a durable format. | [Stored Form: Scheduling Fields Are Readable Without Keys](../../../contracts/stored-form.md#scheduling-fields-are-readable-without-keys): "A tenant's content MUST be sealed by the writing client before it is written, so that the durable store never holds it in plaintext." Also [Every Durable Format Is Self-Describing](../../../contracts/stored-form.md#every-durable-format-is-self-describing): "A decoder MUST preserve a field whose tag it does not recognize through a read and a rewrite." |
| M03 First durable event-to-session flow | M01/M02, owner fence, inbox/doorbell and minimal notification-decision/authority interfaces from the slice contract. Do not bypass the required hook to shorten the flow. | Publish, evaluate notification allow/block, atomically commit state/cursor and decision events, restart and recover the same result. Retry a duplicate/lost-reply publish without a second application; a stale owner cannot commit. This is the first durable vertical flow, not completion of every component. | [Sessions: Each Event Applies Exactly Once, In Order](../../../core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order): "A partition MUST commit a session's new state and its cursor in one transaction." [Failure Handling: An Owner That Crashes Or Moves](../../../core/failure-handling.md#an-owner-that-crashes-or-moves): "The new owner of a partition MUST resume each session from its committed cursors." |
| M04 Store-wide loss and backup recovery | M01 plus an explicitly approved all-node-loss test boundary and storage backup/restore interface. Can proceed beside session/model work. | Lose every node inside the declared boundary; restore committed records and verify no acknowledged state loss. Restore the store's backup separately and compare contents. Record exactly which storage media/resources survived; restarting a process on retained disk is insufficient evidence here. | [Self-durable: The Store Survives The Loss Of Every Node](../../../decisions/durability/self-durable.md#the-store-survives-the-loss-of-every-node): "The durable store MUST survive the loss of every node at once without losing committed state." |
| M05 One authority decision and revocation path | Principal/grant and policy-decision contracts approved; use current authority records. Integrate persistent reads with M01/M02; supply the narrow decision interface needed by M03 without waiting for every authority feature. | Admit one permitted action; delete its grant; the next decision denies before the action's effect. Reject the same action under the other tenant. Hook-specific transaction/event integration is checked as each consuming slice lands. | [Identity: A Grant Is Data](../../../core/identity-delegation-and-secrets.md#a-grant-is-data): "Deleting a grant MUST revoke it at the next decision." |
| M06 Durable transcript append and reconstruction | M03's transition/commit interface plus a reviewed bounded transcript record format. Independent of full compaction. | Append successive synthetic turn records, restart, and reconstruct the ordered history without rewriting earlier bytes or editing old entries. A failed append leaves the prior log readable. | [Direct Context: The Log Is Never Edited](../../../decisions/execution/direct-context-compaction-and-cache.md#the-log-is-never-edited): "The durable log MUST NOT be edited in place." |
| M07 Usage identity and delivery | Approved work-ID, finalization/accounting and sink interfaces; M01/M02 persistence. Ledger tests can precede actual producers; producer acceptance is repeated with M08/M09 and deciders. | Retry one usage identity through a lost commit reply and restart; observe one final stream record. With the first actual model attempt, verify the producer writes one record, including interruption handling. Ledger-only success is not complete usage integration. | [Usage Record: Every Attempt Is Recorded Once](../../../contracts/usage-record.md#every-attempt-is-recorded-once): "Exactly one usage record MUST exist for every model attempt." |
| M08 One direct model turn | M03, M05, M06 and M07 interfaces; approved scheduler/admission, model hooks, provider/capacity and credential boundaries. A simulator-only provider result remains labeled as such. | Process one inbox event into a scheduled model request, gate its result and commit transcript/state/usage; interrupt and restart it as a new attempt without losing the prior record. Run the applicable real-provider adapter probe where authorized. | [Direct Model Turns: No Call Is Made Directly To A Model Endpoint](../../../decisions/execution/direct-model-turns-and-scheduling.md#no-call-is-made-directly-to-a-model-endpoint): "Every model call MUST pass through a scheduler on the partition owner's node." [Durability: Work Resumes From Committed State](../../../core/durability-and-deployment.md#work-resumes-from-committed-state): "An interrupted model attempt MUST be run again as a new attempt with an attempt id of its own." |
| M09 One tool dispatch and ambiguous recovery | M08's model-output path; M05 authority; M07 usage; approved independent idempotency/side-effect semantics, receipts, hooks, credential leasing and egress. | Observe committed dispatch evidence before sending. Lose the reply after a receiver effect, restart and recover according to that tool's semantics without an unjustified duplicate side effect; gated result and usage survive. | [Tools: The Dispatch Sequence](../../../core/tools-and-dispatch.md#the-dispatch-sequence): "A dispatch record MUST be committed before the call is sent." [Recovery Follows The Tool's Semantics](../../../core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics): "A tool with a reconcile binding MUST be asked on recovery whether its effect took effect." |
| M10 One durable timer setting/fire | M03 inbox interface, injected clock/deadline and guard contracts, and approved timer edit semantics. Can proceed beside M08/M09. | Set a timer, restart before due time, observe no early fire, then exactly one applied fire. Cancel or replace before application and reject the stale occurrence. Expand calendar/recurrence cases in subsequent small slices. | [Timers: A Fire Is Exactly Once](../../../core/timers.md#a-fire-is-exactly-once): "A fire whose timer was cancelled, re-armed, or replaced before the session applies it MUST be dropped." |
| M11 One bounded topic fan-out | M03 recipient commits, M05 publish/subscribe decisions and approved membership/resume semantics. No model or full timer milestone required. | Publish to subscribers spanning more than one bounded batch; restart mid-fan-out and observe ordered, deduplicated delivery. A slow subscriber does not hold another's delivery. | [Events: Publish Latency Is Independent Of Subscribers](../../../core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers): "Fan-out MUST deliver to subscribers' inboxes and doorbells in batches within the store's configured transaction key bound." |
| M12 Owned blob and pin lifetime | M01/M02 plus approved blob adapter, owner generation, pin and key-destruction contracts. Can precede model/topic integration. | Persist an immutable blob and pin, restart and read through the correct holder; a foreign tenant or lapsed pin fails closed. Race reference publication with reclamation and retain referenced bytes. Logical deletion alone does not prove key destruction. | [Encryption And Blobs: Sharing Is An Explicit Pin](../../../core/encryption-and-blobs.md#sharing-is-an-explicit-pin): "A pin MUST NOT admit a holder of another tenant." |
| M13 One transcript freeze/compaction swap | M06, M12, a compaction-hook contract and approved range-publication/swap boundaries. Does not block the first bounded model turn. | Freeze a range while appends continue; restart around publication/swap and recover the old or new valid view with no lost tail. Preserve archived history; cancelling does not mutate durable state. | [Direct Context: The Swap Preserves History](../../../decisions/execution/direct-context-compaction-and-cache.md#the-swap-preserves-history): "The switch to a compacted transcript MUST take effect in one commit, however many commits wrote the compacted transcript." |
| M14 One live configuration/control transition | M03/M05 plus approved pending-definition and adoption-boundary contract. Test against the actual M08/M09 boundaries as they become available. | Queue a definition change; restart and adopt at the next turn boundary, never between a tool call and its result. Exercise a specified blocked/quarantined exit through its allowed control path. | [Sessions: A Change Applies At The Next Turn Boundary](../../../core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary): "A configuration change MUST NOT apply between a tool call and the delivery of its result." |
| M15 One authenticated frontend route | M03/M05, verified caller/chain, routing, admission/cap and receipt interfaces; transport choice remains pending. Can integrate before full model/tool coverage. | Submit one state-changing request through the frontend, including a non-owner route, and observe one committed effect/reply. Reject an unauthenticated/cross-tenant request and refuse an exhausted cap before any effect. | [Frontend: A Principal Makes Every Call](../../../core/the-frontend.md#a-principal-makes-every-call): "Every call to the frontend MUST be made by an authenticated principal." [Caps Are Configuration](../../../core/the-frontend.md#caps-are-configuration): "A cap check MUST be atomic with the operation it bounds, so that concurrent requests cannot exceed the cap together." |
| M16 One observation-to-authorized-read flow | M05/M06/M12 and observation queue/pin/access interfaces; can proceed beside tool or timer work. | Publish an observation pointer, restart the consumer, and read only its granted range with an audit record; revoked access or unreadable source is refused, not replaced with copied content. | [Observation: Reading An Observed Range Is Granted And Audited](../../../core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited): "Every read of an observed range MUST be audited." |
| M17 Cross-component drain/restore increment | M04 substrate recovery plus the currently integrated state paths' persistence, epoch, identity, authority/emergency and sink interfaces. Repeat as each new state path joins; do not wait to test until every component exists. | Drain a named integrated flow, flush its pending audit/usage, replace/restore and reopen only after its required authority/emergency records. Preserve IDs and dedupe; reject old-epoch writes. List state paths not yet included. | [Durability: A Restore Resumes Every Session](../../../core/durability-and-deployment.md#a-restore-resumes-every-session): "A restore MUST apply a tenant's authority records before any of the tenant's sessions resumes." |
| M18 Coverage/conformance closure for an approved scope | Approved capability/coverage scope, all applicable required behavior and evidence for that scope, plus approved exceptions where any exist. Close ready scopes independently; full-platform closure still requires all applicable scopes. | Show measured 100% executable lines AND 100% instrumented branches under Bach, complete applicable Duvet links and behavioral/probe evidence, and the original trusted gate result. No unsupported metric or local report is promoted to proof. | [Simulation: The Gate Decides Promotion](../../../core/simulation-and-conformance.md#the-gate-decides-promotion): "A build in which any absolute requirement lacks both an implementation and a test citation MUST NOT be promoted." The additional line/branch targets are already approved in the implementation specification. |

## Evidence At Every Milestone

Every candidate row includes relevant behavior/negative tests, chosen-failure
and recovery tests, tenant-isolation checks, and applicable real-adapter contract
probes as part of that row's acceptance. Probe unavailability is recorded as an
unmet obligation, not deferred invisibly to M18. The original requirement is:

> Each simulator MUST have a contract test that runs the same expectations against the simulator and, through a probe, against the real dependency.

Source: [Simulation And Conformance, Every Dependency Has A Verified Simulator](../../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator).

Each milestone's evidence packet records exact source/config/tool digests,
commands, test assertions/results, failure/replay references and limitations.
Report actual Duvet implementation/test and planning associations separately,
with observed Bach line/branch counts for the declared scope. When measurement
or a real probe is unavailable, report unavailable and its blocker, not zero,
100%, or an invented intermediate target. Required safety/correctness checks
remain requirements before a milestone claims the behavior they establish.

Sampling effort, line/branch coverage, semantic fault/schedule coverage and
trusted conformance remain distinct. M18 closes outstanding obligations; it is
not the first time testing, coverage measurement, probes or Duvet are performed.
The table's source quotations are anchors, not an exhaustive requirement map or
permission to omit other applicable original requirements.

## Parallel Readiness And Remaining Decisions

After their slice contracts are approved, M01 adapter qualification and M02
codec/key tests can proceed independently. M04 store-wide recovery need not wait
for models; M05 authority checks and M12 blob work can advance once their own
dependencies are ready. After M03, timer, topic, transcript and frontend slices
can progress independently where shared interfaces are settled. Model and tool
integration sequence only where the actual model-output/dispatch contract
requires it. Shared files have one owner; an interface change is coordinated
before dependent work starts. No worker assignment or coding restart is made.

Still unresolved: the real store/adapter and survival boundary; persistent test
resources; tagged formats and key/sealing lifecycle; production runtime/I/O,
provider/credential/egress and transport choices; topic membership, timer edit,
tool recovery/accounting and configuration adoption decisions identified in the
[existing audit](implementation-assumption-audit.md). No choice is selected by a
milestone label. A blocked decision holds its affected slice, not unrelated
ready branches. Acceptance of these proposed dependency boundaries and criteria
also remains pending; this decomposition adopts no additional tooling contract.

M01 is actual process-persistent storage, not an in-memory simulation. M03 is a
small durable vertical milestone, not completion of the implementation across all components.
M04/M17 distinguish tested all-node and integrated restore guarantees from a
single retained-disk restart. The authoritative specification set defines the
approved layout and seven-stage structure; this detailed milestone decomposition
changes neither.
