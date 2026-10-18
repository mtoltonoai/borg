# V2 Execution Plan Review Draft

This is the single authored initial execution outline for v2, based on the selected profile in the decision record. It is not an executable queue or a copied v1 capability inventory. Its purpose is to get a small real system running early and then extend that same architecture. Dependencies describe observable producers; a review document alone does not provide an implementation. The decision record identifies execution authorization; actual assignments and readiness are separate from this authored plan.

Initial model-budget work follows [the selected deferral](implementation.md#initial-model-budget-policy): new defaults use the maximum supported allowance. Existing native request admission, finite counters, configured child allocations and lower-limit negative tests remain. No first-release gate demands enforcement of hidden supplied-executor requests, tokens or spending. Actual provider limits and allowed test capacity remain distinct.

## First Acceptance Journey

An authenticated operator creates a coding task; durable task state records it; an owned worker starts the supplied executor in a repository checkout; the executor edits real source, compiles it and runs relevant tests; a change is submitted through the configured repository interface; the product shows status and result references. Restarting the control process preserves the task and its accepted result. Losing a worker recovers only from the last acknowledged external checkpoint. No production rollout is part of this demonstration.

The accepted integration boundary is the task. Its recovery cases distinguish: application restart with intact task storage; executor failure with usable saved conversation history; executor failure without that history; and worker loss after an externally acknowledged checkpoint. A new attempt retains the task identity. A resume proves the history's task association and identifies the actual recovered checkpoint, rather than assuming missing edits survived. Before the first new checkpoint, recovery uses the recorded immutable starting revision. A published checkpoint whose task reference has not committed is not silently selected. Lost acknowledgments for external effects are reconciled before retry. Tests distinguish an executor's successful exit from passing the task's actual acceptance checks. They do not require a fork of the supplied executor or a transactional copy of every internal model/tool action.

The selected security path uses ordinary host accounts and local access controls, with SSH forwarding for private cross-host access where needed. E1 has no key service, credential broker, custom request-signing system or certificate-management dependency. Acceptance checks cover authenticated access at that boundary, no unintended public listener, secrets excluded from task output, stale task ownership refusal, and protected final-rollout authority. Cross-host cases also cover a lost tunnel without false task completion or blind repetition of an uncertain effect. These checks do not claim hostile-worker containment or the old shared credential-service guarantees.

E1 uses one local SQLite controller and a worker-side Codex app-server adapter over stdio. Git source checkpoints are acknowledged outside the worker before their references are committed to task state. Restart also verifies task history, accepted outputs and explicit agent memory. A source checkpoint does not restore a destroyed task database. Automatic failover, a separate artifact service and a programmable policy engine are not prerequisites.

## Delivery Increments

| ID | Deliverable and measurable acceptance | Actual dependency | Work that can proceed independently |
|---|---|---|---|
| V2_E1 | One vertical coding task reaches the real executor, edits/builds/tests/submits, and persists a truthful result; application restart preserves it. | Qualified exact SQLite/client and Codex-stdio profiles; configured host access, repository and provider credentials; existing toolchain/test capacity. | Task-store operations, executor adapter and worker command path can be authored together against the same small compiled application contract. |
| V2_E2 | One bounded native request produces validated structured output; invalid output, timeout and retry limits produce explicit failure. | Common task input/result boundary from E1. | Does not need extended workspace lifecycle, a second general-purpose harness, backups or the full collaboration UI. |
| V2_E3 | A native task delegates coding work; one child result returns to the correct parent without provisioning access or multiplying effects on duplicate delivery. | Actual E1 supplied-executor path and E2 native path. | UI status/results and externally mediated action reconciliation. |
| V2_E4 | The browser collaboration product submits, assigns, tracks dependencies, observes progress/output, cancels, retries and closes real tasks; stop/reassignment races retain truthful outcomes and preserve independent emergency controls. | Durable task/result and executor boundaries exercised by E1; native delegation only for cases that use it. | Product task views form one complete batch; document/question/review workflows are the queued increments specified below. |
| V2_E5 | Two workers run independent tasks; worker loss, lost acknowledgment, stale owner and restart settle without false success or blindly repeating uncertain external effects. | Actual E1 task/executor/checkpoint producers. | Native and collaboration work need not finish before these failure cases begin. |
| V2_E6 | Qualify the initial four-host target: 768 outstanding real native model requests alongside 24 coding workspaces, with build/test capability in every coding workspace; report actual resource/provider limits and failures. | E1/E5 real concurrency/recovery; exact workload and resource envelopes. | Ramp native requests through 32/64/128/256 per worker under coding/build load; 512 is stress-only; locally queued or idle work does not satisfy capacity; the measure is worker client capacity, not provider-internal processing. |
| V2_E7 | Prepare a reproducible release; exercise protected exact-revision checking/automatic merge, artifact-bound final rollout authorization and upgrade compatibility; review remaining conformance and uncovered critical paths. | The included product behavior, E5 and the scale evidence required for the release claim. | Required behavior/recovery tests, focused simulation and coverage measurements accumulate from E1 onward; no blanket full-line/full-branch gate. |
| V2_LATER_SCALE | Later ten-host capacity qualification under its separately specified workload. | Initial four-host evidence plus defined active-agent mix/hardware/resource envelope. | Not a first-release prerequisite; no implication of capacity surviving host loss. |

The table describes the same seven initial delivery views and later ten-host view as the structured declarations below. It does not adopt the v1 graph. Backup/restore remains deferred without an active delivery milestone; adding it later requires its actual recovery objective and surviving medium.

The [four-host target](production-target.md) is the selected initial production qualification level. The ten-host objective remains a later scale qualification on the same architecture, with its exact active-agent workload still to be specified. Four-host resource, provider and responsiveness measurements precede any capacity claim; ancillary latency/soak budgets remain proposals. Merge tests cover failed, missing, stale and wrong-revision protected results, plus proof that merging does not trigger an unauthorized rollout.

## Completion Of A Batch

Each batch includes implementation, ordinary build/test/debugging and a real integration consumer. The owner corrects recoverable failures before handback. The integrator integrates completed batches promptly. Reviews use runnable results and changed interfaces; they do not require a coordinator release for every reversible local edit. Source identity, process cleanup and real effect/currentness checks remain required, but repeated inventory walks and per-case bespoke runner implementations are not acceptance goals.

Maintain one reference end-to-end command or test entry point. Use existing build and test tools before adding another supervisor or report format. A shared interface changes only through the integrator so independent owners do not edit the same source. Contract tests run early enough to catch actual lifetime, cancellation, restart and result-shape mismatches.

## Progress Reporting

Report the last passing integrated journey, the next missing actual producer, elapsed time to a useful result, and concrete failures. Source packets, agent counts, citation completeness and isolated test counts are supporting information. They are not a percentage of a working system. Forecasts follow measured integrated delivery; the withdrawn v1 date is not a v2 commitment.

## Generated Plan And Scope

The capability and milestone declarations below are the single plan consumed by retained Duvet Cap. The build packet binds the shared-scope reconciliation, exact proposed dependency profile and protocol. Native planning associates owned requirements with acceptance gates; it does not execute those gates or manufacture ownership/readiness. The actual shared specification selection and historical implementation snapshot remain unchanged.

## Parallel Delivery Batches

After the shared protocol fixture compiles, controller, coding worker, native executor and product client are four disjoint implementation batches. They use one Rust package and ordinary build/test tooling. The integrator alone edits Cargo.lock/Cargo.toml, protocol types, module exports, application entry points and the whole-plan declarations.

| Batch | Proposed source boundary | Delivery and qualification |
|---|---|---|
| Controller | src/controller/ and tests/controller/ | SQLite state machine, private API and bounded scheduling; real database restart/duplicate/conflict cases. |
| Coding worker | src/worker/ and tests/worker/ | Codex stdio, process cleanup, workspace build/test and Git reconciliation as one batch. |
| Native executor | src/native/ and tests/native/ | Bounded provider requests, schema validation, deadlines and controlled/real boundary tests. |
| Browser collaboration client | src/product/ and tests/product/ | Browser task list/details, task operations, progress/output and code/results using the same controller API; no second store or frontend framework selected. |
| Integrator | Shared manifests/protocol/entry points and tests/e2e/ | First real journey as soon as controller+worker work; recovery begins without waiting for native or full product work. |

These are complete component-sized work packages, not per-file author/reviewer/runner assignments. There are four implementation owners and one integrator. Their proposed file boundaries are exclusive; no actual agent or source file is allocated until execution starts. Each owner owns its tests and ordinary build/debug loop. Shared API changes go through the integrator as a single patch, with affected owners notified together; routine reversible changes do not require handoffs between separate authors, reviewers and runners.

### Common Handoff And Build Rules

The integrator freezes the compiled protocol fixture, one Rust package/lock/profile, command conventions and shared test helpers. Owners work from that exact base in isolated Git branches/checkouts, including across machines. Each batch returns its pushed exact commit, changed-file list, ordinary build/test commands and results, a consumer integration example, and only remaining concrete failures. Full transcript archives, repeated source inventories and a custom runner per case are not deliverables. One source-bound build/test receipt per completed batch is sufficient; changes to that source require affected checks, not automatic repetition of unrelated checks.

Use ordinary Cargo test targets. Each owner has its named top-level integration-test entry file plus supporting files below its test directory, preserving test discovery. Proposed common commands are `cargo check --locked --all-targets`, `cargo test --locked --test <batch>` and the corresponding actual integration tests. The integrator owns common manifest/lint settings, `src/lib.rs`, `src/main.rs`, `src/protocol.rs`, `src/client.rs`, and `tests/e2e.rs`. A copied fixture qualifies only the component seam. A real controller/worker/provider/browser join is required for the relevant integrated acceptance.

No agent needs another agent's mutable worktree. WIP commits can be shared through the Git checkpoint process before merge; that does not mark a dependency complete. Owners have no standing peer-message requirement. They receive the frozen contract and whole batch, work through their own code/build/test corrections, and report only contract conflicts, real blockers or ready integration commits to the integrator. The integrator keeps a runnable branch and exercises the first journey as soon as controller and coding worker are usable.

### Controller Batch

Own `src/controller/`, `tests/controller.rs` and `tests/controller/`. Deliver one local database owner, bounded request queue, private HTTP adapter, task/attempt/command/operation/result/history/memory records, queued assignment, immutable definitions, oldest-eligible scheduling, concurrent sessions per agent with one current execution per session, cancellation/retry and restart reconciliation. Implement the frozen protocol rather than introduce another framework or separate store service.

Close V2_STORE gates STATE, ACCESS and ADMISSION against real SQLite. Include both cancellation/completion orders, duplicate/conflicting operations, stale revisions/generations, lost acknowledgments, intact-storage restart, unsupported/corrupt schema preservation and queued/active assignment refusal. Demonstrate the worker/product clients compiling against the actual handler. This owner also implements the controller half of child waits when the native consumer integrates; it does not hand off an untested state machine for the integrator to repair.

First useful handoff is a real task submit/read plus durable assignment and accepted-result path that survives controller restart. It need not wait for the native adapter, browser polish, later-scale qualification or every failure scenario to allow integration; its remaining gates stay visibly incomplete until exercised.

### Coding Worker Batch

Own `src/worker/`, `tests/worker.rs` and `tests/worker/`. Deliver actual Codex-stdio lifecycle, bounded progress/results, process-tree ownership and stop evidence, repository checkout, direct build/test execution, WIP checkpoint publication/fetch and lost-response reconciliation. The source entry point is the pinned executor's qualified schema; no fork of its reasoning loop, second transcript store, snapshot service, permission engine or build-permit service is included.

Close V2_WORKER's EXECUTOR, WORKSPACE and CHECKPOINT cases with the real pinned process and test repository, then join the real controller for V2_E2E's integrated gates. Exercise duplicate launch, stale incarnation, lost start/interrupt response, unavailable history, unpushed work loss, exact remote-ref recovery and a stale attempt unable to select a checkpoint. Retain failed acceptance separately from a successful executor exit. First useful handoff is one real edit/build/test/checkpoint/result round trip using the controller client.

### Native Executor Batch

Own `src/native/`, `tests/native.rs` and `tests/native/`. Deliver bounded provider requests, structured-output schema validation, cancellation/deadlines, maximum-default request allowances, retained native debits/child reservations and observed usage. Opaque supplied-executor inner-request, token and cost enforcement is deferred. Use the same task/result handler and currently selected model; no shell or second coding harness. Controlled provider tests are not presented as real provider evidence.

Close V2_NATIVE and V2_NATIVE_JOIN, then V2_DELEGATE with the actual coding worker. Cover invalid schema/output, oversized input/output, timeout, provider refusal/throttle, caller cancellation and restart without false accepted results. For delegation, verify one child per operation, durable parent association, quiescent slot release, original request debits/child reservations and deadlines, and one continuation after duplicate child completion. Reject transitive prerequisite/parent-wait cycles and over-depth delegation; the simpler task model does not remove those correctness bounds. Contract fixtures allow early implementation; real provider access and actual joined producers gate only their corresponding evidence.

### Browser Collaboration Batch

Own `src/product/`, `tests/product.rs` and `tests/product/`. Deliver task list/detail/create forms, queued assignment, dependencies, status/progress/available output, cancel/retry and checkpoint/result navigation using the frozen controller operations. The minimal proposed implementation is Rust-rendered HTML and normal forms/links; no JavaScript runtime, frontend framework or separate database is required. First acceptance is a usable browser workflow, not a CLI or static mockup.

Close V2_PRODUCT with real-browser actions against the actual controller and coding worker, including the full browser-created task and restart journey. Cover escaping task/output text, repeated form submission identity, stale page/revision, failed dependency, pending cancellation, uncertain external operation, output truncation, reconnect/refresh and invalid source/result links. Ordinary same-origin/non-GET mutation handling supplements the existing private authenticated access; it does not add roles or per-task grants. Product implementation can proceed from the common fixture while controller/worker integration is prepared.

### Integrator And First Delivery

Own only the shared files and integrated tests listed above. Qualify and freeze the package profile once, compile all four consumers, integrate ready commits promptly and maintain the normal executable entry points. The first smoke test is controller plus worker: submit, real code edit/build/test, acknowledged source checkpoint, accepted result and controller restart. The first user-facing acceptance adds the browser and performs that same journey from task creation through displayed durable result. Native execution is not a dependency of either first coding demonstration.

Follow with worker loss/stale-owner/unknown-effect tests as V2_RECOVERY, then the actual four-host mixed workload and protected release checks. The graph's later ten-host objective is not a hidden first-release barrier. Reviews inspect integrated behavior and substantive changed boundaries; independent reviewers can review whole runnable batches without creating a second writer or per-case execution owner. Report the first failed or passing real journey and its source/run evidence; component counts and source packets are not completion percentages.

## Collaboration MCP And Native Tool Loop

The same collaboration MCP catalogue serves native and Codex tasks through the canonical controller operation layer. The [integration plan](proposals/collaboration-mcp.md) records the source seams, profile and transport choices, current trusted-space limitation and test cases. Native Borg acts as MCP client; provider-hosted web search remains separately profile-controlled. Neither executor failure nor tool failure changes executor selection.

The controller batch delivers and tests bounded projections and caller-bound transactional mutations as COLLAB_MCP. The native batch and Codex adapter can develop independently against that typed fixture. Actual joins use the real controller: V2_NATIVE_JOIN closes the variable-step native path and V2_E2E closes Codex discovery/calls/recovery. An MCP transport fixture is not actual collaboration qualification. No new store or separate agent-facing operation implementation is introduced.

## Queued Collaboration Workflow Batches

All four functional batches in proposals/collaboration-workflows.md are queued. The integrator owns the compiled common protocol, retained-state database/wire disposition, typed activity and canonical receipt contract, router/catalogue and metadata-bound checks. These declarations add no new source ownership or ready evidence; the operational queue retains the named existing owners.

The common protocol release allows all four owners to author their disjoint components in parallel against typed resolver interfaces. Release of an interface means its named gate subset and actual producer were exercised, not that a document exists. All four component releases can proceed in parallel behind that common producer, using real own-domain controller operations and typed peer fixtures. V2_COLLAB_JOIN is the primary complete-queue, actual stopped-slot, atomic resolver/wait/delivery and executor-consumption closure. It consumes all four actual producers plus coding/native/browser/delegation/recovery; fixtures do not satisfy that closure. Stage order is presentation, not a barrier.

| Work batch | Declaration | Required actual closure |
|---|---|---|
| Task coordination | V2_COLLAB_WORK | Common records and typed resolver/stop seams; actual question/review atomicity and stopped-slot evidence close at JOIN. |
| Questions and decision queue | V2_COLLAB_QUESTIONS | Actual question operations and typed review query seam; complete real question/review queue closes at JOIN. |
| Documents and review | V2_COLLAB_DOCUMENTS | Common records, exact immutable subject and current direct-reviewer binding. |
| Resolution delivery and consumers | V2_COLLAB_DELIVERY | Real durable delivery component and compiled consumer adapters; actual native/Codex input, acknowledgment and continuation close at JOIN. |

V2_COLLAB_CONTRACT establishes the shared producer and compatibility prerequisites. V2_COLLAB_JOIN checks the complete journey, both browser/MCP surfaces and recovery, and feeds the existing release milestone. Existing source, history, shared-memory access, immutable-prerequisite, accepted-profile, deadline/accounting, cancellation and uncertain-effect rules remain. General subscriptions, team routing, authenticated-person identity, encryption and broader shared conformance are not supplied by these batches.

## Charter Adoption Batches

The charter increment uses the existing controller store, definition identity, native loop, supplied-executor adapter and product reads. V2_CHARTER_RECORDS freezes one typed source/turn/reply producer and compatibility disposition. Native and Codex consumers then proceed independently against it, while V2_CHARTER_JOIN supplies delivery, audit and recovery evidence at their respective qualified boundaries. Native retains the full model-turn contract; Codex follows the task/public outer-turn exception and keeps unsupported updates visible. Existing stage, capability and graph identities remain; the nine retired owned requirement tuples retain their historical evidence, and the existing release review also consumes CHARTER_JOURNEY. No completed gate or source coverage is inferred from this declaration. The engineering feasibility packet records current gaps separately from the selected scope.

## Focused Governance Incorporation

The four approved additions reuse the current five capabilities and existing milestone graph. V2_STORE adds STOP_ADMISSION, DECISION_RULES and EMERGENCY_CONTROLS and extends CONFIGURATION_APPLY. These are new required gates with no supplied implementation or test evidence; earlier narrow checks retain their exact historical scope. Shared record/interface fixtures may be authored independently, while actual native, supplied-executor, browser and recovery behavior closes in the existing integration gates. No V2_STORE dependency on its downstream collaboration join is introduced.

The configuration payload remains the existing closed limit update. Rule selection, shadow promotion and temporary override use the existing protected host-control boundary and require new versioned decision/audit behavior. They do not add a per-task permission engine or fabricate authenticated-person identity. Existing exact revision/receipt/unknown-outcome semantics and final rollout approval remain.

The finite acceptance matrix is proposals/governance-foundation.md. Root composes the concurrent charter additions, regenerates native requirement/planning bindings once, and reports actual runtime gates separately. Old source snapshots and passing implementation results remain historical.

## Structured Delivery Declarations

The following declarations are consumed by existing Duvet Cap. The later ten-host milestone is separate from first-release completion. Stage order is presentation; explicit interfaces control readiness. First-release declaration adoption is recorded in the bound review metadata. Authored ownership remains empty; actual assignments and evidence belong to the execution ledger. The later ten-host view remains future scope. No progress/evidence is inferred from authoring.

### First End-To-End Journey

Delivery view V2_S1; acceptance is the milestone gate set below.

### Bounded Native Work

Delivery view V2_S2; acceptance is the milestone gate set below.

### Task Delegation

Delivery view V2_S3; acceptance is the milestone gate set below.

### Task Collaboration

Delivery view V2_S4; acceptance is the milestone gate set below.

### Integrated Recovery

Delivery view V2_S5; acceptance is the milestone gate set below.

### Four-Host Qualification

Delivery view V2_S6; acceptance is the milestone gate set below.

### Release Review

Delivery view V2_S7; acceptance is the milestone gate set below.

### Later Ten-Host Objective

Delivery view V2_S8; acceptance is the milestone gate set below.

```duvet-cap-stages
[[stages]]
id = "V2_S1"
order = 1
heading = "First End-To-End Journey"

[[stages]]
id = "V2_S2"
order = 2
heading = "Bounded Native Work"

[[stages]]
id = "V2_S3"
order = 3
heading = "Task Delegation"

[[stages]]
id = "V2_S4"
order = 4
heading = "Task Collaboration"

[[stages]]
id = "V2_S5"
order = 5
heading = "Integrated Recovery"

[[stages]]
id = "V2_S6"
order = 6
heading = "Four-Host Qualification"

[[stages]]
id = "V2_S7"
order = 7
heading = "Release Review"

[[stages]]
id = "V2_S8"
order = 8
heading = "Later Ten-Host Objective"

```

```duvet-cap-milestones
[[milestones]]
id = "V2_BOOT"
capability = "V2_CONTROL"
stage = "V2_S1"
title = "Common application contract"
acceptance = "One ordinary Rust package and shared typed request/response fixture compile; candidate dependency profile and exact schemas are resolved before implementation consumption."
scope = "interface"
provides = ["TASK_PROTOCOL"]
requires = []
owns = []
[[milestones.gates]]
id = "CONTRACT"
expectation = "Compile the actual client/controller/worker protocol consumers; reject invalid enum combinations and unknown operation kinds. No runtime guarantee follows from this interface milestone."
[[milestones.gates]]
id = "PROFILE"
expectation = "Verify exact selected dependency sources/features and runtime identities; unresolved version or feature changes remain explicit."

[[milestones]]
id = "V2_STORE"
capability = "V2_CONTROL"
stage = "V2_S1"
title = "Local controller and durable operations"
acceptance = "Deliver the real SQLite controller, bounded API/database queues and transactional task transitions in one code/build/test batch."
scope = "integration"
provides = ["TASK_STORE", "TASK_API", "COLLAB_MCP"]
requires = ["V2_BOOT:TASK_PROTOCOL"]
owns = []
[[milestones.gates]]
id = "STATE"
expectation = "Real SQLite tests cover durable input/history/memory, one current attempt, duplicate outcome replay, payload conflicts, restart, cancellation intent and stale writes. Stored schema is identified; unsupported/corrupt bytes remain unchanged; the selected development transition is stopped-process reset with a fresh schema, while unsupported or corrupt input is never reset implicitly. Model and reasoning effort are part of immutable template equality. Changed settings under one template revision refuse; new revisions do not change accepted attempts. The fresh development schema requires explicit settings; no prototype migration or alias-memory merge is required. Persist distinct agent, session, task and attempt associations. Two concurrent sessions of one agent see the same memory; competing expected-revision writes produce one commit and one stale-revision refusal without lost updates. Other agents retain separate memory. Retry/restart preserves session identity and its pinned definition revision. Distinct revisions never alter an existing session silently. Repository registrations share the existing SQLite owner and canonical operation receipts. Register two stable IDs, retain immutable revision bodies, and compare the current head generation on revision and enable updates. Exercise exact retry, changed-body conflict, concurrent stale CAS, failed commit, lost reply and intact-store restart without partial revision/head writes. Accepted task projections retain the selected registration revision after later catalogue changes. For shared retry control, qualify durable model/endpoint breaker and dependency/credential-configuration authentication suppression across workers. Original permit/result identities, half-open probe generation, cooldown and late results remain exact. An unknown original request stays unresolved while independent eligible work can use a later probe. Retry exhaustion commits an explicit blocked state, reason event and supported control exit without resetting original task accounting or deadline. Transient store failures roll back then retry the original operation with configured backoff; a lost committed response resolves through its exact receipt."
[[milestones.gates]]
id = "ACCESS"
expectation = "Product requests authenticate through configured host/SSH access; worker host and external repository/provider restrictions remain. No per-task grant or role engine is required; a caller identity field alone does not authenticate. Local storage/deletion limits are explicit. Repository registration records already provisioned locations only. Demonstrate that register/revise/enable operations perform no repository acquisition or filesystem provisioning and change no trusted root, credentials or permissions. Missing material or unavailable operational read access remains unavailable; catalogue metadata is not an access grant."
[[milestones.gates]]
id = "ADMISSION"
expectation = "Dependencies are immutable and refer to existing tasks; failure stays blocked; queues are bounded, oldest-eligible admission uses committed order, sessions of one agent can execute concurrently while each session has at most one current execution, and quiescent delegated waits release slots without changing the accepted definition revision. Resolve repository metadata once at task or parent-workflow acceptance. External inputs select a stable ID and exact current revision without supplying resolved endpoints; reject missing, forged, stale or disabled selections. Enforce one primary association for coding or one explicit native read association, preserving existing executor-shape rules. Accepted work and its already resolved configured children retain their original bindings after head or enable changes."

[[milestones.gates]]
id = "COLLAB_MCP"
expectation = "The existing release in this gate covers the established task/agent/memory/repository catalogue. New question/document/review/wait/resolution producers and their full current-surface qualification are supplied by the collaboration component releases and V2_COLLAB_JOIN, not required to reopen the first journey. The MCP server reuses the canonical controller handler and existing durable records. Exercise real-store task/agent/history/memory reads, task revisions and configured child mutation rules, bounded worker capacity/attempt/checkpoint/release-stage queries, and consistent native/Codex catalogue semantics. Bind caller currentness and original mutation receipt in one transaction; lost acknowledgments replay one exact effect, changed arguments refuse, stale/expired callers cannot add an effect. Known credentials and protected grants/control payloads are excluded; operational views cannot dispatch control operations. Test malformed selectors, pagination, bounds, redaction, existing host/origin checks and the explicit absence of private-project containment. Repository list/read/literal-search/history/diff calls resolve an accepted exact source tuple and return that revision, with bounded results and no arbitrary local path, unconfigured remote, worktree mutation, hook, external diff or source-code execution. Exercise traversal/symlink, unavailable object, wrong repository/revision, large/binary content, cancellation and source races using real Git fixtures outside database transactions. Actual native/Codex consumers close at NATIVE_INTEGRATION and INTEGRATED_WORKER; this gate alone does not prove either model path. Add bounded catalogue list/get projections to the existing shared interface. Native and Codex see the same public registration identity, revision and endpoint data, with controller store keys excluded from successful results, operation projections and errors. Agent tools cannot register, revise or enable repositories. Exact repository reads remain pinned to the accepted immutable revision, with no current-head fallback; unsupported cross-store diffs refuse explicitly rather than silently changing source."

[[milestones.gates]]
id = "READ_PAGINATION"
expectation = "Qualify bounded first pages and explicit continuation for oversized permitted lists and individual task instructions/results, document content and repository ranges. Exercise escaped response-size fitting, useful progress for an oversized first list item, explicit completeness/exhaustion, original read replay and unchanged write outcomes. Bind content continuation to its exact object/version or repository source; refuse changed or unavailable content visibly while unrelated controller revisions do not invalidate unchanged content. Preserve list keyset semantics, caller/source authority, redaction and existing admission bounds. Actual native and supplied-executor consumers receive the same page semantics and request further pages explicitly; no adapter fetches and concatenates the whole result into model context. Controlled peers establish only their component seam."

[[milestones.gates]]
id = "METADATA_LIMITS"
expectation = "Load explicit versioned metadata with no numeric policy fallback in executable code. Resolve global defaults, agent revision overrides and explicit task limits once at admission; retain values and source bindings with the accepted task. Exact replay and recovery keep that binding after default changes. Exercise values above former hardcoded native context/schema/tool-call, Codex frame/journal, API, repository and controller caps through the actual producer and consumer; inconsistent cross-field values refuse visibly without clamps or effects. Missing, unknown, duplicate, overflowing or stale metadata does not become an implicit default. Lowered defaults cannot discard old pending journals or uncertain operations. Inspect effective values through shared agent queries and browser views. Distinguish fixed protocol/type ceilings, unknown provider limits and actual host-control enforcement; changing metadata does not grant credentials or install controls. Retain fencing, exact mutation replay, cancellation and session exclusivity. Per-component tests are prerequisites to the actual native, coding and restart integration gates; metadata completeness is not deployment proof. Verify the maximum-context seed against the selected model window and existing byte representation, including all four native carriers and the explicit per-task coding window. Exercise absent historical coding values without inventing a prior number, known numeric classifications and previous_unspecified, the twelve-field policy, immutable accepted snapshots, and canonical existing-head updates. Test short/truncated frames claiming the maximum with small actual input, fallible bounded growth, above-former-cap continuation, effective executor configuration readback, and retained-record decoding. Maximum configuration alone is not full-window load qualification."

[[milestones.gates]]
id = "CONFIGURATION_APPLY"
expectation = "Use the actual controller and shared collaboration mutation to read an existing failure/settings, apply a permitted definition-limit revision, and submit new work from its selected head. Check stale head/global/policy revisions, unknown/disallowed fields, zero/out-of-range and inconsistent complete configuration, immutable executor/inference/profile, changed-body conflict, lost reply, rollback, exact replay after restart/cancellation, and original-operation lookup. Older explicitly pinned submissions cannot revert the head; accepted tasks/children retain original settings; new-head tasks use the changed effective values. Compare effective before/after and record classification without inventing new numeric ceilings. The receipt, selected revisions and observed task/history/usage identify measurement; absent/failed/truncated evidence stays visible. No global reload, host/credential/protected authority or automatic original retry follows. Actual native/Codex caller joins use the existing integration gates; this controller gate alone is not a model or deployment qualification. The governance extension keeps the twelve-field vocabulary and existing three revision comparisons. Bind the exact classification-rule version, compare both effective configurations under the same current defaults, exclude version/provenance metadata, and treat unsupported comparison as widening without inventing a prior value. Preserve explicitly unspecified values. Refuse self-weakening or an untrusted change class; ordinary agent calls cannot edit governance policy. Audit comparison evidence with the new revision and original receipt. Protected operator rule promotion is a distinct action, not an added agent field or accepted-task hot reload."

[[milestones.gates]]
id = "STOP_ADMISSION"
expectation = "Actual controller transactions order voluntary completion against required task checks, explicit open waits and applicable pending inputs. Exercise both completion/input race orders, stale callers, refusal reason references, bounded continuation exhaustion and evaluator failure, restart with the same count and an independent-event-only reset. No successful executor exit or optional question manufactures completion or an implicit approval wait. Component records alone do not qualify the native/Codex safe-input boundary; those cases close in the existing executor and collaboration joins. Operator cancellation and emergency stop still proceed."


[[milestones.gates]]
id = "DECISION_RULES"
expectation = "Use exact immutable rule/configuration identities and closed deterministic decisions with bounded reasons and actual invocation provenance. Enforcing transition, decision event and original operation outcome commit atomically. Exercise rollback, restart, changed input under the same identity, lost reply and original lookup without duplicate effects. Shadow records an actual counterfactual at the selected point, including an enforcing refusal, without changing or delaying that outcome or dispatching effects. Refuse promotion with absent, failed, stale, incomplete or wrong-version evidence; require the configured deterministic promotion checks. Immutable safety checks remain enforced and model judgment never supplies authority."


[[milestones.gates]]
id = "EMERGENCY_CONTROLS"
expectation = "Exercise authorized stop/pause while ordinary rules refuse, fail or are unavailable; deny the same action through an agent-supplied operator label. Persist exact control identity/scope/reason before reporting it and distinguish stop intent from process closure. Temporary override binds prior/replacement policy and explicit expiry through the trusted-host operator path; restart and expiry races restore the correct prior policy unless a normal governed change explicitly superseded the override. Configuration/override expiry cannot silently resume held work. Uncertain expiry blocks ordinary admission while the independent recovery path remains available. Protected checks, real rollout approval, ownership fences and uncertain-effect reconciliation remain mandatory."

[[milestones]]
id = "V2_WORKER"
capability = "V2_WORKSPACE"
stage = "V2_S1"
title = "Coding worker execution"
acceptance = "Deliver worker supervision, pinned local-stdio Codex adapter, repository checkout and checkpoint reconciliation against the compiled protocol; this does not yet claim real-controller integration."
scope = "interface"
provides = ["CODING_WORKER"]
requires = ["V2_BOOT:TASK_PROTOCOL"]
owns = []
[[milestones.gates]]
id = "EXECUTOR"
expectation = "Use the pinned executor schema; lost turn/start reply is uncertain; duplicate command does not relaunch; own/reap process tree; no success from EOF or interrupt acknowledgment. Supplied-executor internal model/tool retries remain executor-owned. Retry notices do not create a second session or another turn/start; lost start acknowledgments retain the original identity and recovery checks. Do not claim native retry accounting observes hidden supplied-executor requests."
[[milestones.gates]]
id = "WORKSPACE"
expectation = "Every workspace builds/tests directly under host/task bounds; external starting revision/checkpoints are exact; secret output and admin credentials are excluded. Verify concurrent attempts share one worker-local object cache but retain distinct working files and indexes. Coding and clean validation use separate pinned worktrees. Interrupted cache construction/fetch is not adopted as a ready cache, cache maintenance preserves other active worktrees, and lost publication still reconciles the exact original checkpoint. A cache does not substitute for an externally acknowledged source checkpoint. Cache identity additionally binds the accepted repository ID, registration revision, source endpoint and checkpoint destination. Exact bindings may reuse objects; changing any bound component cannot alias, relabel or repoint an existing cache. Use distinct provisioned source/destination fixtures and preserve original marker and worktree bytes on mismatched or unsupported-format refusal."
[[milestones.gates]]
id = "CHECKPOINT"
expectation = "Persist intent through the controller protocol before Git publication; observe the original remote ref/commit after lost acknowledgment; stale publication cannot change selected checkpoint. The checkpoint and original publication intent/receipt carry the accepted repository registration revision. With distinct source and checkpoint endpoints, publication updates only the destination ref. Replay and recovery retain that revision, destination, exact ref and commit after catalogue changes; mismatched registration or destination cannot settle or select the original checkpoint."

[[milestones.gates]]
id = "AUTOMATIC_CLEANUP"
expectation = "Use explicit per-kind worker retention metadata with immediate successful-workspace eligibility, 24-hour failed/cancelled retention and seven-day idle-cache expiry. Absence of historical policy grants no cleanup authority. Exercise real completed coding and validation worktrees including ignored build outputs, exact canonical task/attempt/generation/repository joins, stopped descendants and acknowledged checkpoint/outbox outcomes. Active work, uncertainty, unresolved publication/handoff/reporting and live local cache references hold cleanup. Successful cleanup preserves uncommitted nonignored source; known failed/cancelled uncheckpointed edits can be discarded only after their configured interval and without active or unresolved dependencies. Persist first safe observation instead of using mtime or the command completed flag. Test timing boundaries, backward time, bounded batches/retries, foreign/symlink/replaced paths, wrong cache, interrupted deletion and process restart without retargeting another path. Delete an expired unreferenced reconstructible cache, and refuse one with active worktree/recovery pins. Retain exact durable task/history/result/checkpoint/audit/receipt/journal records and verify result lookup and duplicate-operation replay unchanged. Report reclaimed, skipped and uncertain outcomes with the policy revision. Real-controller completion and restart closure also belongs to V2_RECOVERY; disk quotas and durable-record retirement remain explicit separate gaps. Extend the same gate to eligible terminal supplied-executor homes: copy the required existing transcripts outside the removal set, verify complete durable copies before source deletion, and reclaim the unneeded remainder under the same worker policy. Preserve exact required platform journals at their existing paths when readers depend on them; neither duplicate journal copies nor removal of the top-level home directory is required. Exercise incomplete or failed copy, archive mismatch or replacement, interruption before and after publication, restart during removal, active or uncertain or saved-resume holds, and a completed workspace whose home cleanup is still pending. Keep original task/attempt identities, results and receipts unchanged and prove no new executor work or false recovered-conversation claim. A vendor database with no identified platform obligation is not a permanent exemption. This is local retained-transcript durability, not host-loss recovery, remote archival or a provider-neutral replay service."

[[milestones]]
id = "V2_E2E"
capability = "V2_WORKSPACE"
stage = "V2_S1"
title = "First real coding journey"
acceptance = "Submit a task to the real controller, run real Codex editing/build/testing, publish the source checkpoint, commit acceptance and restart to recover the same task/result."
scope = "integration"
provides = ["CODING_JOURNEY"]
requires = ["V2_STORE:TASK_STORE", "V2_STORE:TASK_API", "V2_STORE:COLLAB_MCP", "V2_WORKER:CODING_WORKER"]
owns = []
[[milestones.gates]]
id = "JOURNEY"
expectation = "Actual edit/build/test/submission and durable status/result survive controller restart. Executor exit zero with a failing check is not success. No native executor or full product UI is a prerequisite."
[[milestones.gates]]
id = "INTEGRATED_WORKER"
expectation = "The existing release in this gate covers the established task/agent/memory/repository catalogue. New question/document/review/wait/resolution producers and their full current-surface qualification are supplied by the collaboration component releases and V2_COLLAB_JOIN, not required to reopen the first journey. Run worker identity, duplicate-launch, pinned-executor, process-stop, checkpoint/publication and stale-result cases against the actual SQLite controller; a protocol fixture alone cannot close these obligations. On the configured host, establish the worker-wide resource group before children start and verify actual worker, executor, Git, build/check and helper membership. Existing process ownership/reaping remains the termination proof; group emptiness alone is insufficient. The pinned supplied executor discovers and calls the shared collaboration MCP surface against the actual controller. Verify caller-bound read/write outcomes match native operation semantics; lost mutation acknowledgment and fresh-process recovery retain one exact effect. Verify the supplied executor applies the accepted template model and effort and refuses effective-setting mismatch before work. Repository MCP reads return the same pinned content as the native adapter without creating another writable checkout. No per-task global configuration mutation, extra credentials or protected-release authority is inferred. In the coding journey, select catalogue metadata, receive the accepted immutable projection, edit/build and publish to a distinct configured checkpoint destination, then recover the exact registration/ref/commit after controller or worker restart. Shared repository reads expose the same retained source and safe public metadata; catalogue changes never rewrite the accepted worker command. For the selected governance extension, use the actual supplied-executor boundary to refuse an early done result with concrete remaining-work/input reasons, consume the exact required input before eventual acceptance, and stop at the continuation bound without resetting task/session/deadline/accounting. Prove cancellation remains responsive and an unknown dispatched effect is not replayed. A proposed adapter contract or opaque unobserved inner reply does not close this case."
[[milestones.gates]]
id = "BOUNDARIES"
expectation = "Run the selected host/SSH access and checkpoint loss boundary without key/sandbox/backup services; no duplicate effect on settled replay."

[[milestones]]
id = "V2_NATIVE"
capability = "V2_NATIVE"
stage = "V2_S2"
title = "Bounded native executor"
acceptance = "Deliver the bounded provider adapter and schema validation against the compiled task contract; controller integration is a separate release."
scope = "interface"
provides = ["NATIVE_EXECUTOR"]
requires = ["V2_BOOT:TASK_PROTOCOL"]
owns = []
[[milestones.gates]]
id = "NATIVE"
expectation = "Bound request/output/schema work, maximum-default finite request allowances, retained accounting/deadlines, structured errors and cancellation; no general shell or second coding harness. Validate real versus controlled provider behavior within allowed test capacity. Support bounded repeated steps with retained exact call IDs/results and context; explicitly selected web search is recognized separately from owned function calls. Disabled tools, unknown output items, malformed arguments, oversized context and multiple owned calls outside the selected profile refuse without unconfigured effects. Existing finite request accounting, deadlines and schema validation remain; no automatic executor fallback. Qualify exact safe diagnostic fields and capture failures, complete429 rejection versus partial/unknown transport, invalid/context/policy/quota/auth categories, valid delta/date Retry-After, bounded exponential jitter, malformed delay refusal, and controller-owned model/endpoint breaker states. Invalid-format feedback and truncated-output continuation each need distinct tests; classification alone is not qualification."

[[milestones]]
id = "V2_NATIVE_JOIN"
capability = "V2_NATIVE"
stage = "V2_S2"
title = "Native tasks on the controller"
acceptance = "Real controller input/result transitions drive the bounded native path and preserve its accepted outcome across restart."
scope = "integration"
provides = ["NATIVE_JOURNEY"]
requires = ["V2_STORE:TASK_STORE", "V2_STORE:COLLAB_MCP", "V2_NATIVE:NATIVE_EXECUTOR"]
owns = []
[[milestones.gates]]
id = "NATIVE_INTEGRATION"
expectation = "The existing release in this gate covers the established task/agent/memory/repository catalogue. New question/document/review/wait/resolution producers and their full current-surface qualification are supplied by the collaboration component releases and V2_COLLAB_JOIN, not required to reopen the first journey. Actual native task succeeds only after schema/acceptance checks; invalid output, timeout, exhausted explicitly configured lower allowance and restart retain honest outcomes. The historical fixed get_current_task journey retains its original source-bound qualification. The current configured direct-tool case uses a real native model request, an actual attempt-bound task read through the shared MCP catalogue, and a second native model request consuming that exact recorded observation. Each request has an acknowledged cumulative reservation and fresh currentness checks; a pending tool-result acknowledgment, cancellation or exhausted accepted request allowance prevents continuation. Unknown provider outcomes are not automatically replayed. Final schema/acceptance and intact-store restart preserve the accepted result. Controlled-provider tests and native-to-Codex delegation are separate evidence from the real-provider direct-tool journey. Extend the current shared-MCP two-request case with a real-controller read/write/read continuation using one shared MCP catalogue. Persist exact mutation intent before send and its recorded result before another model request; exercise lost acknowledgment, caller change, cancellation, deadline and restart between each boundary. Controller acceptance uses actual ordered receipts and request counts, not the old fixed one/two-request shortcut. Verify actual native requests use the model and effort bound to the accepted template. A repository analysis journey reads exact source through MCP and consumes it in the native continuation without launching the coding executor. Provider web-search and selected Codex MCP interoperability require their own actual-path evidence; controlled peers do not substitute for them. Use an accepted native read association selected from the catalogue. The actual MCP read and model continuation consume its exact registration revision and commit after a later head change, without launching coding work, acquiring material or exposing a private store key. For the selected governance extension, drive the actual native completion boundary with pending required work and addressed input. Verify bounded reason delivery and continuation under original deadline/debits, genuine input consumption before acceptance, exhaustion/failure without another request, no reset from self-originated progress, and exact decision identity after restart. Optional advisory notices create no new request. Drive known429 then success with the actual worker/controller and synthetic provider. Verify each POST has its own acknowledged debit and currentness check, retained tool calls are not repeated, notice inputs are not early-ACKed, and delay never crosses original deadline. Exercise exhaustion, cancellation during backoff, other-worker shared breaker behavior, one half-open probe, authentication suppression and stale completions. Then use one small permitted actual-provider journey; do not deliberately exhaust provider quota or relabel historical load evidence. Each charged non-tool request has exact canonical outcome evidence distinct from tool-call groups, compaction and completion continuation. Confirmed refusal then final success uses two debits and no fabricated call. A true idempotent tool timeout retries under its original identity and bounds; uncertain side-effecting calls use exact recorded-outcome lookup. Blocked-state events and actual adapter/control exits are exercised without manufacturing additional request allowance."

[[milestones]]
id = "V2_DELEGATE"
capability = "V2_NATIVE"
stage = "V2_S3"
title = "Native to coding delegation"
acceptance = "One native task delegates coding work using a separately identified child task and consumes one accepted child result."
scope = "integration"
provides = ["DELEGATION"]
requires = ["V2_NATIVE_JOIN:NATIVE_JOURNEY", "V2_E2E:CODING_JOURNEY"]
owns = []
[[milestones.gates]]
id = "DELEGATION"
expectation = "Child and separate parent-wait association commit atomically; child outcome settles the wait once without altering immutable prerequisites; parent association and accepted request debits/child reservations survive duplicate delivery/restart; delegation cannot provision credentials or change configured host/repository/provider access; a waiting parent releases its execution slot. Resolve the configured child repository association with the accepted parent. A later catalogue revision or disable does not retarget or invalidate that retained child binding; new external submissions still obey current admission. Duplicate child creation and restart preserve the same registration, source/destination tuple and original budget/deadline guards."

[[milestones]]
id = "V2_PRODUCT"
capability = "V2_PRODUCT"
stage = "V2_S4"
title = "Browser task collaboration"
acceptance = "Deliver a browser task list/detail interface against the actual controller, with first-release task operations and progress/output/code/result views complete."
scope = "integration"
provides = ["TASK_PRODUCT"]
requires = ["V2_STORE:TASK_API", "V2_STORE:COLLAB_MCP", "V2_E2E:CODING_JOURNEY"]
owns = []
[[milestones.gates]]
id = "READ_PAGINATION"
expectation = "Use the actual browser and controller to inspect the same permitted paged task, document and repository content exposed through the shared agent interface. Show partial/completed state, source identity and explicit continuation/exhaustion without hiding a full result in the page or replacing editable full content with a partial body. Exercise stale mutable content, fixed document versions across head changes, an oversized first list item, empty filtered pages, reconnect and current write/CAS safeguards. No automatic whole-result expansion or lost unsent edit follows from refresh."

[[milestones.gates]]
id = "PRODUCT"
expectation = "The existing release in this gate covers the established task/agent/memory/repository catalogue. New question/document/review/wait/resolution producers and their full current-surface qualification are supplied by the collaboration component releases and V2_COLLAB_JOIN, not required to reopen the first journey. In a real browser, submit/assign tasks, inspect dependencies/status/progress/available recent output, cancel/retry and open checkpoint/result references against the actual controller. The agent list uses the controller's actual agent directory, shows each durable agent with its distinct concurrent sessions, shared memory, pinned session revisions and linked tasks, and handles empty and paginated results without losing agents after task reassignment. Refresh/reconnect reflects committed outcomes; refused or uncertain actions are not shown as success; failed prerequisites stay visible. Complete a browser-created coding task through the real worker/checkpoint/result path and verify it again after controller restart. No second inbox/store, per-task grant engine or CLI-only acceptance. New coding/native forms use the maximum finite request default without a budget selector; omitted configured-child allowance leaves both native reservations, while explicit legacy limits and operation replay remain exact. Overflow and invalid child allocations still refuse. Map the established task-catalogue information surfaces to browser views, including existing task collaboration records, worker capacity, attempts, checkpoints, release checks and the five scoped repository reads. Show agent-bound operation outcomes without forging an active caller or enabling protected mutations. Verify automatic committed-state updates and a right-side durable activity feed under new activity, empty pages, pagination, disconnect/reconnect, controller restart and exhausted bounds. Preserve unsent form input and never repeat writes on refresh; display stale/disconnected state honestly. Activity ordering and deduplication use existing durable identities, with scope and secret redaction unchanged. Expose the shared safe catalogue list/detail projections and select registration ID plus exact current revision in task forms. Display an accepted task/checkpoint against its retained revision even after head changes. Operator catalogue forms use canonical operation identities and head-generation CAS; stale/repeated submissions show the recorded result or refusal. Neither normal task forms nor model tools can supply private store keys or resolved endpoints, acquire material or confer access. Verify UI/shared-interface field parity in the actual browser/controller journey. Expose the selected governance decision, exact rule/configuration revision, remaining-work reason and blocked continuation count. Show trusted-host control provenance without inventing a person. Actual operator pause/stop/recovery remains reachable when ordinary policy is broken; browser display alone never proves authorization or physical stop."

[[milestones.gates]]
id = "AGENT_LIFECYCLE"
expectation = "Register an immutable agent definition without creating a task or session. Operator-only retirement commits its marker and operation outcome atomically, compares the selected revision, and is replayable without a second effect; changed-body operation reuse and failed commit leave no partial change. Browser and shared agent directory pagination exclude retired identities before page bounds, while a cursor referring to a retired identity still advances. New submissions, delegations, recipient assignments and future-definition selection reject retired identities without implicit resurrection. Already accepted work can complete or be cancelled under its original rules. Exact original task/session/memory/history and actor references remain readable after retirement and restart; historical detail labels retirement and offers no new-session/settings action. Exercise the four selected production identities and a representative retired qualification identity against the canonical controller. Registration or a passing local test does not establish an executed agent session."

[[milestones.gates]]
id = "DEFAULT_ASSIGNMENT"
expectation = "Submit a root task without an explicit agent through the browser and canonical API, resolve the configured active registered default in the original submission transaction, and retain its exact definition in the accepted task. The selected deployment metadata chooses triage; an explicit engineer assignment stays engineer regardless of the default. Missing, invalid, unregistered or retired defaults refuse visibly without a task, session or partial accepted outcome. Exact retry after changing the default or retiring the original identity returns the original receipt and assignment; altered input under the same operation identity still conflicts. Restart preserves the accepted identity, definition, task and session. Browser error and reconnect handling preserve the original request identity and do not resubmit a write. Default resolution does not change configured child selection, existing executor/charter/limits admission, authority or scheduling. Verify the final fresh deployment exposes the selected default, and distinguish accepted routing from an executed triage session."

[[milestones.gates]]
id = "TASK_REASSIGNMENT"
expectation = "Through the canonical API, shared agent tools and browser, reassign an open task from one agent to another and retain the task identity while creating exactly one new session. Preserve original accepted input/definition, prior session/attempt history, acknowledged checkpoints, task acceptance obligations, dependencies, deadline and aggregate recorded usage. The successor uses its own accepted definition, charter and agent-keyed memory. A running triage task can commit a pending engineer handoff under explicit accepted repository/check configuration; it does not create a child task or turn read-only repository access into write authority. The successor cannot execute before actual prior execution closure and relevant unknown effects are reconciled. Exercise lost handoff acknowledgment, exact retry after target/default changes, conflicting body, stale revision, retired target, cancellation/completion races, stopped-but-uncertain effects, old session late reports and restart at intent/stop/activation boundaries. A failed commit leaves no partial lineage or extra session. Historical views preserve old provenance and cannot act as current sessions. Verify one task across native triage to coding engineer with actual handoff, durable checkpoint/result and fresh-process reconciliation; controlled peers, local tests and deployed behavior remain separately identified."

[[milestones]]
id = "V2_RECOVERY"
capability = "V2_QUALIFY"
stage = "V2_S5"
title = "Integrated recovery and concurrency"
acceptance = "Exercise two real workers against the first coding journey before full-scale qualification; missing native/product completion does not block these cases."
scope = "integration"
provides = ["RECOVERY"]
requires = ["V2_E2E:CODING_JOURNEY"]
owns = []
[[milestones.gates]]
id = "RECOVERY"
expectation = "Controller restart with a live worker, worker/incarnation restart, tunnel loss, stale output, lost Git/PR acknowledgments and uncertain effects settle without false success or blind replay. Include accepted repository R1 across catalogue R2/disable, lost registration acknowledgment and lost checkpoint-publication acknowledgment. Original task/child association, cache identity and checkpoint receipt remain exact after restart; no import, implicit reset, current-head fallback or blind repeat of an uncertain Git effect is permitted. Exercise automatic cleanup with the real controller and worker after terminal completion, restart during removal and existing uncertainty/publication holds; original accepted results and exact operation replay remain available after disposable files are reclaimed. For platform-owned retries, crash before reservation, after reservation, during backoff, after probe claim and after provider response. Original charges/operation IDs and uncertain model/tool effects survive; no speculative POST after restart. The original unknown request remains fenced; a canonically recorded unknown probe may enter metadata cooldown and later admit an independent probe without replaying the old effect. Verify explicit blocked state, durable reason event and canonical exit without resetting limits. Include applicable store rollback/backoff and idempotent tool retry/lost-result cases, preserving uncertain side-effect lookup. Supplied-executor lost-start/turn cases preserve original session and receipt rather than creating a fresh session for retry."
[[milestones.gates]]
id = "CANCEL"
expectation = "Both cancellation/completion transaction orders, repeated stop, missing process evidence and reconnect with the original deadline and unchanged accepted request accounting are exercised. Generation revocation does not prove a remote process stopped. Join the selected governance controls to real worker cancellation while the ordinary rule evaluator is broken. Preserve original Task/attempt/command and unknown-effect identities; verify policy restoration and explicit recovery after override expiry across restart. An expired override or configuration update cannot clear an emergency hold."

[[milestones]]
id = "V2_SCALE"
capability = "V2_QUALIFY"
stage = "V2_S6"
title = "Initial four-host mixed-load qualification"
acceptance = "Qualify one controller and three workers with 768 outstanding real native model requests and 24 active coding workspaces, each able to build; distinguish targets from measured provider/local limits."
scope = "integration"
provides = ["FOUR_HOST_EVIDENCE"]
requires = ["V2_RECOVERY:RECOVERY", "V2_NATIVE_JOIN:NATIVE_JOURNEY", "V2_PRODUCT:TASK_PRODUCT"]
owns = []
[[milestones.gates]]
id = "SCALE"
expectation = "Ramp 32/64/128/256 outstanding real native model requests per worker while eight coding workspaces build/test; 512 per worker is stress-only. Measure worker client request intervals; locally queued tasks and idle definitions do not count. Provider-internal processing concurrency is not the acceptance target."
[[milestones.gates]]
id = "RESOURCES"
expectation = "Measure peak memory/CPU/output, controller writes, cancellation and provider quotas under mixed load. Qualify the selected single group per worker using actual CPU, memory/swap and process controls: descendant inheritance, limit exhaustion and recovery retain honest task/effect states. Limits leave headroom for control/cleanup and concurrent builds. No per-task reservation, dynamic resource manager or separate heavy-build permit is introduced. Missing actual host controls remain a qualification gap; disk capacity/quota and journal-exhaustion work are separate. Numeric allowances and ancillary durations/latency budgets remain to establish."

[[milestones]]
id = "V2_RELEASE"
capability = "V2_QUALIFY"
stage = "V2_S7"
title = "Protected release preparation"
acceptance = "Prepare a reproducible source-bound release after included product/delegation/recovery/scale checks, without granting rollout or broad external-action authority."
scope = "integration"
provides = ["RELEASE_REVIEW"]
requires = ["V2_SCALE:FOUR_HOST_EVIDENCE", "V2_DELEGATE:DELEGATION", "V2_COLLAB_JOIN:COLLABORATION_JOURNEY", "V2_CHARTER_JOIN:CHARTER_JOURNEY"]
owns = []
[[milestones.gates]]
id = "RELEASE"
expectation = "Automatic exact-revision merge requires protected checks; failed/missing/stale/wrong-revision evidence refuses. Final rollout still needs artifact/target-bound human authorization and mandatory checks."
[[milestones.gates]]
id = "EVIDENCE"
expectation = "Record native traceability separately from implementation/test results; measured coverage reviews uncovered critical paths; no blanket 100% gate or invented conformance. Exact source/binary/profiles accompany actual tests."
[[milestones.gates]]
id = "SCOPE"
expectation = "Shared task-profile amendments, unresolved scope choices and dependency qualification are closed before their claimed release; no inherited full-profile guarantee is silently waived. The four focused governance families are required for a release claiming the selected expansion. Record their new gate readiness separately from historical narrow limit/stop/acceptance evidence. They do not activate the whole shared governance outcome, remove the permission-engine deferral or waive protected-release human authorization."

[[milestones]]
id = "V2_LATER_SCALE"
capability = "V2_QUALIFY"
stage = "V2_S8"
title = "Later ten-host capacity qualification"
acceptance = "Demonstrate the retained ten-host objective only after its actual active-agent workload and hardware envelope are defined; this milestone is not a first-release prerequisite."
scope = "integration"
provides = ["TEN_HOST_EVIDENCE"]
requires = ["V2_SCALE:FOUR_HOST_EVIDENCE"]
owns = []
[[milestones.gates]]
id = "TEN_HOST_BEHAVIOR"
expectation = "Actual ten hosts each run at least1000activeagents and10activeworkspaces during the same interval under an explicitly selected workload/resource profile. Queued definitions and four-host evidence do not satisfy this gate; keep it incomplete until that actual capacity is demonstrated."


[[milestones]]
id = "V2_COLLAB_CONTRACT"
capability = "V2_CONTROL"
stage = "V2_S4"
title = "Shared collaboration records and compiled contract"
acceptance = "Compile all four component consumers against one typed contract and qualify common record/revision/provenance and retained-state compatibility on the actual controller."
scope = "integration"
provides = ["COLLAB_PROTOCOL", "COLLAB_RECORDS"]
requires = ["V2_STORE:TASK_STORE", "V2_STORE:TASK_API", "V2_STORE:COLLAB_MCP"]
owns = []
[[milestones.gates]]
id = "CONTRACT"
expectation = "Compile the actual task coordination, question/queue, document/review and notification consumers against closed actor, record, resolver, outcome and typed non-task activity forms. The integrator owns protocol, schema/wire disposition, MCP routing/catalogue and metadata limits. Fixture compilation releases COLLAB_PROTOCOL only; it is not the actual COLLAB_RECORDS producer."
[[milestones.gates]]
id = "RECORDS"
expectation = "Use real SQLite transactions: durable identities; next collaboration revision in commit order distinct from operation receipts; atomic record/activity/receipt; typed non-task subjects without fabricated tasks; expected revision conflicts; duplicate operation replay after lost acknowledgment; changed-body refusal; rollback and restart. Validate bounded typed task/question/answer/review/document primitive fixtures using the real shared transaction, revision and receipt code. These fixtures are not the later domain handlers and do not require them; each domain batch and the final joined gate exercise the actual handlers. No independent store or agent inbox."
[[milestones.gates]]
id = "PROVENANCE"
expectation = "Derive actor context through the actual authenticated invocation path, not a supplied name. Exercise agent versus trusted-host operator display, direct recipient binding and unsupported recipient refusal. Product decisions confer no protected rollout, host, repository, provider or credential authority; ordinary work with no explicitly requested wait gains no human gate. Do not claim authenticated-person or team semantics unsupported by the selected host trust."
[[milestones.gates]]
id = "COMPATIBILITY"
expectation = "Exercise a deliberate database/wire compatibility disposition against retained current tasks, memory, repositories, accepted definitions, operation receipts and pending worker state. Unsupported/corrupt inputs remain untouched; no implicit reset, profile expansion, schema-number invention or migration claim follows from this declaration. Compare actual serialized aggregate native tool schemas to accepted metadata bounds before selecting new profiles; old accepted profiles remain byte-exact."

[[milestones]]
id = "V2_COLLAB_DOCUMENTS"
capability = "V2_CONTROL"
stage = "V2_S4"
title = "Versioned documents and exact-subject review"
acceptance = "Deliver bounded immutable document/comment and review operations plus browser/MCP consumers through the shared controller. This releases its real-controller component seam with typed fixtures for peer producers; full product qualification closes at V2_COLLAB_JOIN."
scope = "interface"
provides = ["DOCUMENT_RECORDS", "REVIEW_RECORDS"]
requires = ["V2_COLLAB_CONTRACT:COLLAB_PROTOCOL", "V2_COLLAB_CONTRACT:COLLAB_RECORDS"]
owns = []
[[milestones.gates]]
id = "DOCUMENTS"
expectation = "Real controller tests cover immutable content/version identities, non-reused version numbers, computed-versus-supplied content identity refusal, immutable comments, edit creates new version, concurrent expected-head conflict, retained versions and restart. Repeat exact requests after lost acknowledgment without creating another version; never retarget accepted review to the edited head. Shared comment handling also serves questions, verified in the joined case."
[[milestones.gates]]
id = "REVIEWS"
expectation = "Bind review to exact immutable document version or result/checkpoint reference. Compare direct reviewer against actual controller caller; decision immutable and subject retained. Editing never inherits a prior decision. Withdrawal records caller/reason and committed affected-review activity. Test decision/withdraw/edit races, changed-body replay, stale revision, restart, and no protected authority from a product review. Document archive remains refused while an open review needs it; no pruning policy is introduced. REVIEW_RECORDS is released only with the actual record reader and canonical terminal transition, not a fixture."

[[milestones]]
id = "V2_COLLAB_QUESTIONS"
capability = "V2_CONTROL"
stage = "V2_S4"
title = "Questions and complete supported decision queue"
acceptance = "Deliver immutable questions/answers and lifecycle handling with complete bounded direct-recipient question/review queries. This releases its real-controller component seam with typed fixtures for peer producers; full product qualification closes at V2_COLLAB_JOIN."
scope = "interface"
provides = ["QUESTION_RECORDS", "DECISION_QUEUE"]
requires = ["V2_COLLAB_CONTRACT:COLLAB_PROTOCOL", "V2_COLLAB_CONTRACT:COLLAB_RECORDS"]
owns = []
[[milestones.gates]]
id = "QUESTIONS"
expectation = "Actual controller/browser/MCP cases preserve posed prompt/options/typed fields, immutable answers and comments, direct-recipient answer/replacement/withdraw authority, retained original answers and exact operation replay. Open questions outlive an inactive asker under current responsible owner; cancellation/supersession retains original poser, caller and reason, replacement actor/link, and is refused after answer. Owner changes retain open questions without mutating accepted task execution identity. Blocking questions take no default; prompts retain recommendation and operator questions ask exactly one decision. Moot records actor and the answer that made it moot, removes the marked answer product effect and creates no product decision effect. Test simultaneous answer/withdraw/moot/cancel, missing or stale revisions, inactive asker, lost acknowledgment and intact-store restart."
[[milestones.gates]]
id = "QUEUE"
expectation = "Use actual canonical question records and the compiled typed review-record fixture to qualify the bounded queue projection: matching direct-recipient entries, closed rows, multiple pages, continuation/no omission, stale revision and restart. This is the query seam only; both actual question and review producers, complete supported decisions and external-notice non-substitution close primarily at V2_COLLAB_JOIN#QUEUE."

[[milestones]]
id = "V2_COLLAB_WORK"
capability = "V2_CONTROL"
stage = "V2_S4"
title = "Task coordination and resolver waits"
acceptance = "Deliver separate bounded workflow waits over actual task/question/review resolvers while preserving accepted execution and stop semantics. This releases its real-controller component seam with typed fixtures for peer producers; full product qualification closes at V2_COLLAB_JOIN."
scope = "interface"
provides = ["RESOLVER_WAITS"]
requires = ["V2_COLLAB_CONTRACT:COLLAB_PROTOCOL", "V2_COLLAB_CONTRACT:COLLAB_RECORDS"]
owns = []
[[milestones.gates]]
id = "WAITS"
expectation = "Use actual wait/task rows in the current controller store with typed question/review resolver fixtures and multiple bounded waits. Refuse agent/team/external-party and closed resolvers; refuse direct and transitive cycles through waits and parent links. Open wait prevents new attempt admission independently of unchanged execution prerequisites. Retain prior logical workflow state/assignee at first wait; commit wait before initiating quiescence and release slot only after actual execution has stopped. Resolve terminal outcome, all affected waits and durable delivery associations in one transaction; preserve exact outcome occurrence and typed unblock outcomes including negative terminal outcomes. Restore last-wait state only for nonterminal tasks. Test concurrent closure, partial commit refusal, lost acknowledgment, restart, already terminal/cancelled task, unresolved external effect and both stop/resolution orders; never resurrect stopped attempts or overwrite accepted inputs/deadlines/accounting. Fixture tests qualify the seam, actual coding and native consumer closure remains required at V2_COLLAB_JOIN. Use the frozen typed terminal-transition and stop-observation seams for peer producers here; do not claim actual question/review terminal atomicity or executor stop from a fixture. Full atomic resolver/wait/delivery and actual stopped-slot closure are primarily V2_COLLAB_JOIN#WAITS."

[[milestones]]
id = "V2_COLLAB_DELIVERY"
capability = "V2_WORKSPACE"
stage = "V2_S4"
title = "Durable resolution delivery and actual consumers"
acceptance = "Deliver real durable resolution records and compiled native/supplied-executor continuation adapters using existing commands, retention and typed safe-input fixtures; actual joined continuation closes at V2_COLLAB_JOIN. This releases its real-controller component seam with typed fixtures for peer producers; full product qualification closes at V2_COLLAB_JOIN."
scope = "interface"
provides = ["RESOLUTION_CONSUMERS"]
requires = ["V2_COLLAB_CONTRACT:COLLAB_PROTOCOL", "V2_COLLAB_CONTRACT:COLLAB_RECORDS"]
owns = []
[[milestones.gates]]
id = "DELIVERY"
expectation = "Using real controller delivery rows and typed resolver/recipient fixtures, commit the resolution and exact intended destination/association before wake; target active asker or current responsible record owner. Actual domain resolution/recipient producers close at V2_COLLAB_JOIN. Use stable occurrence/delivery IDs and existing controller commands, with no independent inbox or notification poll. Real restart after resolution and before delivery, lost wake, lost acknowledgment, owner change, duplicate delivery and concurrent cancellation leave one durable outcome; acknowledgment follows durable receiving-continuation retention. Stale or terminal tasks remain unchanged."
[[milestones.gates]]
id = "CONSUMERS"
expectation = "Compile actual native and supplied-executor consumer adapters and exercise safe-input/acknowledgment state machines against the common contract with controlled executor/peer fixtures. Preserve currentness, stop/effect guards, session/definition, deadline/accounting, stable occurrence identity and terminal-task refusal. Actual native/Codex consumption, retained outcome before acknowledgment, single continuation, no running-tool cancellation and restart close primarily at V2_COLLAB_JOIN#CONSUMERS; fixture append acknowledgment is not actual model consumption."

[[milestones]]
id = "V2_COLLAB_JOIN"
capability = "V2_PRODUCT"
stage = "V2_S4"
title = "Joined collaboration journey and interface parity"
acceptance = "Qualify all four functional batches together using actual controller, worker/native and browser/MCP consumers, with source-bound recovery evidence."
scope = "integration"
provides = ["COLLABORATION_JOURNEY"]
requires = ["V2_COLLAB_CONTRACT:COLLAB_RECORDS", "V2_COLLAB_WORK:RESOLVER_WAITS", "V2_COLLAB_QUESTIONS:DECISION_QUEUE", "V2_COLLAB_DOCUMENTS:DOCUMENT_RECORDS", "V2_COLLAB_DOCUMENTS:REVIEW_RECORDS", "V2_COLLAB_DELIVERY:RESOLUTION_CONSUMERS", "V2_PRODUCT:TASK_PRODUCT", "V2_DELEGATE:DELEGATION", "V2_RECOVERY:RECOVERY", "V2_E2E:CODING_JOURNEY", "V2_NATIVE_JOIN:NATIVE_JOURNEY"]
owns = []
[[milestones.gates]]
id = "SURFACES"
expectation = "Map every new supported operation to actual browser and shared-agent entry points under each operation's real caller rules. Exercise versions/comments/questions/answers/reviews/waits/decisions and complete queue through both routes, common receipt outcomes, stale page/revision, repeated form/call, caller denial, redaction, bounds, live reconnect and durable activity. Do not impersonate an active agent or authenticated person. Accepted task tools remain unchanged after catalogue expansion; measure native serialized schemas and verify old/new profile selection and unsupported operation refusal."
[[milestones.gates]]
id = "JOURNEY"
expectation = "Actual agent delegates work, poses a routed blocking question, commits the wait, releases its slot after stopping, receives committed answer once, resumes under its original session association, and submits exact document version or code/result reference for review. Both MCP and browser observe the same records/outcomes. Repeat with restart before delivery, lost acknowledgment, duplicate continuation delivery, multiple/negative wait outcomes, document edit while review is open, and resolution racing cancellation or side effect. Verify all common record/provenance and question/document/comment rules through actual producers. No broad shared collaboration, scale, deployment or implementation/test coverage claim follows from planning associations."
[[milestones.gates]]
id = "QUEUE"
expectation = "Join actual question and document/review producers to the canonical decision query. Every supported open direct-recipient question and review remains visible across bounded pages and continuation, mutations, stale views and restart; notices outside the queue never substitute for owed entries. Neither a synthetic review row nor a question-only query closes this gate."
[[milestones.gates]]
id = "WAITS"
expectation = "Through actual task/question/review terminal operations, prove terminal outcome, all affected wait resolutions and delivery associations commit atomically, including multiple and negative outcomes, failed commit, concurrent close, restart and lost acknowledgment. Observe the actual wait commit before initiating quiescence or sending its stop command; a refused or failed wait commit produces neither. Observe actual stopped execution before releasing its slot. Restore prior state/assignee only for a nonterminal task; immutable prerequisites, cancellation and uncertain effects remain independent barriers. Peer fixtures from component gates do not close this obligation."
[[milestones.gates]]
id = "CONSUMERS"
expectation = "Run actual native and pinned supplied-executor adapters on committed resolutions. Exercise active asker delivery, inactive asker fallback to current responsible owner, and owner change before delivery using actual recipient/owner records; correct acknowledgment to the wrong recipient is a failure. Observe the supported safe input boundary and durable receiving-continuation outcome before acknowledgment. Duplicate/lost acknowledgment/restart retains one continuation per occurrence, never cancels a running side-effecting tool, never reopens terminal work or alters original session/definition/deadline/accounting. Actual wake/slot/stop/currentness checks, not fixture signals or browser updates, close these obligations. For governance completion, the same actual native/Codex input ACK that discharges owed resolution work is the completion-check input proof. Exercise a resolution/completion race and lost ACK; assignment, Begin or a canonical audit sibling alone cannot satisfy it, and no second continuation is created."


[[milestones]]
id = "V2_CHARTER_RECORDS"
capability = "V2_CONTROL"
title = "Charter source and durable turn contract"
acceptance = "Compile the actual native, supplied-executor and controller consumers against one closed source/turn/reply contract and qualify the durable source and audit-record producer."
scope = "interface"
provides = ["CHARTER_RECORDS"]
requires = ["V2_STORE:TASK_STORE", "V2_STORE:TASK_API"]
stage = "V2_S4"
owns = []
[[milestones.gates]]
id = "SOURCE"
expectation = "Resolve complete authoritative charter source using existing immutable source identity; validate normative source/heading/exact-sentence references without claiming behavioral judgment. Reuse an existing identity when sufficient, preserve source distinct from rendering/compaction, and keep reusable definition charter separate from task instruction. Real controller operations test exact source bytes, expected binding conflict, unsupported references, changed-body replay, failed commit and restart; no source service or access grant is added."
[[milestones.gates]]
id = "RECORDS"
expectation = "Retain accepted initial source and pending/effective session bindings separately. Exercise original operation and reply occurrence identity, record/activity association, bounded reads and restart. Compile both executor consumers. This component producer does not prove actual model-visible delivery, every-reply visibility or full-platform conformance."
[[milestones.gates]]
id = "COMPATIBILITY"
expectation = "Freeze the common logical source/turn/reply envelope, native response-to-turn mapping, empty applicability representation, source retention under the current policy and wire/schema/worker disposition. Compile the supplied-executor consumer with an explicit unsupported-boundary disposition where the pinned API lacks the required signal; this does not qualify that mapping. Use real old/new readers and retained pending records; historical missing metadata remains absent and unsupported/corrupt state is not rewritten. The Codex task/public outer-turn exception scopes V2_CHARTER_CODEX#DELIVERY and the joined release to qualified initial delivery, genuine observable final-result references and explicit pending/unsupported updates where no later public boundary is qualified. Full native model-turn behavior remains required. A supplied item or result is never serialized as a fabricated native model occurrence. No fabricated charter data, reset or replay of uncertain work."

[[milestones]]
id = "V2_CHARTER_NATIVE"
capability = "V2_NATIVE"
title = "Native charter delivery and update continuity"
acceptance = "Use the actual native model/tool loop to supply the effective source and retain each turn reply reference record, including a changed charter and native compaction continuation."
scope = "integration"
provides = ["NATIVE_CHARTER"]
requires = ["V2_CHARTER_RECORDS:CHARTER_RECORDS", "V2_NATIVE_JOIN:NATIVE_JOURNEY"]
stage = "V2_S4"
owns = []
[[milestones.gates]]
id = "DELIVERY"
expectation = "Controlled Responses peer receives the reusable charter separately from task instruction in the actual request; replies with final text and tool calls carry structured references tied to the actual effective turn revision. Confirm durable records through the real controller, including invalid references preserved for audit. A final task output wrapper alone is not every-reply evidence; actual provider qualification is separate."
[[milestones.gates]]
id = "UPDATE_COMPACTION"
expectation = "Commit a pending native charter change while a tool result is outstanding; the old turn retains its original source and the next safe request receives the new mid-conversation instruction. A burst schedules one low-priority compaction; an already due compaction incorporates a newer charter and its summary uses the new charter. Crash/cancel/lost acknowledgment preserve original occurrence and pending state without request resend or summary-as-authority. Use the actual selected compaction consumer; scheduling a flag alone does not qualify it."

[[milestones]]
id = "V2_CHARTER_CODEX"
capability = "V2_WORKSPACE"
title = "Pinned supplied-executor charter consumer"
acceptance = "Qualify initial charter delivery and observable final-result references on the actual pinned supplied-executor adapter; qualify any later supported public input boundary or retain explicit pending/unsupported updates."
scope = "integration"
provides = ["CODEX_CHARTER"]
requires = ["V2_CHARTER_RECORDS:CHARTER_RECORDS", "V2_E2E:CODING_JOURNEY"]
stage = "V2_S4"
owns = []
[[milestones.gates]]
id = "DELIVERY"
expectation = "Actual pinned executor receives exact initial charter separately from task input, with accepted model/effort/tools/limits unchanged. Retain original intent, dispatch, thread ACK and outer-turn association. Qualify any selected supported later input separately; if unavailable, prove pending/unsupported rather than effective. Use actual final-result declarations as the observable citation surface; do not require inaccessible inner model replies. Scripted peers prove adapter mechanics only; actual pinned consumption remains an acceptance item. Stop/currentness/accounting and accepted TaskInput remain unchanged."

[[milestones]]
id = "V2_CHARTER_JOIN"
capability = "V2_PRODUCT"
title = "Joined charter references and recovery"
acceptance = "Join actual source, native and pinned supplied-executor producers to durable reply/audit reads with explicit configuration adoption and recovery."
scope = "integration"
provides = ["CHARTER_JOURNEY"]
requires = ["V2_CHARTER_RECORDS:CHARTER_RECORDS", "V2_CHARTER_NATIVE:NATIVE_CHARTER", "V2_CHARTER_CODEX:CODEX_CHARTER", "V2_PRODUCT:TASK_PRODUCT", "V2_RECOVERY:RECOVERY"]
stage = "V2_S4"
owns = []
[[milestones.gates]]
id = "REPLIES"
expectation = "Keep full native per-model-reply checks. For Codex read the actual public final assistant result for each admitted outer turn with original thread/turn/item identity, separately retained delivered source, genuine declared references or explicit absence. Restart and bounded reads preserve these facts. Commentary/tool-only intervals are not counted as complete model replies; no fabricated clause metadata. Read the same original association from task/result/history/audit projections across restart and bounded pages. A rendered or compacted source, lookup commit or latest revision cannot substitute for the original binding."
[[milestones.gates]]
id = "AUDIT"
expectation = "Keep matching/wrong revision/unknown clause/altered sentence/unresolved source/missing/malformed cases for native replies and observable Codex final results. Missing hidden replies are unsupported visibility, not fabricated invalid replies. Preserve original received references and deterministic check outcome with the associated reply or result. Findings remain passive; no retry/reset/feedback loop/compaction/blocking/model judge or new authority follows solely from a citation finding; independently invalid task output still follows its existing validation. Test the explicitly frozen empty/streaming disposition without inventing shared policy."
[[milestones.gates]]
id = "ADOPTION"
expectation = "Keep publication authority/CAS, pending load, two-session separation and no idle wake. Native adopts at its real next request boundary. Codex keeps the delivered source during its active outer turn; only an explicitly qualified supported outer boundary may accept a later source. Otherwise pending/unsupported persists visibly through restart without resending or restarting work. Preserve accepted task/definition/model/tool/limit snapshots and all prior associations. Exercise concurrent current-source conflict and change during model/tool activity. No change interleaves a tool call and its result; future-only selected head change is not adoption evidence."
[[milestones.gates]]
id = "RECOVERY"
expectation = "Keep source publication/adoption/retention/ACK cuts. Native occurrence recovery unchanged. Codex recovers original delivery and observable outer result identities with exact source, pending updates, original receipt and known thread history; unavailable history stays unavailable. No duplicate thread/start, turn/start, provider work or mediated side effect after uncertain ACK. Cancellation still requires process stop evidence and reconciles late original results without reviving authority. Read audits after compacted input uses a different representation; authoritative source still resolves under retained-data rules."

```
