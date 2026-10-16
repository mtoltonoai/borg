# Implementation v2 Review Draft

V2 is the current implementation restart after the v1 implementation was deleted. It is an implementation identifier, not a wire-format version. The v1 specification and shared contract remain unchanged. This directory states the intended implementation; selected build and workflow scope is recorded in the decision record and bound review metadata. Specification text and planning coverage do not establish execution readiness or shared conformance.

- [Implementation specification](implementation.md): the selected bounded product and its failure boundaries.
- [Execution plan](execution-plan.md): a runnable first path followed by independently deliverable increments.
- [Technology review](technology-review.md): every recorded v1 technology family, its previous scope and proposed v2 disposition.
- [Compatibility review](compatibility-review.md): shared requirements that these simplifications cannot currently satisfy.
- [Decision record](decisions.md): selected build/workflow scope, discussion provenance and remaining choices.
- [Four-host production target](production-target.md): selected workload and proposed qualification details for four matching devboxes.
- [Build packet](review-packet.md): four complete component work orders, frozen integration proposal and acceptance sequence.
- [Wire-interface proposal](protocol.md): task/attempt records, worker commands and recovery cases.
- [Dependency profile](dependency-profile.md): exact proposed versions/features and qualification limits.
- [Shared profile amendment](proposals/shared-profile-amendment.md): exact finite reconciliation of all original native identities and the unapplied registration patch.

The normative presentation states intended obligations directly. Review status belongs here and in the decision record. The [concrete v2 profile](decisions.md#concrete-v2-profile-accepted) is selected: local SQLite controller, Git checkpoints, local-stdio Codex integration, task collaboration with the [selected workflow expansion](decisions.md#collaboration-workflow-expansion-queued), explicit durable memory, ordinary host/repository/provider access controls and behavior-based validation. The [permission-engine deferral](decisions.md#fine-grained-permissions-deferred) and [browser interface](decisions.md#browser-collaboration-interface-required) are selected as well. Initial backup deferral and the [minimal security posture](decisions.md#minimal-security-posture-accepted) also remain selected. The [four-host target](decisions.md#four-host-target-accepted) is also selected. Exact dependency profiles and ancillary qualification budgets remain to review. No selection here amends shared outcomes, revives deleted implementation work or authorizes deployment.

The [worker-wide resource boundary](decisions.md#worker-wide-resource-group-selected) is selected: one cgroup v2 per worker with generous shared limits, normal startup and exhaustion/recovery qualification. Numeric limits, host setup and separate disk-quota work remain; selection is not implementation or enforcement evidence.

## Reference Source

The [initial model-budget deferral](decisions.md#initial-model-budget-work-deferred) selects the maximum supported default request allowance. Fine-grained spending/token and opaque supplied-executor request enforcement are deferred. Existing native accounting, explicit accepted limits, deadlines, output bounds and host/provider capacity remain applicable.

The comparison target is the immutable 59-file shared evaluation snapshot `ae8c0fca8fe823f5bb53585dc12bd54c20058072de70f8510810f70e5f6867b1`, selection digest `6ba36138d9eb02af757647311add23eab076553e52f8870a6c25812277aa580c`, retained in the [planning inputs](../../../../borg-runtime/planning-inputs/current/spec-snapshot/). It has 2,105 native entries. The unchanged historical runtime lock targets `540189023a5d5b1f5c6baab0d1bbd8145d05511ac2178bc42c6bb119b5954d04` and 58 copied files. Neither is silently repinned.

The existing shared contract remains authoritative. Differences in this draft need an explicit shared amendment or a recorded non-conformance disposition before a v2 conformance claim. The current v1 gate selection is not a v2 selection.

## Terms

- Agent definition: data configuring behavior and executor selection; no per-agent implementation is required.
- Agent identity: the durable identity that owns memory across sessions.
- Session: the conversation and state associated with one agent identity; bounded work does not impose a maximum session lifetime.
- Task: an assigned unit of work with durable input, status and result.
- Attempt: one execution of a task, with its own identity and ownership generation.
- Worker: the execution environment for a task, distinct from the platform control-plane trust boundary.
- Durable checkpoint: explicitly identified work stored outside the disposable worker and acknowledged by that storage.
- Native executor: the bounded model-request path owned by the implementation.
- Supplied executor: the external agent harness used for extended model/tool work.

## Planning Boundary

The execution plan contains five capabilities, six capability dependencies, 22 milestones and eight delivery views. Four charter milestones extend the existing eighteen; three focused governance gates extend the existing store milestone with actual-consumer closure in the existing joins. The declaration graph contains 57 typed prerequisite references, 45 distinct milestone edges, 27 provided interfaces and 56 gates. The existing first-release scope and these selected additions are recorded in bound review metadata; later-scale remains proposed. Empty declared ownership and readiness are separate from the operational queue's existing component owners. No gate completion or runtime conformance follows from authoring.

The [collaboration queue and validation packet](../../../../borg-runtime/artifacts/v2/collaboration-queue-native-mf17y4ic/README.md) records historical native planning coverage of 334/334 with normal/strict exits 0/0. All 265 earlier exact identities remain and 69 are added. Against unchanged allocations the expanded source reports 69 missing associations, with strict exit 1; the amended allocations close only those planning gaps. That earlier source target is the non-authoritative v2-owned evaluation snapshot `9bb974730a1beeccd8922a19d54ebce11de4362685b0bc357d1b3df1ad57309d`. The new collaboration behaviors are not yet qualified by implementation/test evidence. Older implementation reports remain historical evidence for their captured sources.

The four selected batches are task coordination, questions/decision queue, documents/reviews and resolution delivery/consumers. They can be authored in parallel behind the compiled common record contract. The final joined milestone requires the actual question/review/task producers and coding/native/browser consumers, including wait-before-quiescence ordering, atomic resolution/wait/delivery, actual safe-input consumption, duplicate/lost-ack recovery and correct recipient routing. Component fixtures do not discharge that integration.

The capability source snapshot evaluates v2-owned main-spec requirements only. It is separate from the immutable shared comparison target above and the unchanged actual runtime lock. Native planning associations for that owned scope are not a claim of satisfying the full shared contract. Review metadata binds the main, capabilities, phase declarations and the separately consumed protocol/dependency/profile review inputs.

The [reset receipt](../../../../borg-runtime/reset-receipts/2026-10-13/completion.json) records deletion and retained tooling validation. Historical v1 artifact links may now refer to intentionally deleted evidence. The specifications preserve their original provenance rather than presenting those results as current v2 proof.

## Current Charter And Governance Work

The [charter incorporation](proposals/charter-incorporation.md) and [focused governance acceptance](proposals/governance-foundation.md) record the selected source and implementation scope. They add 28 charter and 37 governance requirements while preserving the previous 334 identities. The current main-only evaluation snapshot is `042bb5f94d99cc10089850b0ceb7ecb146f07eb4ee2d371f39f30dbfe6b0ee27`; the [current validation and work queues](../../../../borg-runtime/artifacts/v2/v2-requirements-queues-7ql2e92o/README.md) report actual native results separately from runtime qualification. Full supplied-executor every-reply charter audit remains an explicit public-interface gap; native delivery, audit and compaction can proceed independently. Shared Specification As A Product remains deferred.

## Shared Retry Handling

The [selected retry behavior](decisions.md#shared-retry-handling-selected) adds 27 owned requirements to the prior 399 without retiring their identities. It covers platform-owned model, idempotent-tool and transient store retries; supplied-executor internal retries remain executor-owned. The [retry adoption receipt](../../../../borg-runtime/artifacts/v2/v2-shared-retry-adoption-lz8rk9pd/README.md) separates source/planning validation from actual implementation and provider qualification. The new main-only evaluation snapshot is `5dc96026b71b18687165376918375fd824f345c4a1d61c6928f49939181da2e8`; the real runtime lock/copied shared snapshot is unchanged.
