# Implementation Specification

> **V1 IMPLEMENTATION SPECIFICATION.** This specification applies to implementation v1. The shared constitution, contracts, core requirements and selected outcomes take precedence except for explicitly stated v1 scope deviations, which do not establish conformance to the corresponding shared requirements. The constitution defines the meaning of the capitalized requirement keywords used here. Each keyword sentence states a binding v1 requirement or permission under a stable heading, with the strength defined by the constitution.
>
> The values in Implementation Decisions specify the implementation choices. The declaration contract defines the fields of capabilities and milestones, including their delivery, acceptance and gate criteria. Shared-source quotations retain their original identities and applicability and are not new v1 requirements.
>
> A requirement applies to its subject: collaboration boundaries govern product behavior; development, approval, traceability, schema and generation requirements govern the change process or its artifacts. The shared governing-requirement rules continue to apply. Fixed choices are not evidence of implementation, and process compliance is not runtime conformance. Paragraphs explicitly labeled informative explain these requirements without adding obligations.

## Implementation Decisions

### Tenancy Scope

One-tenant with dedicated hosts and stores.

### Execution

Direct execution.

### Runtime Language

Borg runtime components, excluding tooling and external dependencies, MUST be written in Rust.

Binding scope: this includes Borg-owned runtime orchestration and service adapters. Code used as part of the deployed runtime is not exempt merely because it is called a script or adapter. The tooling exception covers planning, build, validation and qualification tooling. External dependencies retain their existing approval, inventory and simulator/real-probe obligations.

### Model API

The selected default model for API calls is `gpt-6.1-sol`, through the OpenAI Responses API.

### Simultaneous Production Scale

V1 MUST support at least 10 hosts operating simultaneously.

Each host MUST support at least 1,000 agents operating concurrently.

Each host MUST support at least 10 workspaces operating concurrently.

Scale acceptance MUST demonstrate the required agents and workspaces operating concurrently on the same deployment.

Binding scope: the target is 10,000 agents and 100 workspaces across the same ten hosts, with both per-host targets satisfied during the same test interval. An agent is the existing agent identity, whose sessions perform work; agent, session and model-request counts are distinct. Dormant definitions, reserved workspace records, separate agent-only and workspace-only runs, or additional uncounted workspace hosts do not demonstrate the combined target. The v1 workspace capacity target supersedes the conditional shared default of four workspaces per host for this implementation.

The production architecture MUST be designed and implemented to satisfy the simultaneous scale target.

Implementation delivery MUST NOT introduce an intermediate architecture that cannot satisfy the simultaneous scale target.

The phased execution plan MUST include scale qualification for the simultaneous scale target.

Informative: smaller-scale tests and controlled dependency fixtures can qualify portions of the intended production architecture. They are not substitute implementations or evidence of full target capacity. The hardware and workload profile, performance and duration criteria, and host/process mapping remain explicit design and qualification work. The capacity target does not itself select a storage engine, dependency, isolation mechanism or deployment, and does not imply that full capacity survives losing a host. Existing authority, isolation, node-process, durability and effect guarantees remain applicable.

### Runtime Self-Delivery

A working v1 cluster MUST be capable of delivering changes to the Borg runtime through a governed runtime-change workflow.

V1 workspaces MUST support editing Borg runtime source code.

V1 workspaces MUST support compiling Borg runtime source code.

The runtime-change workflow MUST support agents submitting runtime source changes through governed repository interfaces.

The runtime-change workflow MUST automatically execute the applicable runtime validation.

The runtime-change workflow MUST prepare a deployable runtime artifact from the source change.

Borg's runtime-change preparation MUST NOT require an intermediate per-change human approval step.

The final rollout of a runtime change MUST require human approval.

Human rollout approval MUST NOT bypass applicable mandatory validation gates.

Binding scope: agents can edit, compile, test, submit changes and prepare releases. Automated preparation includes obtaining trusted release evidence. Workspace-produced test results are development feedback; they do not replace the checker's own evidence from a protected runner outside the agent-writable workspace. Final human approval is a deployment-control-plane decision bound to the exact artifact/change and rollout target/action. It is not a runtime decision-hook deferral to a person, and it gives agents no capability to approve their own changes or alter the trusted gate. A changed artifact or target requires its own matching decision.

This workflow preserves [trusted evidence provenance](../../contracts/evidence-record.md#evidence-is-the-checkers-own-record), [artifact-bound results](../../contracts/evidence-record.md#the-record-identifies-its-subject), [the checkers-only runtime boundary](../../decisions/authority-widening/checkers-only.md#a-deferral-to-a-person-blocks-instead), [positive decisions bound to actions](../../core/governance.md#agents-produce-evidence-and-never-approve), and the existing [deployment and compatibility rules](../../core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type). Failed or unknown required checks do not become successful because a human approves a rollout.

Informative: this per-change approval policy governs Borg deploying its own runtime updates. It does not replace the separate requirement for Matthew to approve new externally supplied technologies used in this implementation work. The workspace outcome is selected separately below; this workflow selects no repository provider, trusted-runner product or deployment substrate, and does not authorize an actual rollout while defining the capability. The required workspace and trusted delivery producers remain explicit implementation work; scripts run outside Borg alone do not demonstrate cluster self-delivery.

### Workspace Service

The v1 workspace-service outcome is build, as defined by [the native workspace service](../../decisions/workspace-service/build.md).

V1 MUST implement the applicable requirements of the native workspace-service outcome.

Binding scope: the full existing outcome applies, including session-held leases, fenced snapshot lineages, recovery, credentials, isolation, host health and promises, workspace-free tools, stored values and observation. The two permissions remain optional. Generic durability, blob, event, encryption, identity and call-guard mechanisms are consumed from the shared framework rather than independently reimplemented. Workspace leases belong to the service and remain distinct from session lease generations. Agent/session state is accessed through the platform API; workspace records are product data, not another agent-state authority.

The simultaneous production target and runtime self-delivery requirements continue to apply, including ten workspaces per counted host concurrently with the required agent load. This v1 target supersedes the conditional shared default of four workspaces per host.

Informative: this outcome choice selects no storage engine, sandbox, container, virtual machine, host substrate, repository provider, trusted-runner product, resource profile or deployment control plane. It grants no new access and authorizes no rollout. Existing third-party approval requirements remain. The host/process mapping and detailed WS delivery declarations require their own review; adopting this outcome does not blanket-approve existing foundation declarations.

### Key Service

The v1 deployment MUST provide a key service as an integrated Borg component or as an accompanying deployed service.

OpenBao 2.7.1 is selected for private key-service qualification. The selected release bundle, built-in components and synthetic fixture configuration are recorded in [the technology inventory](technology-inventory.md#openbao-qualification-bundle). Its separate use in the protected session record target is defined below; neither selection establishes deployed custody or irreversible key destruction across backups.

The shared [key hierarchy](../../contracts/stored-form.md#the-key-hierarchy), [root-key administration separation](../../core/tenancy-and-confidentiality.md#root-key-administration-is-separated), and [key-destruction requirements](../../core/encryption-and-blobs.md#deletion-is-key-destruction) govern the service subject to the following v1 backup-retention exception.

### Backup Key Retention

V1 backups and point-in-time recovery copies MAY retain key material after the corresponding live key is deleted.

Binding scope: deleting keys from these retained recovery copies is not required for v1. This exception covers owner/session keys and tenant root keys. It does not waive deletion of live keys, tenant isolation, trusted key selection, key-administration/use separation, or the existing decrypt-cache and host-plaintext destruction bounds. Live blob, inbox and episode copies remain subject to key deletion; their inclusion in a backup does not exempt the live copies.

Informative: restoring an older backup may restore a deleted key and make its data readable again. V1 therefore does not claim the original backup-erasure guarantee in [The Tenant Is The Boundary](../../../constitution.md#the-tenant-is-the-boundary), [A Resource Carries Its Own Wrapped Keys](../../contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys), or [Deletion Is Key Destruction](../../core/encryption-and-blobs.md#deletion-is-key-destruction). Those shared requirements and their exact traceability references remain unchanged; a v1 acceptance result is not evidence that their backup-erasure clauses passed.

Informative: a future key-service replacement, such as a managed key service, can be considered if irreversible erasure from backups becomes necessary. No replacement product, integration or migration is selected here.

### Immutable Byte Storage

The selected production target is a Borg-owned immutable-byte service using PostgreSQL16.15 for both encrypted bytes and metadata. The exact existing distribution and native adapter inputs are recorded in the technology inventory; this target adds no database version or package.

The immutable-byte service MUST commit each bounded encrypted byte unit and its associated publication metadata in the same durable transaction.

The immutable-byte service MUST expose a usable reference only after that transaction's committed outcome is established.

Recovery from an unknown publication outcome MUST settle the original byte-and-metadata operation before retry or exposure.

Binding scope: this selects the co-located transactional medium, with no separate byte-durability acknowledgment or node-local-file fallback. Existing immutable addressing, owner/pin fencing, integrity, key authorization, reclamation and retention requirements remain. Mutable session state remains session state; a digest does not convert it into an immutable blob. Large named values use the bounded segment/index representation of the selected profile. This selection establishes neither actual surviving media nor backup/restore, simultaneous-scale capacity or deployment qualification.

### Protected Session Record Target

The selected target is C2-service-held: family `borg.p1.sealed`, format revision2, profile1. The exact [descriptor](/home/dev-user/borg-runtime/integration/session-store/artifacts/p1-protected-descriptor-y2a3sl2l/proposal-03/descriptor.json) has SHA256 `430e04637ff13cd8c47a2272d7f1630e86820e532ca3867a74f197d37e514a2e`. Its encoding, key derivations, typed bindings, lookups, bounds and body-schema evolution contract are the selected representation. The descriptor's preparation-time approval/status fields are historical metadata, not a different current choice.

Protected session record readers and writers MUST implement the selected C2-service-held representation.

Each independently deletable session incarnation MUST have a distinct, never-reused resource-owner identity.

The resource-owner mapping MUST be atomically committed from the genuine creation inputs before it can authorize key issuance.

State and control records MUST use distinct resource and payload-key identities.

A new session incarnation MUST NOT inherit its predecessor's resource-owner identity.

The protected-record implementation MUST use the exact selected normal/build technology and feature profile.

Binding scope: the owner allocator obtains fresh20-byte opaque identities from the actual configured entropy/identity source. Owner generation1 exists only in a committed fresh mapping, not as an absence default. Partition assignment and agent/session execution ownership do not define key ownership. Service-held tenant root, epoch branch and separate owner-removal protection use the descriptor's exact double-wrap, resolver, derivation, authenticated-data and attempt-accounting domains. Creating a new operation or current lease does not reset per-key accounting. Live deletion and bounded plaintext/cache destruction remain required; Backup Key Retention above permits old recovery copies to restore deleted keys and readability.

The selected technology profile is [the exact normal/build map](/home/dev-user/borg-runtime/integration/session-store/artifacts/p1-production-path-6sloafpa/proposal-02/proposed-production-normal-features.json), SHA256 `74e5742840d4bcd5f1798ba487269653df96c82a74ccc193c9b366924feaf75b`, containing73 external packages. Root defaults are disabled: minicbor2.3.0 without extra features; aes-gcm0.11.1 with aes/zeroize; aes-siv0.8.0 with zeroize; hkdf0.13.0 without extra features; sha2 0.11.0 with zeroize; zeroize1.9.1 with alloc; Tokio1.53.2 with rt/net/time/sync/io-util; tokio-postgres0.7.18 with runtime. The existing OpenBao2.7.1 bundle/SBOM supplies Transit derived aes256-gcm96. Transitive features are those in the pinned map, not all lock or development packages. This target selects no custody endpoint, credentials, host, replication layout or deployment.

Protected record parsing and writing MUST enforce the selected encoded-size and structural bounds before the corresponding allocation or work.

Every scan, hash, cryptographic input, copy, retained allocation, comparison, database decode and waited reread MUST charge its work to the shared caller budget before that work occurs.

Binding scope: complete state/control frame122880bytes, body106496bytes, metadata16384bytes, scalar point131072bytes, encoded named value67108864bytes, complete blob segment8388608bytes and segment body8372224bytes. The tagged flat index has at most16 segment entries, body16384bytes and complete frame32768bytes. Header6144bytes, key header8192bytes, outer extensions1024bytes and capsule4096bytes bound the framing. All other field, item, depth, fragment and attempt bounds remain exactly those in the pinned descriptor, including mutable per-key attempt cap1048576 and cache lifetime upper bound300seconds. These are encoded-space or named operation bounds, not free work allowances, aggregate capacity proof or whole-workspace snapshot limits. Opaque identities remain exact bytes, without lossy text conversion or numeric reinterpretation.

Unsupported protected record revisions, binding kinds or body schemas MUST refuse interpretation without rewriting the retained bytes.

A protected record rewrite MUST preserve the exact authenticated spans of supported informational extensions.

Binding scope: strict definite tagged CBOR uses minimal integer/length encodings, sorted unique field tags and nested tagged evolving components. Unknown critical fields or unsupported variant meanings cannot be converted into informational passthrough. Existing private1 retains its own reader, key domain and original bytes; C2 is neither a relabel nor an automatic upgrade. The descriptor's lifecycle schema currently covers only its listed kinds; newer dispatch variants require a complete compatible body-schema successor before use. No alias, dropped field or fabricated authority fills an unsupported kind.

Every participating protected write MUST update the session's current publication reference and revision with the protected original in the same transaction.

Immutable typed-origin lookup MUST select the exact original prepared record for the originating operation.

A partition-only reassignment MUST leave the protected publication locator unchanged.

Original replay MUST refuse a missing or corrupt protected association without inserting a replacement or resealing content.

Protected publication acknowledgment MUST require the actual outer-transaction commit witness.

Binding scope: locator revision1 requires genuine creation; subsequent participating writes use checked previous-plus-one revision. A locator or digest alone is not current authority. Remote key resolution and separately acknowledged preparation finish before session locks; actual same-transaction prepared readback, attempt accounting and final source/currentness checks still precede outer commit. Losing creation, failed commit and unknown acknowledgment do not release a key/seal or publication permit. The node's one bounded ResolveOwner operation can involve two actual backend decrypt operations; it is not evidence that a shared one-service-call requirement has been discharged.

Compatible protected-record readers MUST be installed before C2 writers expose records to them.

Every participating predecessor writer MUST be frozen or upgraded before C2 exposure.

Binding scope: reader inventories, retained-original recovery and refusal tests establish compatibility; previous private tests are not new C2 qualification. Workspace lineage/history deletion, live-fork retention, compression, whole-workspace bounds, production custody deployment and full-scale survival remain separate work. Neither this target nor the immutable-byte selection introduces a new agent-state authority or waives current governance, fencing, authentication, durability or isolation.

### Receipt Close Fencing

An admitted close of a call in flight MUST end the call.

A service MUST refuse a close's requested lifecycle mutation when its lease generation is older than the highest the service has recorded for that session.

A service that refuses a close's requested lifecycle mutation because its lease generation is stale, and holds a receipt for the call, MUST return the recorded state with a distinct superseded-owner refusal.

A close refused by the generation fence MUST leave the receipt unchanged.

A close refused by the generation fence MUST leave the generation fence unchanged.

A close refused by the generation fence MUST NOT cancel or otherwise change the running effect.

Binding scope: authentication and call binding still precede receipt access. A terminal receipt remains replayable across generations; a stale close with no receipt creates neither a receipt nor a fence change. Reconcile remains unfenced. These v1 semantics specialize the two shared close clauses identified in the [exact compatibility mapping](decisions.md#five-decision-compatibility-disposition); they do not establish conformance to those unchanged clauses or select a response encoding.

### Emergency Override Restoration

An emergency override for a policy without a pending emergency-override restoration MUST record the effective policy it replaces as its restoration version.

An emergency override for a policy with a pending emergency-override restoration MUST inherit that restoration's recorded version.

At expiry of the active emergency override, its recorded restoration version MUST be restored with an event.

A superseded emergency override MUST NOT change the effective policy when its expiry is delivered.

A successfully committed non-emergency policy adoption through the governance-policy tier MUST end every pending emergency-override restoration for the same tenant and policy.

A policy adoption at any other tier MUST NOT end a pending emergency-override restoration for that policy.

An emergency-override audit event MUST identify the recorded restoration version.

Binding scope: a pending restoration includes an expired override whose restoration has not committed; retained history alone is not pending. Successive overrides of an ordinary policy retain that original ordinary restoration version. A fully governed ordinary adoption cancels the earlier restoration obligations; a later override records that newly adopted ordinary policy. Existing direct authentication, quorum, confirmation-window, expiry and protected-check requirements remain. This selects no renewal cap. The v1 restoration rule specializes the shared expiry clause identified in the compatibility mapping.

### Session Admission After Reassignment

Partition reassignment MUST immediately fence the old partition owner.

A session's lease generation MUST advance, in the same commit as the change, on every restore, pause, quarantine, stop, end, and resume.

After a partition owner move, the new owner MUST commit each session's admission before executing or recovering that session.

A session's admission MUST advance its lease generation under the current partition assignment.

Partition reassignment MUST NOT require updating every session's state.

The work of admitting one session MUST be bounded independently of the partition's session count.

A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation at sealing, and its owner identity.

A session's current control authority MUST be recoverable independently of its content envelope.

A session's current control authority MUST authenticate its association with the tenant, session incarnation, partition assignment, lease generation, lifecycle restrictions, and held content.

A session MUST NOT be admitted from an unauthenticated or unrecoverable lease high-water.

A quarantined session's held content MUST NOT be overwritten or substituted.

A control transition of a quarantined session MUST preserve its association with the held content.

An executor MUST accept a dispatch handoff only when its committed dispatch record agrees with the current partition assignment and session control authority.

Acceptance of an executor handoff MUST be serialized with changes to that authority.

Binding scope: current control authority is the authenticated authority to act for a session; held content is the original content envelope retained even when unreadable. Neither term specifies a separate storage primitive or physical record layout. The resource owner in the sealed header is not a replacement for the current partition assignment. Existing header/body authenticated-data binding remains. A read followed by an unprotected send is not serialized executor acceptance. Work accepted before revocation retains its existing receipt and outcome handling; new recovery requires the new owner's admission. The three changed shared tuples and compatibility obligations are identified in the compatibility mapping. No control-loss repair is selected. The separate Protected Session Record Target below selects the representation without waiving these authority and held-content obligations.

### Ordinary Input During A Budget Wait

A successfully accepted ordinary notification input MUST become visible through the existing governed state and transcript read interfaces even when subsequent model-request budget admission denies.

The accepted input transition MUST commit its effective content, input cursor, due valid configuration progress, original hook observations, and durable pending-model-work obligation atomically under the current fence.

The durable pending-model-work obligation MUST preserve the original input and audit association after inbox trimming or restart.

A session whose model request is budget-denied MUST retain its pending model work while idle.

Budget denial MUST NOT start a model call.

Budget denial MUST NOT create a model-start acknowledgment.

Budget denial MUST NOT create an active-attempt association.

Budget denial MUST NOT create an admitted-model event.

Budget denial MUST NOT create a provider request.

A budget-change wake MUST obtain fresh guarded model admission from current ownership, configuration, authority, and budget facts before starting pending model work.

A budget-change wake MUST NOT reapply an already committed notification input.

A still-denied budget wake MUST NOT increment the turn count.

A still-denied budget wake MUST NOT increment the model-request count.

An unknown input-wait commit outcome MUST be settled for the original durable operation identity under current valid ownership and fencing before redo or sleep.

Binding scope: the atomic obligation specifies the guarded durable effects of the accepted input step, not a SQL transaction shape or physical grouping. It does not prevent independent boundary-only valid configuration progress; already committed configuration is not reapplied by that step. Notification-arrival Block still applies the event as blocked and advances its cursor without exposing the blocked body to the model. Valid due configuration can progress at a boundary without starting a model turn. A timer fire held by the timer wake cap remains in the inbox with its durable reset deadline; a run containing an ordinary non-timer event retains the existing timer-cap exemption. Fresh admitted model work still requires the actual canonical start/count commit and acknowledged settlement before scheduler or transport handoff. In-memory handles or a wake hint do not establish durable pending work or admission. This budget-wait behavior selects no budget quantity, cost model, refill rule, record encoding or storage layout; the separate protected-record target below retains its own scope.

### Subscription History

A session's initial subscription MUST begin delivery eligibility at the actual join point.

A session's rejoin MUST begin new delivery eligibility at the actual rejoin point.

Unsubscribing MUST NOT retract work to which the session was already entitled.

Rejoining MUST preserve earlier entitled-but-undelivered work.

A session's rejoin MUST NOT automatically replay work published during its unsubscribed gap.

New assignments MUST be published after their required subscription is effective.

Binding scope: entitlement is determined by membership at publication, not by delayed fan-out or delivery time. The actual authenticated join cut follows the existing publication/membership ordering, not a wall-clock guess. Preserving prior entitlement does not bypass current security revocation, retention expiry or explicitly recorded drops; expired or lost work uses the existing unrecoverable-range/drop handling rather than invented delivery. Denied membership changes do not change eligibility. Rejoin opens a new eligible interval without clearing earlier entitled debt or filling the unsubscribed interval. Existing viewer no-history and connection-lifetime rules remain distinct.

This policy selects no routing rule, numeric cursor representation, stored encoding, backend, retention duration or automatic historical-start API. Explicit authorized history replay is not selected. Assignment integration still uses the platform API and existing event/lifecycle recovery contracts, with no duplicate product inbox, premature assignment-caused eligibility or removal of independently required subscriptions. A publication-before-subscription gap is not repaired by assuming that this policy authorizes history replay.

### Compatibility Of Selected Behavior

A non-additive activation of the receipt-close or session-admission contract MUST carry an explicit contract revision increment.

A non-additive activation of the receipt-close or session-admission contract MUST carry a stated migration path.

Binding scope: implementation identity v1 is not a wire or stored-format revision. The selected behavior above is normative for v1; actual protocol/format discrimination, reader and client inventories, migration qualification and cutover remain required before incompatible consumers or durable data are exposed to it. Existing unknown-format refusal, no fallback rewrite, fencing and retained-data protections remain. The shared source and runtime target remain distinct; old source citations are not evidence for replacement meanings. The separate decision record preserves exact source mappings, compatibility disposition and approval provenance.

### Collaboration Platform

Selected shared outcome: [build](../../decisions/collaboration-platform/build.md). The shared collaboration default is none; v1 selects the native product to provide collaboration records and interfaces over the existing Borg framework. This is an implementation-specific choice.

The v1 implementation MUST provide the native collaboration platform specified by COLLAB and its seven milestone declarations.

The collaboration platform MUST reuse the existing shared-framework durability, events, change feeds, live streams, authentication, call guards, and encryption mechanisms for its product records.

The collaboration platform MUST access agent session state and operations only through the platform API.

The product's gapless per-tenant revision MUST order its own committed record changes.

Binding scope: the product revision orders product records, not agent session cursors, partition fences or platform event sequences. Tasks, documents, questions, reviews and revisions are domain data over existing primitives. This specification selects no new storage engine, independent agent-state store, task claim lease or goal entity. The tenancy and execution selections above apply unchanged.

Core emergency controls MUST remain independent of collaboration-product availability.

A non-emergency pause requested through the collaboration platform MUST use the agent's lifecycle intent.

A collaboration go-ahead MUST NOT widen agent authority.

Collaboration actions MUST remain subject to current platform policy and grants.

Product questions and go-aheads MUST NOT replace the platform's decision-hook protocol.

Product questions and go-aheads MUST NOT replace the checkers-only outcome.

The collaboration platform's runtime-exclusivity mechanism MUST NOT prohibit legitimate concurrent sessions.

Binding scope: runtime exclusivity does not establish a task lease. Session management is imperative; the five declarative-only collaboration clauses remain conditional-inactive.

Informative: the seven collaboration milestones contain the acceptance for assignment/publication/subscription recovery, repeated stop decisions, assignment/stop races, shared-queue deadline callbacks and bounded atomic retirement/resolver operations. The schemas, role mappings, exclusivity mechanisms, capacities and representations not fixed by those criteria remain unresolved.

Approval of the implementation-specific collaboration declarations MUST NOT be treated as activation of the shared collaboration outcome.

Informative: the active shared outcome is identified by the shared decision record and gate selection.

### Development Conformance Checks

Duvet MUST be run during development as implementation progresses.

Progress updates MUST report the latest actual Duvet metrics.

The existing obligations remain those in [Building From The Specification](../../../AGENTS.md#building-from-the-specification), [Coverage Requires Implementation And Test](../../core/simulation-and-conformance.md#coverage-requires-implementation-and-test), [Behavior Is Witnessed By Execution](../../core/simulation-and-conformance.md#behavior-is-witnessed-by-execution), [The Gate Decides Promotion](../../core/simulation-and-conformance.md#the-gate-decides-promotion), and [The Evidence Record](../../contracts/evidence-record.md).

Duvet results MUST NOT replace those gates.

### DAG-Driven Parallel Development

An implementation dependency DAG MUST be derived from the original Borg requirements and the current code.

The implementation dependency DAG MUST be maintained.

Each graph node MUST be associated with its original source requirement identities: file, stable section, and exact sentence.

Dependent work MUST honor its prerequisites before it starts.

Dependent work MUST honor its shared interfaces before it starts.

Dependent work MUST honor its invariants before it starts.

Ready independent branches MUST be executed in parallel with isolated ownership and file boundaries.

Coordination MUST remain in this existing BorgBuilder thread.

Shared files MUST NOT have simultaneous writers.

Integration MUST use the required behavior tests.

Integration MUST use actual Duvet checks.

Integration MUST use the original applicable conformance gates.

Integration MUST use the original applicable evidence gates.

### Single-Source Top-Level Capabilities

The authoritative implementation specification set MUST be the authored source of the top-level capabilities and their explicit prerequisites.

Top-level capability declarations reside in the linked [Implementation Capability Specification](implementation-capabilities.md), which is part of this authoritative set.

Each capability entry MUST have a stable ID.

Each capability entry MUST have an explicit prerequisite list.

Capability declarations MUST be written consistently enough for deterministic extraction without guessing.

JSON MUST be generated from those declarations.

Dependency order MUST be generated from those declarations.

Diagrams MUST be generated from those declarations.

Generated outputs MUST NOT be another authored manifest.

Duvet MUST be used for original requirement extraction.

Duvet MUST be used for original requirement traceability.

Duvet MUST be used for original requirement coverage.

Duvet MUST NOT be used to derive semantic dependencies.

The graph MUST remain at the capability level.

The graph MUST remain at the top level without nested subgraphs.

The graph MUST NOT introduce a duplicate requirement inventory.

Implementation status MUST remain outside structural capability declarations.

Worker assignments MUST remain outside structural capability declarations.

DAG validation MUST check output reproducibility.

DAG validation MUST check for duplicate IDs.

DAG validation MUST check for missing IDs.

DAG validation MUST check for unknown prerequisites.

DAG validation MUST check for cycles.

DAG validation MUST check original-source references.

Unmapped requirements MUST be reported explicitly.

### Externally Supplied Technologies

Every externally supplied technology used to build, test, or run v1 MUST be recorded in this implementation specification.

Every externally supplied technology used to build, test, or run v1 MUST have Matthew's direct approval before use.

The selected technology uses are:

| Technology | Selected version and use |
|---|---|
| Rust, Cargo and standard library | Rust1.99.0 b940084d7 and Cargo1.99.0 5f94df478 for owned libraries and development drivers. |
| Python and standard library | System Python3.12.3 for Duvet Cap and validation/qualification harnesses. |
| LLVM coverage tools | Matching LLVM23.1.1 tools for the stable compiler; isolated nightly2026-10-11, Rust1.101.0-nightly 6f5da406b and its matching LLVM tools for branch instrumentation. |
| Bach | Version0.1.2, revision8bddd1958ff66460ea7d0e060bf07b999262aadb, for the simulation roles below. |
| Bolero | Version0.13.4, revision71e5de0eceb043f3eda2f62ccd02d2be2b398c44, including generator0.13.5 and the pinned test dependency closure, for the generated-testing roles below. |
| Duvet | Version0.4.3 for the native extraction, traceability and coverage roles below. |
| PostgreSQL and libpq | PostgreSQL16.15 Ubuntu16.15-0ubuntu0.24.04.1 is the production target for transactional encrypted bytes and metadata in the owned immutable-byte service. The existing private transaction, restart, parity and integration qualification and libpq160015 scope remain. |
| Tokio and tokio-postgres | Tokio1.53.2 with rt/net/time/sync/io-util and tokio-postgres0.7.18 with runtime, default features disabled, for private native-runtime qualification. The exact 83-package lock and consumption boundary are in the technology inventory. |
| Native inference HTTP/TLS and JSON | Reqwest0.13.5 with rustls/http2 and serde_json1.0.151 with std/raw_value, default features disabled, using the existing Tokio1.53.2 profile. The exact 162-package bundle, AWS-LC TLS provider, installed native build tools and consumption boundary are in the technology inventory. Selected for private acquisition/build and local synthetic qualification. |
| Assertion signing and local verification | Existing aws-lc-rs1.18.1/aws-lc-sys0.45.0 profile and OpenBao2.7.1 built-in Transit for private synthetic qualification with disposable non-derived plain Ed25519 keys. The verifier uses only public keys and exact message/signature bytes. Exact inputs and consumption boundaries are in the technology inventory. |
| minicbor | Version2.3.0 with default features disabled for borrowed-codec qualification and the exact C2 protected-record target. |
| Envelope cryptographic libraries | aes-gcm0.11.1, aes-siv0.8.0, hkdf0.13.0, sha2 0.11.0 and zeroize1.9.1 with the pinned features and 28-identity metadata inventory, for private synthetic envelope qualification and integration. Actual resolution and acquired bytes are checked before execution. The exact C2 normal/build profile below also selects their protected-record use. |
| Workspace sandbox inspection | gVisor release20261005.0, revision97e8e896904cb364319cd9758241d3f0bd27a6f4, official x86_64 bundle, for download and static inventory only. No sandbox execution or host configuration is selected. |
| Native inbound HTTP qualification | Hyper1.11.1 server/HTTP1/HTTP2 and hyper-util0.1.21 server/server-auto/server-graceful/tokio, plus httpdate1.0.3 and the existing native transport inputs, for private loopback-only qualification. |
| Decider engine qualification | Wasmi2.0.0 with defaults disabled and stable/std/validate/portable-dispatch/extra-checks, for bounded private qualification on x86_64-unknown-linux-gnu. The technology inventory identifies the exact eight checksum-pinned active archives and thirteen additional archives restricted to static inspection and offline metadata. |
| OpenBao | Version2.7.1 release bundle for private key-service qualification with synthetic keys, isolated Raft snapshots, built-in Transit/token/policy/audit, Shamir seal and aes256-gcm96 fixture keys. Exact artifact and dependency-SBOM identities are in the technology inventory. The separate C2 target selects the same bundle with derived Transit aes256-gcm96 for service-held protection; no endpoint or deployment is selected. |

Binding scope: technology use is limited to each recorded purpose. The immutable-byte and protected-record sections select their production implementation targets; other qualification-only uses remain qualification-only. These selections do not establish production qualification, deployment, topology, capacity or additional access. Nightly remains isolated to coverage instrumentation; optional fuzzing engines are not selected. The existing backup-key retention exception remains unchanged.

The [technology inventory](technology-inventory.md) records the consumption sites, pinned dependency identities and real-versus-simulated boundaries. Its bytes are bound as a separate review input; it adds no normative behavior or declaration approval.

Binding scope: the [native-runtime qualification bundle](technology-inventory.md#native-runtime-qualification-bundle) is selected for synthetic testing of one Rust application's store sequencing and recovery with the existing private PostgreSQL fixture. That qualification bundle alone selects no production runtime, threading topology, store layout, network transport, durable format or cryptographic profile. The separate C2 target selects its exact native adapter profile without selecting a global runtime or deployment topology. The owned composition remains subject to Bach testing and real-adapter contract qualification; the supplied runtime and client internals are not claimed to run under Bach.

Binding scope: the [native inference transport bundle](technology-inventory.md#native-inference-transport-qualification-bundle) qualifies actual HTTP/1 and HTTP/2, bounded streaming/JSON, cancellation, retry and secret handling with synthetic local peers. Provider credentials remain in the inference-service adapter, outside Borg's core. The selected JSON version has a known floating-comparison defect; its qualification excludes that comparison path from bounds validation and exercises the actual checked numeric paths. This selects no production transport/capacity policy, Borg content cipher or durable codec, and includes no real provider call. Actual acquisition, build features and simulator/real parity have their own evidence; library selection does not establish those results.

Binding scope: the [assertion-signature qualification bundle](technology-inventory.md#assertion-signature-qualification-bundle) selects isolated synthetic signing and public-key-only local verification. Its disposable fixture uses one raw 32-byte public-key representation, one 64-byte signature representation and exact bounded message bytes without prehashing. This does not select a production assertion scheme, token encoding, publication adapter, custody design or deployment. Primitive interoperability does not establish authenticated caller, current grant, fence or effect-admission behavior.

Binding scope: the [envelope qualification bundle](technology-inventory.md#envelope-qualification-bundle) selects the exact cryptographic inputs for the private synthetic experiment and its recorded integration sites. The separate C2 target below selects its exact algorithms, hierarchy and durable profile. Other production uses and actual custody deployment remain distinct. The existing backup-key retention exception is unchanged.

Binding scope: [workspace sandbox inspection](technology-inventory.md#workspace-sandbox-static-inventory) permits acquisition and static inventory of the pinned distribution. It permits no execution of runsc, bundled sidecars, the containerd shim or a sandbox workload, and no privilege, daemon, network or persistent host-configuration change.

Binding scope: the [inbound service qualification bundle](technology-inventory.md#native-inbound-service-qualification-bundle) selects the recorded server feature extension and httpdate package for local synthetic acquisition, build and loopback tests. It selects no production listener, routing or authentication format, malformed-framing exception, deployment or provider call.

### Decider Engine Qualification

Wasmi 2.0.0 is selected for bounded private qualification, not as a production-qualified engine or complete sandbox implementation.

Decider-engine acquisition MUST remain within the eight active archives and thirteen metadata-only archives identified by exact checksums in the [decider qualification inventory](technology-inventory.md#decider-engine-qualification-bundle).

The thirteen metadata-only archives MUST be used only for acquisition, static inspection and offline dependency metadata.

The thirteen metadata-only archives MUST NOT be compiled or executed under this selection.

The decider-engine active normal/build dependency graph MUST remain the original eight-package graph with its exact selected feature and build inputs.

Decider-engine build and execution MUST wait until packaged inputs match that selected profile.

An additional decider-engine archive, feature, or build input MUST have Matthew's direct approval before use.

Binding scope: the profile includes the declared Wasmi, wasmi_ir, wasmparser and libm build scripts with the selected compiler, and excludes the thirteen metadata-only identities from compilation and execution. Offline metadata is evidence about the resolved graph, not authorization to build an additional active dependency. A newly active package or profile mismatch stops dependent execution without expanding this selection. The wasmi_ir generator is limited to the exact build.rs and fourteen bundled helpers identified in the technology inventory; it generates four internal Rust files and adds no package or feature. Qualification uses bounded integer-only private programs, strict minimal imports, no start function or ambient filesystem/network/clock/entropy, fresh evaluation state, bounded input/output copying, engine fuel/stack/memory controls and actual supervisor deadline/reaping. It includes nonconstant input-dependent decisions, transformations and Block reasons, malformed inputs, exhaustion, cross-evaluation isolation and simulator/real-engine parity with a deliberately divergent simulator discriminator. Fuel is not a wall-clock or total-memory bound. No guest compiler, public ABI, WASI, WAT, SIMD, component-model runtime, provider call, host grant or deployment is selected. A stateless first experiment does not remove the full session state-slot requirement.

Informative: the intended consumption point is the sandboxed-program decider behind the governance pipeline's decider interface. An engine verdict cannot replace protected platform checks, current authority or atomic decision/effect association. Current adapter, durable state-slot integration and production ABI status are recorded separately from this selection.

### Simulation And Generated Testing

Bach is the selected simulation-testing framework.

Bolero is selected for generated test inputs, property testing, and fuzzing.

Original source: [Properties Are Generated And Checked Against A Model](../../core/simulation-and-conformance.md#properties-are-generated-and-checked-against-a-model).

### Component-Level Simulation

Every component developed for Borg MUST be individually testable under Bach simulation.

Every off-the-shelf component whose real implementation is not itself simulation-tested MUST be explicitly listed in this implementation specification at every level where it is consumed.

Informative: simulation of a substitute or interface does not establish simulation testing of the real third-party implementation.

Informative: listing a component does not approve an exception.

Each substantive exception entry MUST have Matthew's direct approval.

Original sources: [Core Principle IV: Deterministic By Construction](../../../constitution.md#iv-deterministic-by-construction), [Everything Runs Under Simulation](../../core/simulation-and-conformance.md#everything-runs-under-simulation), [Every Dependency Has A Verified Simulator](../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator), and [Declared Defaults: Simulation](../../record.md#simulation).

### Phased Delivery And Coverage

A phased implementation specification MUST be defined.

Phase references MUST use stable capability IDs.

The linked [Phased Implementation Specification](implementation-phases.md) is the authoritative definition of the seven-stage structure within this implementation specification set. Its stages are milestone views, not implicit global barriers.

Implementation MUST proceed in phases that prioritize a working end-to-end durable system before completing coverage of isolated components.

The staged implementation MUST establish working end-to-end behavior for every applicable component.

Durable integration MUST demonstrate actual persistence.

Durable integration MUST demonstrate process-restart recovery.

Durable integration MUST demonstrate representative cross-component flows.

Informative: a first durable milestone is not completion of every component; an in-memory simulator does not establish durability.

Required behavior coverage MUST broaden as implementation progresses.

Required fault coverage MUST broaden as implementation progresses.

Required recovery coverage MUST broaden as implementation progresses.

Each phase MUST require essential correctness evidence.

Each phase MUST require essential safety evidence.

Each phase MUST report measured line coverage with remaining gaps.

Each phase MUST report measured branch coverage with remaining gaps.

Each phase MUST report actual Duvet results with remaining gaps.

No intermediate numeric coverage threshold is established here.

Final coverage MUST reach 100% of executable lines in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

Final coverage MUST reach 100% of instrumented branches in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

Informative: original applicable requirements, real-dependency probe parity, and conformance and promotion gates still apply. Passing a development phase does not establish full conformance or authorize promotion.

Original sources: [P5: Durable State, Disposable Memory](../../../constitution.md#p5-durable-state-disposable-memory), [Durable Store Durability](../../core/shared-framework.md#durable-store-durability), [Self-Durable Store](../../decisions/durability/self-durable.md), [Simulation And Conformance](../../core/simulation-and-conformance.md), and [The Evidence Record](../../contracts/evidence-record.md).

### Duvet Artifact Traceability

Duvet MUST be used to link all artifacts back to the specification.

This includes the capability and dependency-DAG planning artifact.

Artifact traceability MUST use the same authored requirement references.

Native Duvet citation outputs MAY be generated from those references.

Generated citation outputs MUST NOT become another authored source.

Planning associations MUST be kept in a separate Duvet report.

Planning citations and report results MUST NOT count as runtime implementation evidence.

Planning citations and report results MUST NOT count as behavior-test evidence.

Planning citations and report results MUST NOT count as execution evidence.

Custom DAG tooling MUST be limited to capability and milestone declarations, explicit prerequisites, schema and cycle validation, phase readiness and gate reporting, and deterministic graph or execution-plan views and generated adapters.

A second hand-authored requirement inventory MUST NOT be maintained.

DAG and planning tooling MUST consume Duvet's native JSON requirement and citation graph and native coverage.

DAG and planning tooling MUST NOT re-extract the normative requirement catalog.

Original requirement identities MUST be resolved against each pinned Duvet report.

Stable capability IDs MUST be resolved against each pinned Duvet report.

Legacy annotation array indices MUST NOT be used as persistent identities.

Source hashes MUST be checked for integrity.

Exact original requirement references MUST be checked for integrity.

The selected interface is that supported by installed Duvet 0.4.3.

### Duvet Cap

Duvet Cap is the logical name of the existing capability and dependency layer over Duvet.

Capability DAG validation MUST use Duvet Cap.

Capability DAG generation MUST use Duvet Cap.

Duvet Cap MUST read capability declarations and explicit prerequisites from the authoritative implementation specification set.

Duvet Cap MUST consume Duvet's native requirement and citation graph and coverage.

Duvet Cap MUST validate exact requirement references.

Duvet Cap MUST validate the declaration schema.

Duvet Cap MUST validate dependency cycles.

Duvet Cap MUST deterministically generate dependency order.

Duvet Cap MUST deterministically generate JSON.

Duvet Cap MUST deterministically generate diagrams.

Duvet Cap MUST preserve stable capability IDs.

Duvet Cap MUST preserve original requirement identities: file, stable section, and exact sentence.

Duvet Cap MUST NOT use report-local annotation indices as persistent identities.

Duvet Cap MUST NOT re-extract the normative requirement catalog.

Duvet Cap MUST NOT introduce a second authored requirement inventory.

Informative: the planning/runtime evidence separation above applies to Duvet Cap. Tool use does not approve a declaration or dependency.

### Duvet Cap Phase Execution Plans

Duvet Cap MUST generate phase-specific execution plans from the linked authoritative implementation, phased, and capability specifications.

Duvet Cap MUST use the structured stage and milestone declarations defined below.

Duvet Cap MUST determine milestone readiness from recorded approvals and explicitly required interface readiness.

A milestone that integrates a capability MUST identify the required interface conditions for that capability's prerequisites.

An undeclared prerequisite condition MUST remain a readiness blocker.

An interface-definition-only milestone MUST identify its limited scope explicitly.

Duvet Cap MUST evaluate milestone completion separately against its declared acceptance and evidence gates.

Duvet Cap MUST generate deterministic dependency order.

Duvet Cap MUST generate deterministic parallel-ready sets with non-overlapping declared ownership boundaries.

Duvet Cap MUST report missing declarations explicitly.

Duvet Cap MUST report missing approvals explicitly.

Duvet Cap MUST report missing prerequisite readiness explicitly.

Duvet Cap MUST report missing gate evidence explicitly.

Duvet Cap MUST keep progress outside authoritative declarations.

Duvet Cap MUST keep worker assignments outside authoritative declarations.

Generated execution-plan views MUST NOT approve declarations.

Generated execution-plan views MUST NOT authorize runtime execution.

Informative: generated projections identify their source and evaluation scope. Reported interface/gate receipts are planning inputs, not proof of behavior or trusted conformance.

### Duvet Cap Declaration Contract

Duvet Cap declarations MUST conform to the field definitions in this section.

A milestone's implementation MUST satisfy its declared acceptance criterion.

A milestone's completion evidence MUST satisfy each declared gate expectation.

**Binding schema definitions.** These definitions specify declaration syntax and field meaning.

The main specification's links labeled "Implementation Capability Specification" and "Phased Implementation Specification" identify the two authoritative documents. Other links, including proposal links, are not imports. Each declaration has one authored definition. Source hashes cover all three authoritative documents.

Stage metadata is authored in one fenced `duvet-cap-stages` TOML block in the phased specification. Its `stages` array uses `id`, positive unique `order`, `heading` referring to an existing stage heading. Stage order controls presentation, not implicit prerequisite edges.

Milestones are authored in one fenced `duvet-cap-milestones` TOML block in the phased specification. An empty `milestones` array declares no milestones. Each entry uses:

| Field | Meaning |
|---|---|
| `id`, `capability`, `stage` | Stable milestone ID and references to declared capability and stage IDs. |
| `title`, `acceptance` | The authored deliverable label and observable acceptance criterion. Acceptance specifies the delivery obligation; parsing the field does not establish that the criterion is satisfied. |
| `scope` | `integration` by default; explicit `interface` describes interface definition only and leaves consumer integration prerequisites deferred. |
| `provides` | Interface IDs this milestone can make available. |
| `requires` | Explicit `producer-milestone:interface` references; cross-stage references are allowed if acyclic. |
| `owns` | Canonical relative file/directory paths, without glob patterns, defining declared ownership boundaries. Missing boundaries block readiness. |
| `gates` | Entries with stable local `id` and an `expectation`. Expectations define completion evidence; a gate is not a prerequisite for starting its own milestone. |

IDs use uppercase letters, digits and underscores, starting with a letter. Capability declarations identify deliverables, acceptance criteria, prerequisites, dependency rationales and exact shared-source references.

An optional progress overlay uses JSON fields `schema_version` (1), `spec_set_digest`, and `milestones`. Each milestone can report `interfaces`, `gates`, and an `assignment`. Interface receipts use `status` (`ready`, `not_ready`, `unknown`) and an `evidence` reference. Gate receipts use `status` (`pass`, `fail`, `unknown`) and an `evidence` reference. A positive status without an evidence reference is insufficient. The planner records these references without certifying their contents.

Informative: source-order invariance applies to semantic dependency/readiness results; provenance hashes change when authored bytes change. Parsing or passing tooling tests does not adopt a substantive milestone, acceptance criterion, dependency or architecture.

Informative: readiness uses the recorded capability, stage and milestone approvals, nonempty ownership boundaries and required producer-interface receipts. A consumer needs the declared producer release, not completion of every unrelated producer gate. Completion separately uses the milestone's declared gate receipts. The planner validates schema and evidence-reference presence; it does not evaluate acceptance prose or certify the contents of evidence.
