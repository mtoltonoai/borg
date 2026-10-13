# V1 Decision Record

This descriptive record states the implementation's selections, changes and rationale. Normative behavior is defined by the main, capability and phase specifications. Per-declaration adoption status is recorded in implementation-review.json. This record is not a fourth normative source and does not grant execution authorization.

## Fixed Selections

The implementation uses one-tenant dedicated hosts/stores, direct execution, Bach, Bolero and the specified Duvet interface. These choices remain defined in the main specification. Exception authority also remains in the main specification and is not delegated by this record.

## Coordination Scope

Coordination and file ownership are development-process boundaries, not platform runtime behavior. Independent work uses disjoint file boundaries; shared interfaces and specification integration have one coordinating owner. A declaration or generated plan does not establish an assignment or authorize execution.

## Seven-Stage Delivery

The seven-stage sequence establishes testable interfaces, proves real-store atomicity and restart recovery early, integrates durable event/session processing and the direct model/tool harness, completes remaining capabilities, qualifies integrated fault/recovery/isolation and real dependencies, then closes coverage and conformance.

This order gives early persistence and integration feedback without delaying tests until final qualification. Evidence accumulates throughout. Stages are milestone views rather than global barriers; independent work proceeds when its required decisions, interfaces and invariants are ready.

## Collaboration Platform

V1 selects the native build outcome. The shared specification retains reusable build/adapt definitions and a default of none; selecting build for v1 does not select it for every implementation.

The product provides collaboration records and interfaces over existing durability, events, identity and encryption mechanisms. Agent session state and operations remain accessible only through the platform API. This reuses common primitives without a new storage engine or independent agent-state subsystem. No task claim, task lease or goal entity is introduced.

The implementation scope is COLLAB, six capability dependencies, seven delivery milestones, 109 shared-source allocations and their qualification dependencies. COLLAB and its seven milestones retain their approved status; the 25 foundation capabilities and 93 foundation milestones remain proposed in the separate review metadata. The seven-stage structure is approved. Intended normative wording does not alter these statuses.

The imperative session-management and checkers-only outcomes remain selected. The five declarative collaboration clauses remain conditional-inactive. Product go-aheads do not widen platform authority or replace decision hooks. Runtime exclusivity is not a task lease and does not prohibit legitimate concurrent sessions.

### Non-Emergency Pause Boundary

Product lifecycle intent is the path for non-emergency collaboration pauses. Existing core emergency controls remain independent of product availability and lifecycle ownership, subject to their existing grants, safe-point timing and lifecycle precedence. Limiting the product pause clauses to non-emergency requests prevents the product from obstructing that existing core behavior; it adds no emergency power.

### Retired Pause Identities

The pause section heading is unchanged. The two sentence replacements narrow the collaboration-specific path; evidence for a retired sentence is not automatically evidence for its replacement.

| Shared file and section | Retired sentence | Replacement sentence |
|---|---|---|
| `spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path` | An agent's run, paused, or retired intent MUST be the only way to pause its sessions. | An agent's run, paused, or retired intent MUST be the collaboration platform's mechanism for requesting a non-emergency pause of its sessions. |
| `spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path` | A pause requested through any path MUST take effect by setting that intent. | A non-emergency pause requested through the collaboration platform MUST take effect by setting that intent. |

## Normative Presentation Cleanup

Independent obligations use separate sentences under stable headings. Shared Source quotations are references rather than newly authored v1 obligations; this prevents reference extraction from duplicating requirements. The main specification distinguishes product behavior, fixed selections, informative explanation and development/change-process constraints.

## Review-Ready Specification Presentation

The normative documents describe the complete intended behavior. Separately hash-bound review metadata records actual adoption status so the whole specification can be reviewed without silently adopting foundation declarations. Missing, stale or inconsistent metadata is rejected; valid hashes establish input integrity rather than independent proof of authorization.

Provider and self-durable store choices remain unapproved. Their milestone readiness still requires approved choices. The coalesced event-hook contract requires interface review; acknowledgement, capacity, retention and compatibility details remain unspecified. Existing drafts concerning engine/codec reuse, constituent-preserving event delivery and publication-time entitlement retain their separate status; this record does not change shared contracts or make those remaining decisions.

## Key Service Provision

On 2026-10-12, Matthew stated that no existing key service is available, directed the implementation to deploy its own service or integrate it with Borg, and explicitly requested that this decision be recorded in the implementation specification. V1 therefore provides the service as part of its deployment; it does not require the operator to supply an existing service.

This decision selects responsibility and integration scope. It does not select a service product, key-custody mechanism, envelope format, algorithm, hosting topology or access grant. The shared key-administration/use separation, trusted key selection, owner/root destruction and backup constraints remain unchanged. It changes no capability or milestone adoption status and provides no execution or conformance evidence.

## Model API And External Technology Approval

On 2026-10-12, Matthew selected Sol 6.1 for model API calls. The implementation uses its exact API identifier, gpt-6.1-sol, with the Responses API required for its tool calls. The earlier Luna qualification remains historical evidence, not the current selection. This records the model choice without adopting a production hosting or credential arrangement.

Matthew also required every consumed technology not built by Borg to be recorded in the implementation specification and directly approved by him. The main specification records that rule. The existing Bach, Bolero, Duvet and model selections remain; experiment results, availability, and this inventory exercise do not approve additional technologies, particular dependency versions or production adoption. Pending technology choices are collected separately for review before their selections are added to the specification.

### Development And Qualification Stack

On 2026-10-12, Matthew selected "Approve this development/qualification batch" for the concrete technology-approval-batch proposal: Rust/Cargo1.99, system Python3.12.3, isolated LLVM/nightly coverage tools, the pinned Bach/Bolero test dependency closure, PostgreSQL16.15/libpq for private tests and minicbor2.3.0 for codec experiments. The selected versions and restricted uses are recorded in the main specification, with the full observed inventory in technology-inventory.md. This resolves the temporary PostgreSQL/minicbor qualification hold. The separately stated production store/format, key-service product and optional-engine exclusions remain unchanged; no capability/milestone status or readiness follows from this technology approval.

### OpenBao Qualification Choice

On 2026-10-12, after the exact OpenBao 2.7.1 qualification packet and the requested alternative/custody comparison, Matthew stated: "in terms of key service, openbao sounds good I think unless we want to build that too". The recommendation is to reuse OpenBao's existing key-management functionality and own Borg's integration rather than build a key-management server. This records acceptance of the bounded qualification candidate described in proposals/storage-services-qualification.md, including its exact release bundle, packaged dependencies and synthetic fixture. It does not adopt a production custody design or treat recoverable snapshots as irreversible erasure.

The qualification is local and disposable, with synthetic keys/tokens and isolated recovery snapshots. Built-in Raft, Transit, token/policy/audit, Shamir seal and aes256-gcm96 are fixture-only choices. Actual acquisition and linked dependencies are recorded before execution. No external plugin, HSM/KMS account, existing credential or access grant is selected. Production owner erasure, deploy/admin/use separation and plaintext-cache destruction remain original acceptance gates. The separate object-store alternatives and owned blob-service layout remain proposals.

## V1 Backup Key Retention

On 2026-10-12, Matthew stated: "In the future, we can swap OpenBao out with KMS or something if we need this capability. You can update the impl spec that deleting keys from backups is not required". This explicitly narrows v1 acceptance: retained backups and point-in-time recovery copies may retain deleted owner/session keys and tenant root keys. It supersedes the earlier backup-erasure acceptance described above for v1, while preserving live deletion, tenant/key authority boundaries and cache/plaintext destruction bounds.

The canonical requirement and its scope are in implementation.md#backup-key-retention. The SF capability and SF_S2, SF_S5 and SF_S6 acceptance text distinguish v1 scope from the unchanged original all-copy erasure requirements. Their Source quotations, allocations and dependencies remain intact. This decision changes neither capability/milestone adoption nor shared gate selection, and does not grant original conformance or promotion.

The OpenBao K1/K2 results remain evidence that recoverable snapshots restore deleted material. They are not rewritten as passes of the original requirements. OpenBao remains selected for private qualification; no KMS product, production custody arrangement, replacement interface or migration is selected. The follow-up custody proposal is deferred rather than an outstanding managed-versus-self-hosted decision for this backup-erasure capability.

## Native Runtime Qualification

On 2026-10-12, Matthew answered "Yes" to the explicit native Rust runtime
qualification batch in proposals/native-runtime-qualification.md: Tokio1.53.2
with rt/net/time/sync/io-util and tokio-postgres0.7.18 with runtime, default
features disabled, plus the exact 83-package lock SHA256
06e270111b7bdb78a654fce69dbf173b6577c3941906081c25557a666527431b.
The request compared this maintained Rust client/runtime with an owned Rust FFI
adapter over existing libpq. The selected scope is acquisition, feature inspection
and private synthetic qualification at qualification/native-runtime; it is
recorded in the main specification and technology inventory.

This approval does not select the production runtime, store mapping, network
transport, codec, encryption or deployment. It does not approve the separate
RustCrypto envelope request or stale-close contract clarification. Capability and
milestone adoption, ownership and readiness are unchanged. The metadata-resolution
packet remains historical evidence of preparation, not a rewritten execution or
approval receipt.

## Simultaneous Scale And Production-Directed Delivery

On 2026-10-12, Matthew required operation across ten hosts, each supporting one
thousand agents and ten workspaces, and clarified that these loads are
simultaneous. He required phased scale testing in the execution plan and direct
construction of the production architecture, excluding intermediate architectures
that cannot meet that scale. He then explicitly directed recording these changes
in the implementation specification.

The main specification records the combined per-host target and the phased
specification records eight scale/allocation gates across the existing seven
stages. This does not turn configured identities or synthetic workspace handles
into capacity evidence. The v1 target is ten workspaces per host rather than the
conditional shared default of four. Workspace delivery/outcome and host/process
mapping remain unresolved implementation work; the target is not deferred or
weakened by that gap.

No hardware class, latency/throughput objective, test duration, model-provider
capacity or failure-capacity target was selected by these instructions. Existing
capability/milestone adoption and actual readiness stay separate. The native
runtime qualification approval remains in effect, but its candidate and all
reused components now need a demonstrated path to the simultaneous production
target; a passing non-scaling fixture cannot serve as its implementation.

## Runtime Self-Delivery And Final Rollout

On 2026-10-12, Matthew required a working Borg cluster to have the capability to
deliver changes to the Borg runtime, explicitly including workspaces that edit
and compile code. He clarified that only the final rollout should need human
approval. When asked whether this replaced his separate approval rule for new
third-party technology, he answered: "No. I was referring to the Borg runtime
deploying its own changes. It should only require approval at the final stage."

The main specification therefore requires automatic runtime-change preparation
and final human rollout approval, preserving agent editing, testing, source
submission and release preparation. Applicable trusted checks remain automatic
and mandatory. Workspace-produced evidence cannot substitute for the protected
checker's own record; the human decision is bound to the exact artifact and
rollout action in the deployment control plane, not a runtime hook deferral.
The separate technology-approval rule, checkers-only outcome and agents' lack of
self-approval authority remain unchanged.

Four delivery gates are added to existing G_S1, SR_S5, G_S6 and G_S7 milestones.
No workspace product, source-control provider, trusted-runner substrate, actual
deployment, new grant or infrastructure provisioning is selected by this record.
The newly scheduled model-scheduler and record-rollout consumers use only the
already approved development stack; their registration changes no dependency
technology or capability/milestone adoption.

## Native Workspace Service

On 2026-10-12, Matthew answered "Agree" to the reviewed native workspace-service
BUILD selection, including its exact shared activation patch and all 150 existing
requirements. The v1 selection is recorded in the main implementation specification
and the checked-in shared evaluation now includes BUILD. Existing runners remain
unselected, while the shared default for other implementations remains none.

This applies the complete outcome with its original guards and two optional
permissions. Generic durability, blobs, events, identity, encryption and call guards
remain shared mechanisms. The v1 target of ten simultaneous workspaces per counted
host overrides the conditional shared default of four.

No sandbox product, host/process topology, substrate, provider, resource profile,
new grant or actual deployment was selected. Detailed workspace declarations and
their dependency allocations remain reviewable planning work; this outcome choice
does not approve unrelated foundation declarations. The actual historical runtime
snapshot/lock is not retargeted by changing the current evaluation selection.

## Runtime Language

On 2026-10-12 Matthew required all Borg runtime components to be written in Rust, explicitly excepting tooling and external dependencies. The scope includes owned runtime orchestration and adapters; existing Python planning, build and qualification tools are not adopted as a production runtime path. This language choice does not waive third-party approval or dependency qualification, select a new dependency, or establish that the current integration demonstrations are already an all-Rust runtime.

## Workspace Planning Decomposition

The native workspace BUILD outcome was selected separately. The source-linked WS capability and six delivery increments complete the planning allocation for its 150 unchanged requirements. Their detailed decomposition, new readiness interfaces and qualification links remain proposed in the separate review metadata; this integration confers no additional declaration adoption or readiness. Existing foundation and collaboration review states are unchanged.

The independently reviewed proposal and its exact allocation, graph and native traceability evidence are frozen in [the workspace planning packet](/home/dev-user/borg-runtime/artifacts/planning/workspace-capability-mapping-1_9brgk1/). Canonical capability and phase documents now own these declarations; the packet's copies are historical review inputs, not another maintained declaration source. Host/process/isolation, durable format/envelope, resource profile and concrete producer handoffs remain explicit decisions before their affected work. The workspace-free tool's minimal-state clause versus inherited receiver receipt state remains an unresolved source interpretation, not a passing qualification.

The assigned workspace-control consumer uses only the existing approved Rust standard library and pinned Bach/Bolero development-test closure. Registering this site does not select a substrate or alter technology approval scope.

## Native Inference Transport Qualification

On 2026-10-12 Matthew answered "I approve" to the concrete native inference transport qualification request: Reqwest0.13.5 with rustls/http2, serde_json1.0.151 with std/raw_value, the unchanged Tokio1.53.2 profile, the exact 162-package lock and recorded installed native build-tool scope. The approved proposal bytes are preserved in [the adoption checkpoint](/home/dev-user/borg-runtime/artifacts/execution/native-inference-adoption-owwuww48/before/proposals/native-inference-transport-qualification.md), SHA256 778e94f2bbe30b6a78d416f772400d3459c8adccd7717b8f8c7af59e96b84a7c. The transport lock is 31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff.

This selects private acquisition/build and bounded local synthetic HTTP/TLS/JSON qualification at qualification/inference/native. It includes the disclosed JSON1.0.151 floating-comparison limitation and exact checked numeric-path qualification. Provider credentials remain at the inference-service boundary, outside the core. Actual package bytes, build inputs and feature materialization are verified before their use; no provider request, production transport adoption, registry/configuration change or upgrade is included.

The existing native 83-package and Bach/Bolero selections retain their separate scopes. This answer concerns the latest transport question only; it does not answer the separately pending application-envelope or receipt-close questions. Capability/milestone adoption, deployment, runtime source snapshot and original conformance policy are unchanged.

## Assertion-Signature Qualification

On 2026-10-12 Matthew answered "I approve" to the pending private assertion-signature qualification question. The [exact question](/home/dev-user/borg-runtime/artifacts/execution/assertion-qualification-question-f8qy2k4u/question.json), SHA256 `ab03b1f49ce3509b830ab70c9ca597fb237fc3f486271aecfffc0f304b4de1c0`, binds [the reviewed proposal bytes](/home/dev-user/borg-runtime/artifacts/execution/assertion-signature-adoption-z5oyghwu/before/proposals/assertion-signature-qualification.md), SHA256 `c5a8815a1b512ff3ef918df16098bf5a8538a5e61ee49b8233b0895207b52ac5`.

This selects the existing aws-lc-rs1.18.1/aws-lc-sys0.45.0 profile and OpenBao2.7.1 built-in Transit for disposable plain Ed25519 signing and public-key-only local verification at qualification/assertion-signature. Actual package subset, features and build inputs are checked against the recorded existing bundles before execution. Fixture credentials are synthetic and isolated; no production access or persistent configuration is changed.

This is a new bounded application-signature use of existing supplied technologies, not approval of a production signature scheme, token format, publication adapter, key-custody arrangement or deployment. Primitive qualification does not release an authenticated runtime caller or substitute for current grants, fences and receipt handling. Separate pending envelope, receipt-close, emergency-policy and workspace-substrate questions remain unanswered by this reply. Capability/milestone approval scope, runtime target snapshot/lock and shared conformance policy are unchanged.

## Envelope, Workspace Inspection And Inbound Technology Qualification

On 2026-10-12 Matthew answered "ok, i approve those technologies" immediately
after the owner listed three technology requests: the original envelope-library
bundle, gVisor download/static inspection, and Hyper server qualification. The
exact answer, context and before-state are preserved in the [adoption packet](/home/dev-user/borg-runtime/artifacts/execution/technology-batch-adoption-_fj6tbnv/user-approval.json).

This selects the five pinned RustCrypto/zeroize roots and recorded dependency
metadata for private envelope qualification; the exact gVisor20261005.0 archive
for acquisition/static inventory only; and the Hyper1.11.1/hyper-util0.1.21 server
feature extension with httpdate1.0.3 for private loopback qualification. Exact
versions, features, checksums and consumption sites are in the main specification
and technology inventory. This answer refers to the original envelope request,
not the later AWS-LC reuse alternative.

The technology approval does not adopt a production cipher, key hierarchy,
format, server architecture, isolation profile, deployment or additional grant.
No gVisor or bundled-program execution, host/configuration change, provider call
or new external input outside the recorded sets follows. The three behavior
questions about receipt close, overlapping emergency restoration and session
admission remain unanswered. Capability/milestone approvals, current readiness,
the shared source selection, missing gate baseline and historical runtime
snapshot/lock remain unchanged.

## Five Decisions Accepted On 2026-10-12

Matthew accepted all five recommendations in the fresh register and required their selected meaning in the implementation specification. The relayed spoken "except" was explicitly confirmed as "accept". This is approval of these exact five scopes, not of unrelated declarations, internal PRs, production choices or other ED/HTTP/schema-engine alternatives. The main specification contains the requirements without personal approval narration.

| Selected decision | Exact reviewed SHA256 | Normative destination |
|---|---|---|
| decider-engine-qualification.md | `bbd6c9baf518d54d22e9a6526684d14388f2e0c73cfe4219673b0834db247a70` | [Selected meaning](implementation.md#decider-engine-qualification) |
| receipt-close-fencing-clarification.md | `786f02aca42d9e21b89b63441290aac51fdfd48f03dc941b505c9da0784381b1` | [Selected meaning](implementation.md#receipt-close-fencing) |
| emergency-override-restoration.md | `ac7d0c0906cbfb89a6ec47898417cfa9c04445553739a8b040c7e00146e13b19` | [Selected meaning](implementation.md#emergency-override-restoration) |
| session-admission-clarification.md | `92be76bd77395d848882d953d7458ba0ffc0752f22bafd50a225eecad694412e` | [Selected meaning](implementation.md#session-admission-after-reassignment) |
| Ordinary input during a budget wait | `f21b1b8107b7c6931a58c5a69c8707e919a36f10cfbce6276f9bb6f18d4324b3` | [Selected meaning](implementation.md#ordinary-input-during-a-budget-wait) |

The accepted budget frame is `b863826a59e03453afa03dd1ad33c2432f51b7ddad04f6b77254d5f9aa2947a9`. The exact [runtime proposal](/home/dev-user/borg-runtime/integration/governed-reference-turn/artifacts/native-durable-model/budget-idle-source-ofdhyvxp/INPUT_ACCEPTANCE_PROPOSAL.md) is historical and remains byte-identical; its old pending banner is superseded by this record, not rewritten. It selects ordinary-input visibility and durable pending work, not a SQL shape or budget formula. Due valid configuration may progress independently at a boundary.

Wasmi is selected only for the eight-package bounded qualification profile. Packaged-input verification precedes execution; thirteen lock-only entries are excluded. Receipt, emergency, session and budget behavior is selected; implementation and actual conformance evidence remain separate. Capability/milestone approval, ownership, readiness and production promotion are unchanged.

## Wasmi Bundled Generator

On 2026-10-12 Matthew selected only the exact wasmi_ir2.0.0 `build.rs` and its fourteen bundled helpers for the existing private eight-package qualification: "Yes, allow the generator". The [packaged-input packet](/home/dev-user/borg-runtime/qualification/decider-sandbox/artifacts/packaged-scope-delta-bqpm1368/packet.json) has SHA256 `c3374f7e943c82d5dce98263a0edd01003f2fba288a016382fda71f7f3387371`; its generator entry is `3f433746f416fb1cec6957102b480de050119a1f96b8ecf5cfb9062ae02a27d2` within the unchanged archive `8f17b774caa13c618c7244f1ee51fe23c5e7b8538a471fa46d9949779758aed6`. The selected meaning is recorded in [the implementation profile](implementation.md#decider-engine-qualification) and [technology inventory](technology-inventory.md#decider-engine-qualification-bundle).

This accepts no part of the packet's thirteen-archive metadata alternative, including equivalent1.0.2. The original checksums, features, compiler, bounded execution and packaged-graph checks remain. A bounded generator-only check may verify these four internal outputs independently; it is not a substitute for the still-required engine dependency-graph proof. No production engine, ABI, scope expansion or other declaration approval follows. The original packet and prior qualification receipts remain historical and unchanged.

## Five-Decision Compatibility Disposition

The actual normative destinations for the accepted receipt/session/emergency semantics are `spec/impl/v1/implementation.md`, under the headings below. The six shared tuples listed here remain unchanged at their original source identities. The table explicitly records v1 specialization/deviation, not retired shared citations or automatic proof transfer. Native shared planning association totals continue to include these original obligations; a passing v1 test does not discharge a conflicting unchanged shared sentence.

| Shared file and stable section | Exact unchanged shared sentence | V1 normative destination and disposition |
|---|---|---|
| `spec/contracts/tool-call.md#a-service-dedupes-by-call-id` | A close of a call in flight MUST end the call. | [V1 specialization](implementation.md#receipt-close-fencing); original shared identity retained, not proved by the replacement meaning. |
| `spec/contracts/tool-call.md#a-service-fences-a-superseded-owner` | A service MUST fence a close by lease generation as it fences an admission. | [V1 specialization](implementation.md#receipt-close-fencing); original shared identity retained, not proved by the replacement meaning. |
| `spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation` | A session's lease generation MUST advance, in the same commit as the change, on every change of the party that may act for the session: an owner move, a restore, a pause, a quarantine, a stop or end, and a resume. | [V1 specialization](implementation.md#session-admission-after-reassignment); original shared identity retained, not proved by the replacement meaning. |
| `spec/contracts/stored-form.md#authenticated-data-binds-the-envelope` | A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation, and its owner identity. | [V1 specialization](implementation.md#session-admission-after-reassignment); original shared identity retained, not proved by the replacement meaning. |
| `spec/core/failure-handling.md#state-that-cannot-be-decoded` | A quarantined session's state MUST NOT be overwritten. | [V1 specialization](implementation.md#session-admission-after-reassignment); original shared identity retained, not proved by the replacement meaning. |
| `spec/core/governance.md#two-paths-sit-outside-the-pipeline` | At expiry the replaced policy MUST be restored with an event. | [V1 restoration rule](implementation.md#emergency-override-restoration); original shared identity retained. |

The exact five-tuple proposed shared crosswalk is frozen in [the compatibility review](/home/dev-user/borg-runtime/artifacts/execution/five-decisions-adoption-n0k5tii9/review-compatibility/exact-tuple-crosswalk.json). Its seventeen new sentences are prospective shared destinations only. Actual v1 extraction and mappings, including the atomic receipt/fence split, are in the adoption receipt. The admission writer, authenticated current-control/held-content association and serialized executor acceptance are real missing producer obligations, not permission to assume fixtures implement them.

Compatibility disposition: do not expose a changed close response to an old client or reinterpret a stored header in place. First bind an explicit successor contract/format revision to the exact predecessor source identities, inventory actual readers/clients and their capability, and qualify positive and unknown-revision refusal paths. Then deploy compatible readers/clients before new writers, preserve retained original envelopes and receipt outcomes, and fence migration/cutover using the current authority. For session admission, establish and authenticate current control/high-water from verified records before admitting the session; if that cannot be recovered, hold it rather than manufacture a lease or substitute content. Migration and rollback preserve original bytes, authenticated association and generation high-water. A stale old reader/writer cannot regain authority after cutover. Concrete discriminators/encodings, deployed-consumer inventory and these migration tests are unfinished. No arbitrary numeric version, production format, physical control-store split or missing-control repair is selected here. The implementation identifier v1 is not the required protocol revision increment.

This source adoption does not activate non-additive shared contracts or change the historical runtime target. It enables the already accepted behavior work and bounded synthetic qualification under explicit v1 semantics while the compatibility obligations remain visible. The ordinary policy A, overrides B then C, late/repeated B expiry, C expiry, and normal adoption D cases close at CFG_S5; no new renewal bound is introduced.

## Three Decisions Accepted On 2026-10-13

Matthew answered "Approve all pending decisions" in message `Sentinel_dae9cbddda3081918f4476997f7a53b1` after the three-choice summary and clarification that backups may restore deleted keys and PostgreSQL becomes the production byte/metadata target. The scope is exactly the following three reviewed requests. Subscription initial/rejoin history remains pending. The exact answer, request hashes and pre-change canonical bytes are preserved in [the adoption record](/home/dev-user/borg-runtime/artifacts/execution/three-decisions-adoption-qnlsmha5/user-approval.json).

| Selected scope | Exact request SHA256 | Normative destination |
|---|---|---|
| Thirteen Wasmi archives: acquisition/static inspection/offline metadata only; original eight active graph unchanged | `da9350b9545eb87c0e4f7e92e7daf4bf93854a81637f6aea454f7252df3f5fb0` | [Decider Engine Qualification](implementation.md#decider-engine-qualification) |
| Owned immutable bytes and metadata transactionally in PostgreSQL16.15 | `863a80be06fecfc318868f5dd6815b3c1b65c198e7b3d3be49025977936e370f` | [Immutable Byte Storage](implementation.md#immutable-byte-storage) |
| Exact C2-service-held descriptor and technology/feature map, per-incarnation owner and separate state/control keys, persisted bounds and existing backup exception | `7ebf4011535e71ae2f010065147f9670b8b1ba1ba7b622179d1c7a729564b283` | [Protected Session Record Target](implementation.md#protected-session-record-target) |

The descriptor hash is `430e04637ff13cd8c47a2272d7f1630e86820e532ca3867a74f197d37e514a2e`; the exact73-external-package normal/build feature map is `74e5742840d4bcd5f1798ba487269653df96c82a74ccc193c9b366924feaf75b`. Old request/descriptor status banners and prior decision entries describe their preparation time and remain unchanged. This entry supersedes only their pending status for these selected meanings. It does not adopt additional packages/features, blanket capability/milestone declarations, readiness, PR approval, deployment, credentials/access changes or unrelated work.

Compatibility disposition: C2 uses its explicit new family/revision/profile and preserves the experimental private1 reader/domain/bytes. Compatible readers precede new writers; all participating old writers are frozen or upgraded before exposure. Actual deployed-consumer inventory and cutover/rollback tests remain required; the implementation identifier v1 and a source checkout do not supply them. Current S dispatch lifecycle kinds4,5,6 exceed the selected descriptor's listed lifecycle body variants: refuse those kinds until a complete compatible body-schema successor and its caller mapping are released, rather than aliasing or discarding them. The approved initial creation path is representable. No shared original tuple or its evidence is silently transferred to a new meaning.

The C2 session resource-owner mapping does not select workspace lineage/history ownership or allow deletion to break a live fork. The complete-frame/body bounds are encoded ceilings, not raw-input promises or total work allowances. Its node ResolveOwner operation can perform two backend decrypts; this distinction does not prove the unchanged shared one-call clause. Existing backup-retention and previous v1 compatibility/nonconformance mappings remain explicit. Storage, key/capsule issuance, format/caller integration and actual C2 qualification are new work; former private runs remain evidence only for their original subjects.

The production byte-medium selection adds no PostgreSQL version or package and establishes no capacity or survival result. The exact C2 technology target adds no endpoint, key custody deployment, host/replication layout, workspace compression or whole-workspace bound. The subscription-start decision is not answered by this approval. Existing source target, real copied snapshot/lock, shared gate and absence of its baseline remain unchanged.

## Subscription History Accepted On 2026-10-13

Matthew separately accepted the recommended initial/rejoin policy in user message `Sentinel_c1398c11dd348191888d44782e09c29d`: "Yes. Approve it." The preceding assistant message `Sentinel_8309c6e234248191befb0bd46c9a2466` explicitly described delivery from the actual join point, preserving prior entitled-but-undelivered work and no automatic replay of the unsubscribed gap. This is the fourth decision, not retrospective expansion of the earlier three-decision approval.

The exact [request](/home/dev-user/borg-runtime/artifacts/execution/subscription-start-policy-request-n26y_8by/request.json) has SHA256 `9e4896bd37a0c89aeda4eb5555964757a51e0c14015a082f8fcd0c8b189c9bcb`; its [readable recommendation](/home/dev-user/borg-runtime/artifacts/execution/subscription-start-policy-request-n26y_8by/REQUEST.md) has SHA256 `fed68634e8e4e7230f8a91c25b1aed4eeb09acbf33b8ddbecd6d925746fcc66b`. The source addendum remains `a82060c7e162cc1ee8067976fbdadee5adfb72ff4945d5122f943150976a9f75`. These request/proposal bytes and the previous three-decision receipt retain their historical pending statuses. This entry closes only this named delivery-history decision.

The selected meaning is normative under [Subscription History](implementation.md#subscription-history), with focused existing ED primitive/fan-out/caller and collaboration assignment acceptance. Established publish-time entitlement, unsubscribe non-retraction, current security revocation, retention and explicit-drop handling remain. No routing, numeric cursor, stored format, backend, retention duration, dependency, deployment, credential/access or explicit history-replay alternative is selected. Other capability/milestone approval and readiness remain unchanged.

Compatibility disposition: the shared requirements and exact Source quotations are unchanged. This v1 policy resolves a previously unspecified initial/rejoin history choice; it does not transfer proof from prior private tests or silently adopt an optional historical-start API. Qualify actual subscriber/publisher/assignment paths and recoveries against their retained data and consumers; an incompatible existing consumer still requires the existing version/migration discipline. The choice does not authorize a new wire encoding or claim such migration complete. Actual membership/order and assignment/stop publication must not rely on a replay of a previously unsubscribed gap.

The [separate adoption checkpoint](/home/dev-user/borg-runtime/artifacts/execution/subscription-history-adoption-rp07ltjj/user-approval.json) preserves the trusted answer, exact request identities and pre-change source bytes. The previous three selected scopes and their constrained technology permissions remain in effect without expansion.
