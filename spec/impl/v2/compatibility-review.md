# V2 Shared-Contract Compatibility Review

This descriptive initial impact ledger is neither an exemption list nor an amendment. Shared files, gate selection and v1 remain unchanged. A new implementation identifier does not override their obligations. Exact shared tuples below come from native Duvet, not a new requirement parser.

## Reference Identity

Shared snapshot: `ae8c0fca8fe823f5bb53585dc12bd54c20058072de70f8510810f70e5f6867b1`. Native report SHA256: `ed072802712c5d0ab7c6425877b26bed2a2dc28aa29b30167e9fadb183bd5a4f`. Report-local array positions are not persistent identities.

## Exact Requirement Impacts

### Content encryption

Source: [constitution.md / v-secure-by-default](../../../constitution.md#v-secure-by-default)

> Every durable record of a tenant's content MUST be encrypted at rest.

V2 disposition: Initial application content encryption is omitted; infrastructure encryption does not replace application key guarantees. See [the proposed clause context](implementation.md#storage-protection).

### Tenant-key destruction

Source: [constitution.md / the-tenant-is-the-boundary](../../../constitution.md#the-tenant-is-the-boundary)

> Destroying a tenant's root key MUST make the tenant's data unreadable everywhere, backups included.

V2 disposition: Ordinary deletion replaces the proposed initial cryptographic-erasure claim; this is a protected-guarantee difference. See [the proposed clause context](implementation.md#deletion-does-not-promise-cryptographic-erasure).

### Envelope and keys

Source: [spec/core/encryption-and-blobs.md / sessions-are-encrypted-under-an-envelope](../../core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope)

> Every session's values and blobs MUST be encrypted under the envelope the stored-form contract pins.

V2 disposition: Content-envelope/key requirements need an explicit disposition. Signing and credentials remain separate. See [the proposed clause context](implementation.md#storage-protection).

### Live key deletion

Source: [spec/core/encryption-and-blobs.md / deletion-is-key-destruction](../../core/encryption-and-blobs.md#deletion-is-key-destruction)

> Deletion MUST come from destroying keys.

V2 disposition: No cryptographic erasure; this is broader than v1 backup-key retention. See [the proposed clause context](implementation.md#deletion-does-not-promise-cryptographic-erasure).

### Production plaintext boundary

Source: [spec/core/encryption-and-blobs.md / sessions-are-encrypted-under-an-envelope](../../core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope)

> A stage whose stores do not apply the envelope MUST refuse a production tenant at start and on every call.

V2 disposition: The reduced profile is not currently a conforming production stage under this rule; no production activation is implied. See [the proposed clause context](implementation.md#storage-protection).

### All session state durable

Source: [constitution.md / p5-durable-state-disposable-memory](../../../constitution.md#p5-durable-state-disposable-memory)

> Every part of a session's state MUST be kept in the durable store.

V2 disposition: Only declared platform task state is durably owned; supplied-executor internal state has a separately disclosed recovery boundary. See [the proposed clause context](implementation.md#accepted-state-has-one-durable-owner).

### Disposable memory

Source: [constitution.md / p5-durable-state-disposable-memory](../../../constitution.md#p5-durable-state-disposable-memory)

> Everything a node holds in memory MUST be a cache that can be dropped and rebuilt from the durable store at any moment.

V2 disposition: A thread identifier does not preserve all executor state; unavailable inner state may require a new attempt. See [the proposed clause context](implementation.md#accepted-state-has-one-durable-owner).

### Session lifetime retained

Source: [spec/core/sessions-and-partitions.md / a-session-is-one-continuous-transcript](../../core/sessions-and-partitions.md#a-session-is-one-continuous-transcript)

> The platform MUST NOT bound a session's lifetime.

V2 disposition: Retain this rule: bounded tasks and model attempts do not impose a maximum session lifetime. See [the proposed clause context](implementation.md#the-native-executor-is-bounded).

### Snapshot after each change

Source: [spec/decisions/workspace-service/build.md / a-snapshot-follows-every-changing-call](../../decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call)

> The service MUST snapshot a workspace's contents, content-addressed, after every call that changed them.

V2 disposition: Explicit durable checkpoints replace automatic snapshots initially; intermediate changes can be lost. See [the proposed clause context](implementation.md#loss-returns-to-the-last-checkpoint).

### Workspace continuation

Source: [spec/decisions/workspace-service/build.md / a-snapshot-follows-every-changing-call](../../decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call)

> A session MUST be able to continue from its last snapshot on another host after its host is lost.

V2 disposition: A fresh checkout is not exact filesystem continuation; only declared checkpoint content survives. See [the proposed clause context](implementation.md#loss-returns-to-the-last-checkpoint).

### Workspace rollback

Source: [spec/decisions/workspace-service/build.md / swap-fork-and-rollback](../../decisions/workspace-service/build.md#swap-fork-and-rollback)

> A rollback MUST make an earlier snapshot the head as a new snapshot, so that a lineage only grows.

V2 disposition: Atomic workspace rollback is deferred; source-control operations do not claim it. See [the proposed clause context](implementation.md#loss-returns-to-the-last-checkpoint).

### Hostile process isolation

Source: [spec/decisions/workspace-service/build.md / a-workspace-is-isolated-and-bounded](../../decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded)

> A workspace's processes MUST see no other workspace's processes.

V2 disposition: The trusted-worker profile does not promise hostile-workload containment. Directories alone are insufficient. See [the proposed clause context](implementation.md#workers-are-trusted-disposable-environments).

### Filesystem confinement

Source: [spec/decisions/workspace-service/build.md / a-workspace-is-isolated-and-bounded](../../decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded)

> A tool process MUST see only the system and toolchain read-only, its own workspace read-write, and the paths declared visible at acquire.

V2 disposition: The stronger visibility guarantee needs an explicit change; no equivalent containment is claimed. See [the proposed clause context](implementation.md#workers-are-trusted-disposable-environments).

### Platform node retained

Source: [spec/core/durability-and-deployment.md / a-node-runs-only-the-platform-s-process](../../core/durability-and-deployment.md#a-node-runs-only-the-platforms-process)

> A platform node MUST run only the platform's own process.

V2 disposition: Host/process mapping remains unresolved; workers cannot silently occupy the platform-node trust domain. See [the proposed clause context](implementation.md#sensitive-authority-stays-outside-agent-writable-work).

### Platform command boundary retained

Source: [spec/core/durability-and-deployment.md / a-node-runs-only-the-platform-s-process](../../core/durability-and-deployment.md#a-node-runs-only-the-platforms-process)

> A platform node MUST accept no command and no interactive session from any principal.

V2 disposition: Separate worker trust domains or a reviewed amendment are needed; same physical host does not mean same trust domain. See [the proposed clause context](implementation.md#sensitive-authority-stays-outside-agent-writable-work).

### Whole-node loss

Source: [spec/decisions/durability/self-durable.md / the-store-survives-the-loss-of-every-node](../../decisions/durability/self-durable.md#the-store-survives-the-loss-of-every-node)

> The durable store MUST survive the loss of every node at once without losing committed state.

V2 disposition: Process restart on intact storage does not prove all-node or media-loss survival; unsupported guarantees are deferred. See [the proposed clause context](implementation.md#backups-are-outside-the-initial-milestone).

### Backup restore

Source: [spec/decisions/durability/self-durable.md / a-new-cluster-restores-from-the-store-s-backup](../../decisions/durability/self-durable.md#a-new-cluster-restores-from-the-stores-backup)

> A new cluster MUST restore every session from the durable store's own backup.

V2 disposition: Initial backups and restore guarantee are deferred. See [the proposed clause context](implementation.md#backups-are-outside-the-initial-milestone).

### Backup procedure

Source: [spec/decisions/durability/self-durable.md / a-new-cluster-restores-from-the-store-s-backup](../../decisions/durability/self-durable.md#a-new-cluster-restores-from-the-stores-backup)

> A procedure for backing up the durable store and restoring a cluster from the backup MUST be defined.

V2 disposition: Procedures and drills are later work, not hidden prerequisites for the first prototype. See [the proposed clause context](implementation.md#backups-are-outside-the-initial-milestone).

### Cluster-independent resources

Source: [spec/core/durability-and-deployment.md / stage-resources-outlive-every-cluster](../../core/durability-and-deployment.md#stage-resources-outlive-every-cluster)

> The blob store, the tenant root keys, the signing roots, the restore source, and the frontend's stable name MUST survive the replacement or loss of any cluster.

V2 disposition: No survival claim for resources whose only storage has been destroyed. See [the proposed clause context](implementation.md#backups-are-outside-the-initial-milestone).

### Internal per-block gating

Source: [spec/decisions/execution/direct.md / output-is-gated-per-block](../../decisions/execution/direct.md#output-is-gated-per-block)

> Model output MUST be gated per completed block.

V2 disposition: Task/result boundary checks do not establish arbitrary hooks inside the supplied executor. See [the proposed clause context](implementation.md#the-supplied-executor-owns-its-inner-loop).

## Hybrid Execution Is A New Choice

The [existing execution choice](../../decisions/execution.md#the-outcomes) selects direct or hosted execution. Per-definition executor selection is a new proposed v2 choice, not a silent adoption of either complete outcome. The [hosted contract](../../decisions/execution/hosted-harness.md) additionally requires quiescent working-state commits, controlled credentials/egress/dispatch, per-attempt metering and transcript/compaction reporting. A supported resume API alone does not establish those properties.

## Minimal Security Profile Impacts

The [selected v2 security posture](decisions.md#minimal-security-posture-accepted) removes key/secret services and custom per-call signing/credential issuance. SSH authenticates host access and can protect forwarded traffic; it does not establish the older application-level assertion, delegation, lease or key-publication contracts. The following additional exact native identities make that incompatibility visible. This list supplements the encryption and workspace impacts above; it is not an exhaustive replacement security contract.

### Call-Bound Assertions

Source: [spec/core/identity-delegation-and-secrets.md / services-take-identity-from-an-assertion](../../core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion)

> For each call or lease, the platform MUST present a short-lived signed assertion bound to that call, of the session's principal, chain, tenant, scopes, and lease generation.

V2 disposition: The host/SSH authentication boundary and existing repository/provider controls do not implement this signed per-call protocol.

### Brokered Dispatch Credentials

Source: [spec/core/identity-delegation-and-secrets.md / secrets-stay-out-of-the-model-and-the-records](../../core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records)

> At dispatch the core MUST obtain a short-lived credential from the broker for the principal, chain, and target.

V2 disposition: Existing locally configured provider credentials replace the initial broker dependency; no equivalent short-lived delegated credential is claimed.

### Single-Call Credential Leases

Source: [spec/core/identity-delegation-and-secrets.md / the-broker-is-its-own-trust-boundary](../../core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary)

> A credential MUST be leased for one call.

> A credential MUST NOT be reused for another call.

V2 disposition: Ordinary SSH and provider credential reuse does not satisfy these restrictions. Durable task ownership generations remain separate correctness checks.

### Independent Signing Administration

Source: [spec/core/identity-delegation-and-secrets.md / signing-roots-are-held-apart-from-the-cluster](../../core/identity-delegation-and-secrets.md#signing-roots-are-held-apart-from-the-cluster)

> A lineage's signing root MUST NOT be created or administered by the cluster's own deployment.

V2 disposition: The initial profile has no application assertion-signing root or associated custody service. Operator-managed SSH credentials are not a claim of this separate administration scheme.

### Cluster Credential Scope And Rotation

Source: [spec/core/shared-framework.md / node-transport-is-authenticated](../../core/shared-framework.md#node-transport-is-authenticated)

> The credential from which peer authentication is derived MUST be scoped to one cluster.

> The credential from which peer authentication is derived MUST be rotated within the configured interval.

V2 disposition: The selected SSH mechanism does not by itself establish either cluster scope or the configured rotation policy. No separate credential lifecycle service is selected to reproduce them.

### Application Assertion Signature

Source: [spec/contracts/signed-assertion.md / the-signature-is-asymmetric](../../contracts/signed-assertion.md#the-signature-is-asymmetric)

> An assertion MUST be signed with an asymmetric scheme, so that a verifier holding the verification key cannot issue an assertion.

V2 disposition: SSH's transport authentication does not create the application assertion described by this contract.

### Application Verification-Key Publication

Source: [spec/contracts/signed-assertion.md / keys-are-published-and-rotated-without-a-gap](../../contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap)

> The platform MUST publish the set of verification keys at a location every verifier can fetch.

V2 disposition: The initial profile does not publish an application assertion-verification key set or operate a key service. Existing SSH host verification remains a different mechanism.

## Remaining Compatibility Work

### Focused Simulation And Changed Coverage Scope

The accepted v2 validation package uses focused owned-state-machine simulation, real integration/recovery tests and measured coverage. It is not evidence that every component and external dependency meets the unchanged shared simulation requirements.

Source: [constitution.md / iv-deterministic-by-construction](../../../constitution.md#iv-deterministic-by-construction)

> Every component MUST run under the deterministic simulation framework with every external dependency replaced by a simulator.

> Every external dependency MUST have a simulator that injects the failures the real dependency exhibits.

> A simulator MUST be verified by a contract test that runs the same expectations against the simulator and against the real dependency.

V2 disposition: Explicitly scope the inherited simulation obligations before a shared conformance claim. Focused testing of the new controller and adapter does not by itself qualify the supplied executor's internals or all dependencies.

The following are v1-owned requirements, not entries in the shared 2,105-item denominator. They remain unchanged in v1 and are explicitly not inherited by v2's selected behavior-based release criteria.

Source: [spec/impl/v1/implementation.md / phased-delivery-and-coverage](../v1/implementation.md#phased-delivery-and-coverage)

> Final coverage MUST reach 100% of executable lines in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

> Final coverage MUST reach 100% of instrumented branches in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

V2 disposition: Replace both blanket numeric gates with mandatory behavior/recovery acceptance, focused Bach/Bolero, measured coverage and review of uncovered critical paths. This is an expressly selected v2 difference, not a retrospective v1 exception or a claim that shared conformance has been satisfied.

Resolve all affected stored-form, signed-assertion, tool-call, usage-record, failure-handling and workspace clauses before publishing a conformance profile. This initial ledger is not exhaustive coverage of their transitive effects. Preserve real authentication, configured external access restrictions, current ownership checks and unknown-effect handling at the platform boundary, and identify inner-executor guarantees that are unavailable. Retain version/migration discipline where deployed consumers exist; deleting private implementation files says nothing about external deployed data.

V1 planning still has 2,105 native associations against its own target. This draft neither removes entries from that denominator nor claims v2 satisfies them. V2-owned requirements, inherited requirements, proposed amendments and deferred features need their own resolved scope before complete planning/conformance metrics are meaningful.

## Fine-Grained Permission Deferral

The selected initial v2 profile has no per-agent/task grant graph, role engine, delegated capability token system or programmable permission checks. Host/SSH authentication and configured repository/provider controls remain; task ownership generations and recovery checks remain. Tasks sharing a trusted worker identity share its actual accessible resources. This does not claim the unchanged shared delegation, grant/revocation or per-action authorization contracts are satisfied. Exact shared replacement/unsupported bindings remain required before profile activation. The permission deferral does not apply the shared registration patch or widen access on any host or service.

## Initial Model Budget Deferral

The selected v2 policy now uses the maximum supported default request allowance and defers fine-grained spending/token and opaque supplied-executor inner-request enforcement. Existing native debits, configured child reservations, accepted explicit limits and deadlines remain. This does not satisfy or remove shared budget-policy obligations by implication.

The existing shared-profile binding proposal retains its explicit historical 162-requirement owned target and original review evidence. The later native-tool additions and this budget-policy change need a successor compatibility reconciliation before shared-profile activation; retired owned quotations in that historical ledger are not current allocations or proof. Current v2 capability allocations and native reporting use the revised main specification separately. No shared requirement, active selection or runtime source pin is changed here.

## Agent Identity, Sessions, And Development Reset

The selected agent/session correction aligns the implementation with the shared glossary's Definition and Agent identity and with core/identity-delegation-and-secrets.md sections A Definition Has Its Own Principal and An Agent Identity Outlives Its Sessions. The shared memory requirements in core/sessions-and-partitions.md section A Memory Belongs To An Agent Identity bind memory to the durable agent rather than an instantiation. The former v2 agent-wide execution exclusion is retired; session-level execution ownership permits concurrent sessions sharing that agent's memory. Access scope, current mutation authority and memory revision conflict checks remain.

The operator selected resetting prototype development state instead of backward compatibility. No prototype identity aliases, memory rekeying or old database/journal migration are required. This is the explicit migration disposition for that development transition; it does not remove exact replay and restart guarantees inside the new format, authorize implicit reset on failures, alter shared normative files or widen the remaining v2 deferrals. Repositories, source commits and credentials are retained. The reset is pending the verified new build and stopped-process procedure.

## Repository Catalogue Compatibility

The preceding source profile was wire7/schema11/worker8, not the older proposal's wire6/schema10. The selected successor wire8/schema12/worker9 adds immutable repository associations and catalogue state; cache identity2 and publication phase2 preserve those associations through local recovery. The startup read map is not a durable registration source and is not implicitly imported. No current database, worker root, receipt, repository, credential or checkpoint is rewritten by source integration.

Unsupported existing state remains fail-closed and unchanged. A stopped-development transition into explicitly new stores, or a separately reviewed exact migration, remains a later operator action; source integration performs no reset. The previous development-reset decision applies to its recorded transition and is not used as blanket authorization to discard later accepted work. Within the successor format, registration changes affect new admission while accepted tasks and configured-child associations retain their exact immutable revision. This preserves the existing currentness, replay and uncertain-effect obligations. Catalogue metadata adds no permission graph, capability token, grant, acquisition mechanism or shared-contract amendment, and does not resolve unrelated deferred shared-profile obligations.

## Focused Governance Incorporation

The approved V2 addition implements selected obligations from shared governance's Stop Admission, Governance Changes Are Ordinary Operations, Shadow/Caching/Episodes and Trust Root sections. Its V2 requirements, allocations and actual acceptance are separately attributable. It adds no shared outcome activation and does not repin the immutable shared reference or runtime lock.

Existing task result checks, explicit waits, immutable limit snapshots and stop commands are partial prerequisites. They do not yet establish governed voluntary completion, versioned decision audit, shadow counterfactual/promotion or policy-independent expiring override. The new V2 gates remain unqualified until actual implementation and tests run. The configuration extension preserves the closed operation and accepted snapshots; it does not claim full shared live configuration inheritance or a generic governance pipeline.

Host-authenticated operator provenance remains distinct from directly authenticated people. Shared emergency principal/quorum/bootstrap and broad scope guarantees are not silently supplied by the selected single-host trust mechanism. Protected release still requires its real artifact/target-bound human decision and mandatory checks. Arbitrary engines, model judges, elaborate caches, automatic learning, full hook coverage and unrelated previously recorded compatibility differences remain outside this finite incorporation.

## Platform-Owned Retry Adoption

The exact shared failure-handling source and complete sentence inventory are retained in the preparation artifact; shared files and active shared gate selection are unchanged. Selected retry work covers model-attempt identity, finite retry budgets/deadlines, backoff/jitter/Retry-After, coordinated per-model/endpoint circuit and authentication handling, invalid/context/policy/quota dispositions, invalid-output feedback and truncated-output continuation. Existing external-effect, task identity, tool receipt, cancellation and accounting invariants remain. The supplied executor's internal retries are excluded from platform reimplementation, not exempted from truthful task recovery.

Current implementation evidence is partial: native POSTs are bounded and durably charged; governed tool mutations use original-ID receipt lookup; ambiguous model responses remain held. The observed complete 429 was conservatively classified unknown and no positive native retry path existed. New typed rejection, diagnostic capture, retry timer/accounting and circuit coordination need qualification. Ambiguous provider response retrieval/deduplication is unavailable in the current nonstored adapter, so no supported outcome means an explicit held/blocked result, not invented replay safety. Cluster-wide authentication, invalid-output repair, output-cap continuation and circuit crash recovery are separate acceptance cases until executed.

Applicable platform-owned store conflicts/outages and idempotent-tool unreachable/timeout handling are also adopted, with exact original receipt/rollback/currentness boundaries. Unknown side-effecting tool results are never blindly resent. General notification deciders, partition movement, event quarantine/re-drive and tenant-role assumption remain deferred or not applicable to the selected interface. The source review's exact shared tuples record those dispositions; no model-only implementation closes them. A bounded breaker cooldown can permit a new independent probe while keeping a prior unknown effect unresolved; it cannot authorize replay of that old effect.

## Supplied Executor Charter Exception

The [Codex charter exception](proposals/codex-charter-exception.md) is the selected narrow V2 task/public outer-turn replacement for inaccessible supplied-executor replies and internal update boundaries. Native model-turn delivery, citations, adoption and compaction remain required. Supplied delivery retains exact original source/context identity; supported final results retain genuine declarations and passive audit. A host delivery binding does not supply model citations. An unqualified running update stays pending or unsupported without waking, restarting or replaying work.

Seven shared reply/audit selectors receive explicit task-boundary replacement mappings; exact source resolution and the non-compliance meaning of reference checks are preserved. The common-record consumer distinguishes supplied public results from native model occurrences. No blanket shared conformance, wire/schema migration, gate completion or general security/recovery exception follows. Existing original operation, currentness, cancellation, checkpoint and uncertainty rules remain unchanged. Final-output and durable-result consumer work plus actual pinned-executor qualification remain separate from this source proposal.
