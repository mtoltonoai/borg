# Collaboration Platform Activation Record And Review

**SHARED ACTIVATION SAVED; V1 INTEGRATION ADOPTED.** The reviewed six-file shared activation was applied and verified on 2026-10-11 after Matthew clarified its scope. COLLAB, seven milestones, their 109 allocations and qualification dependencies have their single authored definition in the [capability specification](../implementation-capabilities.md) and [phase specification](../implementation-phases.md). All 109 quotations now resolve to active shared requirements; the other 25 capabilities and 93 milestones remain proposed. [Current activation evidence](../../../../../borg-runtime/artifacts/planning/collaboration-shared-activation-zidp_b4j/README.md).

The collaboration platform adds typed product records and user interfaces over Borg's existing shared durability, events, identity and encryption components. It does not add another storage engine, log, snapshot mechanism, or independent copy of agent state. Its own task, question, document and revision records are domain data; all access to agent session state and operations still goes through the frontend API.

## Applied Reviewed Change

The [exact activation patch](collaboration-platform-activation/activation.patch) records the reviewed changes to six files. Its original bytes are preserved; the pending confirmation note was reconciled only after exact application was verified:

| File | Saved change |
|---|---|
| `.duvet/config.toml` | Activate only the existing collaboration `build.md` outcome. Keep adapt and all other outcome selections unchanged. |
| `spec/record.md` | Record native build and Matthew's 2026-10-11 direction confirmation; the live record now states successful activation; reconcile Part C to 33 active and 13 commented files; replace the outcome-bound independent-engine/search/pipeline implications with shared-component reuse and explicitly unselected physical placement/indexing. Preserve the existing task/viewer/version defaults for review when sizing each slice. |
| `spec/decisions/deferred.md` | Record collaboration as the selected product while retaining adapt as an unselected alternative. Preserve unrelated deferred choices and the existing section heading. |
| `spec/overview.md` | Reconcile the collaboration entry with native build and shared-component reuse. |
| `spec/glossary.md` | Remove the collaboration section's obsolete deferred label. Preserve its terms. |
| `spec/decisions/collaboration-platform/build.md` | Clarify shared-component reuse, API-only agent-state access, product answers versus core authority, and runtime exclusivity versus concurrent sessions; narrowly qualify the two product pause obligations as non-emergency. |

Part C and the live configuration agree: 33 active files and 13 commented files, with build active and adapt commented. The shared record confirms the actual activation date after verified application, distinguishes the separate scoped COLLAB/seven-milestone approval, and leaves the 25 foundation capabilities and 93 foundation milestones proposed.

No approved implementation, capability or phase file is part of the patch. The existing API boundary is unchanged:

> The service MUST reach the agent platform through the platform's API rather than through the durable store.

The existing node-loss durability requirement is unchanged. Sharing a component does not grant access to agent internal keys or imply a shared physical storage instance. This follows [shared mechanisms](../../../../constitution.md#vi-generic-mechanisms-belong-in-the-shared-framework), [durable-store durability](../../../core/shared-framework.md#durable-store-durability) and [the product API boundary](../../../decisions/collaboration-platform/build.md#it-is-a-client-and-an-adapter-not-a-store-client).

Two exact requirement identities changed under the retained `Lifecycle Intent Is The Only Pause Path` heading:

| Retired sentence | Active replacement sentence |
|---|---|
| An agent's run, paused, or retired intent MUST be the only way to pause its sessions. | An agent's run, paused, or retired intent MUST be the collaboration platform's mechanism for requesting a non-emergency pause of its sessions. |
| A pause requested through any path MUST take effect by setting that intent. | A non-emergency pause requested through the collaboration platform MUST take effect by setting that intent. |

Those changes keep an emergency control, including one initiated through the product UI, independent of the collaboration product and its availability. All other original normative identities remain exact. The patch does not silently reinterpret the retired sentences as evidence for their replacements.

Product go-aheads remain subject to platform policy and grants. They are not core decision-hook answers and cannot bypass [checkers-only](../../../decisions/authority-widening/checkers-only.md#a-deferral-to-a-person-blocks-instead). The adopted collaboration acceptance includes a valid product go-ahead paired with a denied core operation; that operation remains denied. Runtime exclusivity is not a task claim lease and does not forbid the legitimate concurrent sessions allowed by core identity rules. Its concrete enforcement interface still needs design review.

## Behavior Included By Native Build

The existing built outcome is more than a UI. Activating its whole file activates these obligations, with their original guards:

| Behavior | Existing source sections |
|---|---|
| Disposable node state, durable product records, one gapless commit revision per tenant, durable IDs and tenant-local cited references | `It Runs On Many Servers With No Node State`; `Changes Within A Tenant Are Ordered By One Revision`; `A Durable Store Behind It`; `Every Record Has A Durable Id Beside Its Cited Reference` |
| Authenticated authors, fail-closed authorization, bridge identity/dedupe, closed operator team and recovery | `Identity Comes Only From Authentication`; `The Operator Team Is Closed` |
| Explicit task assignment/status, parent-depth limits, record-resolver waits, combined wait/parent cycle refusal, atomic resolve/unblock and prior status/assignee restoration | `Coordination Logic Lives In The Collaboration Platform`; `A Block Is A Wait On A Resolver` |
| Question survival/reassignment, immutable answers, authenticated operator review, matching typed go-aheads and descendant gates, complete decision queue | `A Question Outlives Its Asker`; `A Decision Is Recorded Under The Principal That Made It`; `The Decision Queue Is Complete By Construction` |
| Protected document versions and tenant encryption, lifecycle intent, atomic retirement cleanup and answer rerouting | `A Durable Store Behind It`; `Lifecycle Intent Is The Only Pause Path` |
| API-only agent integration, revision-derived record events, assignment subscriptions, open-work stop admission, platform status/recovery, no second inbox or watchdog | `It Is A Client And An Adapter, Not A Store Client`; `It Publishes Every Record Change`; `It Manages Subscriptions And Answers Hooks`; `It Keeps No Delivery Mechanism Of Its Own` |
| Fresh unstored tail tokens, person notifications, deadline housekeeping, restorable archive, overdue events and durations | `It Manages Subscriptions And Answers Hooks`; `It Keeps No Delivery Mechanism Of Its Own`; `Housekeeping Runs From Deadlines` |
| Conditional definition serving and retirement confirmation | `It Serves Definitions`; `Lifecycle Intent Is The Only Pause Path` |

All headings above refer to [the existing built outcome](../../../decisions/collaboration-platform/build.md). The exhaustive exact file/section/sentence list is generated from native Duvet in the evidence run; this table is a review guide, not a second requirement catalog.

Five explicitly declarative-only requirements remain inactive under the current imperative choice. Definition-identity non-reassignment remains active. Pruning, imported answers, gates and similar clauses retain their operation-specific conditions. The empty final `Block Types Share One Document Model` heading adds no requirements. Descriptive chat/wiki ambitions do not define an additional editor or block protocol.

Task assignee, record owner and project coordinator are product concepts, not session/partition ownership or fences. A product task blocked on a record resolver is not automatically a quarantined or failed session. Product waits are not the Duvet Cap development graph. Existing explicit assignment is sufficient; no self-claim, expiring task lease, recovery claim or goal entity is introduced.

## Integrated Delivery Scope

The former [capability extension](collaboration-platform-activation/capability-extension.md) and [milestone extension](collaboration-platform-activation/milestone-extension.md) now point to the single canonical capability and phase documents. The 25 foundation capabilities and 93 foundation milestones remain proposed, with their existing acceptance and 1,846 allocations retained; only the two explicitly approved qualification prerequisites are added to foundation consumers. Scoped-approved COLLAB has API, AUTH, DA, FEED, R and SF prerequisites and seven incremental deliveries:

| Scoped-approved collaboration milestone | Observable release and prerequisite boundary |
|---|---|
| `COLLAB_S1` | Reviewed product commands, records, resolver transitions, authority and durability contracts. Core boundary receipts enable review, not claims of implemented dependencies. |
| `COLLAB_S2_RECORDS` | Real shared-store record/revision/ID primitives and restart checks using DA/SF releases; no independent persistence subsystem or public-operation claim. |
| `COLLAB_S3_TASKS` | Authenticated explicit assignment and task/resolver primitives using real AUTH/D/SF/R releases. Full mixed-resolver obligations close in the next delivery. |
| `COLLAB_S5_DECISIONS` | Actual questions, document/review resolvers, operator controls, complete wait graph and decision queue; authority-bypass negative cases. |
| `COLLAB_S5_AGENT_LINK` | Actual API, event, subscription, lifecycle, stop/status-hook and retirement callers. Boundary fixtures cannot substitute for these producers. |
| `COLLAB_S5_VIEWERS` | Real tail-token brokerage, shared live views, person notifications and durable-deadline housekeeping. |
| `COLLAB_S6_QUALIFICATION` | Integrated uncertain-commit, restart, replay, isolation, lifecycle and capacity evidence plus dependency parity. |

The canonical phase specification adds the scoped-approved `COLLAB_S6_QUALIFICATION:QUALIFIED` prerequisites to the otherwise-proposed `SIM_S6` and `G_S6`; existing final `G_S7` coverage/conformance gates remain. Tests and evidence accumulate at every delivery. These are milestone views within the approved seven stages, not new global barriers. COLLAB and its seven milestones are scoped-approved; foundation declarations remain proposed. Every ownership list is empty and no work is ready. No component path or worker assignment is adopted.

Canonical mixed-approval totals: **26 capabilities, 87 summary edges, 100 milestones, 533 interface references, 520 distinct milestone edges, 109 interfaces and 454 gates**. Typed-reference/cycle checks, source-order invariance and exact generated-view reproduction pass. The seven collaboration increments have scoped implementation-spec approval; the foundation plan is not blanket-approved.

Integration acceptance explicitly covers assignment-commit/publication/subscription crash windows, stable repeated stop-decision verdicts, new assignment racing a stop, and an actual product deadline callback release over the shared queue. Required atomic retirement and resolver closure must fit the real transaction bounds; a mismatch surfaces a representation/bound decision rather than silently replacing atomicity with batching. No sequencing protocol, task lease or storage representation is adopted by these tests. The product callback release is delivered within `COLLAB_S5_VIEWERS`; existing session-timer completion is not treated as that release.

## Historical Native Duvet Baseline

Installed Duvet **0.4.3** ran in new isolated copies with the original selection plus each evaluated outcome. Planning sources contain generated native citations only; runtime source globs and test evidence are excluded. Each report runs normal and strict with `--require-citations true --require-tests false`.

| Evaluation | Covered / raw native total | Missing | Partial / unmatched | Normal / strict exit |
|---|---:|---:|---:|---:|
| Historical pre-activation selection and draft | 1,846 / 1,846 | 0 | 0 / 0 | 0 / 0 |
| Existing build selected; unchanged draft | 1,846 / 1,955 | 109 | 0 / 0 | 0 / 1 |
| Final proposed activation patch; unchanged draft | 1,846 / 1,955 | 109 | 0 / 0 | 0 / 1 |
| Final candidate plus proposed product extension | 1,955 / 1,955 | 0 | 0 / 0 | 0 / 0 |

The 109 additional native entries all have mandatory strength: 94 affirmative and 15 prohibitive sentences; five are explicitly declarative-only. The base 56 conditional definition-source entries and one descriptive keyword-extraction anomaly remain visible. The final patch replaces two build identities and adds no extra normative count. An initial factual adapt comparison measured 1,846/1,862, with 16 missing and normal/strict 0/1; it is retained as counterfactual evidence, not an operator choice or candidate implementation plan.

The 109 canonical collaboration Source declarations each have one explicit primary milestone/gate allocation, validated against native exact identities. Complete planning association is not implementation, a passing assertion, dependency parity, authorization or conformance. All readiness remains blocked. Original repository CI and the candidate's unchanged gate mechanism both still fail because `.duvet/snapshot.txt` is absent; no baseline has been created and no gate weakened.

## Remaining Review Scope

The native build direction and its reviewed shared activation are saved. No activation authorization remains pending. Tenancy, direct execution and build/adapt are settled; the separate previously denied four implementation-direction records remain untouched.

The collaboration declarations are now integrated and scoped-approved in the v1 implementation specification. Unspecified mechanisms and foundation declarations remain separate proposals. Before their corresponding slice, review product command/record schemas and runtime-exclusivity enforcement, project configuration and capacity assumptions, identity/role mapping, and the exact product-go-ahead/core-authority boundary. These details do not imply task leases, a new goal entity, a storage backend or a hosted execution model. None needs to be invented to measure or review activation.

Historical proposal preparation: [run summary and receipts](../../../../../borg-runtime/artifacts/planning/collaboration-platform-record-correction-2lrbv1gx/README.md), [original current/build/adapt measurements](../../../../../borg-runtime/artifacts/planning/collaboration-platform-preactivation-6uz6d3vu/measurement.json), [earlier candidate validation](../../../../../borg-runtime/artifacts/planning/collaboration-platform-record-correction-2lrbv1gx/final-validation.json), [earlier candidate source manifest](../../../../../borg-runtime/artifacts/actual-v1-qzphhmdk/collaboration-candidate-manifest.json).

The real 58-file snapshot and lock still target `540189023a5d5b1f5c6baab0d1bbd8145d05511ac2178bc42c6bb119b5954d04`. The current activated shared evaluation snapshot is `3a25c0f4df3ba736336cc30be3ca04b9ca17cf0052ce33c0a1cf426565644a74`; historical pre-activation snapshot `a8eee7fbd02441eac467aef756e98ff771d7362f7d4a43946b7965cd07d14c7d` and candidate `58bdb1554e9eaec533484986bcb9ddcfc198942167355bf4de368b45c2cea8ad` retain their original evidence. The real runtime lock is unchanged.

The final layout keeps authored implementation documents in `spec/impl/v1/` in Borg, and tooling/generated evidence in `borg-runtime`. [The specifications-only correction receipt](../../../../../borg-runtime/artifacts/specs-only-yc8gxkus/README.md) is historical path/source evidence.

[The implementation adoption receipt](../../../../../borg-runtime/artifacts/planning/collaboration-impl-adoption-amuc6fr8/README.md) preserves the earlier scoped integration and candidate reports. [The current activation receipt](../../../../../borg-runtime/artifacts/planning/collaboration-shared-activation-zidp_b4j/README.md) records the saved shared selection, exact identities, source snapshot, full 1955-entry planning report, deterministic views and preservation checks. Native association completeness is not behavior or promotion evidence; foundation approvals and execution readiness remain separate.
