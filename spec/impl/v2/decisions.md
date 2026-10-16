# V2 Decision And Review Record

This descriptive record holds provenance and unresolved choices. It does not add product requirements or imply that a draft is ready to execute.

## Restart And Authoring Scope

On 2026-10-13 Pacific time, Matthew stopped implementation and requested deletion of the full implementation except Duvet Cap. The deletion receipt confirms that the implementation and copied build/qualification artifacts were removed while specifications, Git metadata and the existing tool/input closure were preserved. There is no implementation archive.

Matthew then requested a v2 specification based on the discussion and a review of all previous technology decisions. This authorizes this authored review draft. It does not restart implementation, silently approve a new dependency stack, modify shared outcomes or approve deployment.

## Design Directions Discussed

The discussion considered removing Borg-managed content encryption, reducing full workspace sandboxing, and replacing elaborate workspace snapshots/rollback/migration with disposable workers and explicit durable checkpoints. It then considered Codex instead of a native harness and converged on exploring one agent-definition model with a small native executor plus Codex for extended work. Earlier questions were exploratory; the v2 request permits concrete drafting of these directions, not an assertion that every clause or technology is separately adopted.

The latest clarification is explicit: backups can be deferred initially. The draft therefore excludes initial backup creation, point-in-time recovery and disaster-recovery guarantees, while retaining process-restart durability on intact storage. No production recovery objective is yet selected.

## Task-Level Executor Boundary Accepted

Matthew answered "ok, do it" to the recommended initial v2 contract: durable tasks and accepted results, explicit code checkpoints, replaceable workers, and honest interruption/retry behavior. The recommendation uses one agent-definition model with bounded native work and Codex-owned extended model/tool execution. Exact preservation of an unfinished Codex conversation is later work.

The accepted recovery distinction is: recover task state after an application restart; resume a supplied-executor conversation when its saved history is available; otherwise create a new attempt from the same durable task and checkpoint. Reconcile uncertain external effects before retry. This does not waive permission checks, allow false success or change final rollout authorization.

This selection did not itself approve a specific Codex release, entry point, process/session topology, storage product/profile, credential mechanism, deployment or implementation restart. The subsequent security and concrete-profile selections are recorded separately below.

## Minimal Security Posture Accepted

Matthew directed: "we should adopt a minimal security posture. we can use ssh to create tunnels between hosts and do port forwarding if we want, but that's it", followed by "we should not rely on openbao" and "or any key service".

The selected v2 posture uses trusted operator-controlled hosts and workers, ordinary host access controls and optional authenticated SSH tunnels/port forwarding. It excludes OpenBao and every managed or owned key/secret service, custom assertion-signing infrastructure, per-call credential issuance and a separate certificate-management system. The draft's initial omission of application content encryption is consistent with this posture; no encryption-key hierarchy is retained as a dependency. Ordinary SSH keys and provider credentials remain necessary and locally configured. Existing provider HTTPS is not replaced by an unencrypted connection.

Configured task permissions, secret hygiene, truthful ownership/recovery, protected validation and final human rollout authorization remain. This is not a hostile-workload containment profile: a compromised trusted worker can use its available credentials and damage accessible data. The implementation does not claim the older shared signing, broker, root-key or sandbox guarantees. The compatibility ledger records those differences; shared requirements and gate selection remain unchanged.

This is a v2 specification selection, not an instruction to configure SSH, deploy services or restart implementation. Exact host accounts, provider credentials, SSH package/profile and process layout remain configuration or technology review details; no replacement security service is to be designed to fill that list. The subsequent concrete-profile selection below resolves the earlier SQLite suggestion.

## Concrete V2 Profile Accepted

On 2026-10-13 Pacific time, Matthew answered "Yes approved" to the complete proposed v2 package, including the explicitly highlighted single-controller downtime, automatic merging after protected checks, and replacement of the blanket 100 percent line/branch coverage gate. The accepted scope is:

- One controller using local SQLite; workers use the application API; no automatic controller failover initially. Git checkpoints preserve source outside the disposable worker, not task-database backups.
- Codex app-server over local stdio through a small adapter and pinned release, with exact-interface qualification before reliance; bounded native tasks remain.
- Initial collaboration selection: tasks, assignment, dependencies, status, cancellation, retry and results. The later-only deferral of the selected document, question, review, resolver-wait and resolution-delivery increment is superseded by [Collaboration Workflow Expansion Queued](#collaboration-workflow-expansion-queued); other collaboration deferrals remain.
- Database persistence for task history, accepted outputs and explicit agent memory; disposable binaries/caches rebuilt; durable large-artifact storage only when a concrete workflow requires it.
- Automatic edit/build/test/commit/push/pull-request preparation and automatic merge after protected checks for the exact revision. Merging does not authorize deployment. Other external actions need configured permission; production rollout retains its human gate and mandatory checks.
- Ordinary configured permissions and application checks; Wasmi and programmable policy execution deferred.
- Mandatory end-to-end, controller-restart, worker-loss, duplicate-effect and stale-owner tests, focused Bach/Bolero and measured coverage, with review of uncovered critical paths instead of v1's blanket full-line/full-branch gate.

This records selected v2 meaning, not qualification results, a selected dependency version/feature closure, implementation restart or shared-spec amendments. The existing ten-host scale objective was explicitly retained in that package. The subsequent [four-host target selection](#four-host-target-accepted) defines first production qualification while retaining the larger objective for later scale qualification.

Matthew confirmed that the four intended boxes have the same size as the current box. Local read-only inspection found 32 CPU affinity slots, approximately 98.2 GiB physical RAM and a 580.2 GiB filesystem. The other boxes were not accessed; equal sizing is the supplied planning input, not remote measurement.

## Four-Host Target Accepted

Matthew answered "Ok. Sounds good. Approved." to the revised four-host recommendation: one controller and three workers; 256 native model requests in flight and eight active Codex coding workspaces per worker; at most four heavy builds/tests per worker. Cluster totals are 768 native requests, 24 coding workspaces and at most 12 heavy builds/tests. Native requests ramp through 32, 64, 128 and 256 per worker while coding/build work runs; 512 per worker is stress-only. Queued requests and idle definitions do not count as active capacity.

This is an adopted qualification target, not measured capacity, provider quota, implementation restart or deployment authorization. Local and provider constraints still govern actual admission. The existing ten-host objective remains a later scale goal; its requirement sentences are retained, and four-host results do not discharge it. The ancillary service-latency numbers, one-hour/24-hour durations and per-build CPU/memory allowances in the linked target document were supporting proposals, not separately selected immutable requirements.

## Every Workspace Can Build

During implementation-ready preparation, Matthew directed: "Just make any workspace able to build for now". This supersedes the separate four-heavy-build-per-worker limit, twelve-cluster-wide cap and proposed command-permit mechanism. Every coding workspace runs repository builds/tests directly under ordinary task/host-resource bounds. No build-admission service or separate permit queue is required. The coding-workspace and native-concurrency qualification targets are unchanged. This is a specification correction, not an instruction to start implementation or execute builds.

## Objectives Carried Into Review

One tenant, the existing preferred model, Rust for owned runtime components, the collaboration product, runtime self-delivery with final human rollout approval, and the simultaneous ten-host/1,000-agent/ten-workspace per-host objective remain visible. Their old v1 approvals remain v1 provenance. This draft does not claim whole v1 adoption, whole v2 approval, compatibility or executable readiness.

## Implementation-Ready Package Preparation

Matthew instructed "Ok. Proceed." after the proposed next step: reconcile inherited requirements, finish a compact execution contract and exact dependency review, build a small parallel delivery plan, then review that package before implementation restart. This authorizes the current preparation, not implementation or shared-profile activation.

The new detailed task/attempt/command records, immutable startup prerequisites, separate parent-child wait relation, one-controller database queue, per-workspace executor process decomposition and exact API fields are proposed engineering contracts. Their source is the main task contract and linked protocol proposal. The five capability and twelve milestone declarations are proposed allocations; no worker ownership, completion or readiness is asserted by writing them. Existing accepted v2 selections retain their status.

The source-grounded dependency profile proposes exact direct pins, including a SQLite3.54.0/rusqlite pairing, current JSON defect fix and an existing JSON Schema validator. It is not a resolved checksum-complete dependency closure or a successful build. The shared profile patch is unapplied registration groundwork; additional first-release exclusions, conditional choices, replacement bindings and native profile-gate enforcement remain explicit review items. No shared file, active gate, real lock or copied snapshot is changed.

## Decisions That Materially Affect The First Journey

| ID | Decision | Recommendation | Remaining choice |
|---|---|---|---|
| V2_D1 | Executor profile: accepted | Bounded native requests plus task-level Codex app-server over local stdio; pinned release and small adapter. | Exact release/profile qualification, optional history persistence and process/session resource mapping. |
| V2_D2 | Worker trust: accepted; topology open | Disposable trusted workers with ordinary host permissions, minimum privileges and explicit resource bounds. | Process/account layout on counted hosts and the exact accessible-resource boundary; no new sandbox or credential service. |
| V2_D3 | Persistence: accepted | One controller with local SQLite; remote workers use its API; no automatic controller failover initially. | Exact patched SQLite/client profile, resource limits and measured workload capacity. |
| V2_D4 | Minimal security posture: accepted | Trusted hosts/workers, optional SSH tunnels/forwarding, ordinary local credential configuration; no OpenBao or any key/secret service. | Exact host/forwarding configuration and shared-contract disposition; no signing or secret-management service selection pending. |
| V2_D5 | Recovery and memory: accepted | Remote Git source checkpoints; database task history, accepted outputs and explicit agent memory; disposable caches/binaries. | Repository configuration; a separate artifact service only if a concrete preservation need emerges. |
| V2_D6 | Initial backup deferral: accepted | No initial backup/PITR/restore promise; state the permanent-loss boundary. | Later production recovery objective and evidence before claiming it. |
| V2_D7 | Verification: accepted | Behavior/recovery acceptance, focused Bach/Bolero, real dependency tests and measured coverage with critical-path review; no blanket full-line/full-branch gate. | Exact scenarios and measurements for the chosen release; shared-contract compatibility disposition. |
| V2_D8 | Product scope: accepted and selected workflow expansion queued | Task collaboration plus the four bounded workflow batches in [Collaboration Workflow Expansion Queued](#collaboration-workflow-expansion-queued). | Compiled common contract, retained-state compatibility and actual component/joined qualification; unrelated broader collaboration remains deferred. |
| V2_D9 | Autonomy and policy: accepted | Configured actions and ordinary checks, automatic code preparation/submission and protected-check merge, final human rollout; no initial Wasmi. | Repository branch configuration and any separately requested non-coding external action. |
| V2_D10 | Four-host production target: accepted | One controller plus three workers: 768 outstanding real native model requests at the worker client boundary and 24 coding workspaces, all able to build/test; mixed qualification ramp and stress-only 512 per worker. | Exact workload/resource/provider qualification; ancillary latency and soak budgets; later ten-host workload definition. |
| V2_D11 | Initial model-budget deferral: accepted | Maximum supported default task request allowance; retain existing native debits, child reservations and observed usage. | Fine-grained cost/token and opaque executor inner-request enforcement are deferred, not first-release blockers. |
| V2_D12 | Worker-wide resource group: accepted | One cgroup v2 per worker, generous shared limits, ordinary startup configuration and exhaustion/recovery qualification. | Exact host limits/setup and separate filesystem quota/journal-exhaustion work; no per-task resource manager. |

This record distinguishes settled selections from remaining qualification details; it is not a new permission queue. Previously approved facts are not reopened merely to repeat their history; changed scope or a new product remains explicit. The separate [technology review](technology-review.md) covers every prior family and candidate rather than treating an old lockfile as a new selection.

## Additional Deferrals And Simplifications Accepted

After the walkthrough of initial-release deferrals and simplifications, Matthew answered: "agree with all of those. should we also defer permissions?" The agreement selects the preceding recommendations; the permission question is exploratory and does not itself change the existing authority requirements.

The additional first-release deferrals are general publish/subscribe, registered webhooks and recurring user timers; distributed keyed-session/partition placement; full provider-neutral replayable transcripts, tails and upload/attachment services; automatic experiments, learning and observation-episode pipelines; and advanced collaboration archive/dwell/overdue monitoring, external ingestion, person notifications and one-time operator recovery. Task assignment/progress/completion, worker wakes, task deadlines, task history/results, explicit agent memory, definition/source provenance, basic usage measurements and runtime self-delivery remain in scope.

The selected simplifications are bounded oldest-eligible scheduling without inherited priority/quota rebalance/provider batching; immutable direct definition revisions without live layered inheritance/keyed-instance reconciliation; one active execution per agent identity while distinct agents from the same template and independent child agents can run in parallel; and explicit application/database schema versions with deliberate migrations rather than a permanent tagged-envelope compatibility promise. The reviewed noninteractive host/provider authentication, designated human final rollout and secret hygiene at owned boundaries accompany the already selected trusted-worker profile; they do not establish hostile opaque-executor isolation.

This records the selected scope directions. Exact replacement requirement bindings and shared-profile/gate reconciliation remain preparation work; the inactive shared registration patch is not applied. The detailed capability/milestone declarations and exact dependency profile are not blanket-approved by this answer. That answer did not yet adopt permission-system deferral; the subsequent selection is recorded below. The question about coordinating unpushed code did not add a live synchronization mechanism.

## Fine-Grained Permissions Deferred

Matthew answered "yes, defer those" to the recommendation to defer fine-grained grants and a permission engine while using ordinary SSH/Git/provider access controls and retaining protected checks and final human rollout approval. This supersedes the earlier per-task authority/grant wording for initial v2. Authenticated private access, configured worker credentials, non-admin workers, secret hygiene, exact-revision protected checks and artifact/target-bound human rollout remain. Ownership generations, effect reconciliation, budgets and deadlines remain correctness obligations. Tasks sharing a worker identity are not separately isolated by a Borg permission system. No actual access configuration or external action is authorized by this specification selection.

## Browser Collaboration Interface Required

Matthew directed "yes, we should make sure it's a browser UI" after the gap in the minimal readable-interface wording was identified. The selected first product interface is a browser task list/detail UI with submission/assignment, dependencies, status/progress/available recent output, cancel/retry and source/result links through the same controller API and state. A command-line-only implementation cannot close the product milestone. No frontend framework or separate product store is selected. The early controller-worker smoke test remains independent of full UI completion.

## Build Packet Preparation Completed

Matthew directed "Ok, finish that" after the proposed final preparation: reconcile accepted v2 scope against the older shared requirements, freeze the direct dependency and controller/worker/browser interface proposal, and prepare four whole-component assignments with a single integrator and one real journey. This authorizes the completed specification/planning packet, not package execution, shared-gate activation or an implementation restart.

The selected oldest-eligible scheduling, one active execution per agent, immutable definition revisions and explicit stored-schema migration/refusal meanings are now stated in the main specification. The protocol's exact envelope/operation fields, queued-compatible assignment, WIP ref namespace, normal-form browser delivery and batch file boundaries are concrete engineering proposals for the review package. They do not adopt a new frontend technology or rewrite shared contracts.

The browser's completed milestone now requires the real coding-journey release, adding one capability edge and one typed prerequisite reference. This is qualification of the already required browser workflow, not a new product feature or a barrier to writing the UI independently. All detailed declarations retain proposed review status and empty actual ownership/readiness. Shared original source, the actual runtime target/snapshot and existing historical evidence remain unchanged.

## V2 Build Execution Authorized

Matthew directed "Ok proceed" after receiving the completed build packet and the next step to resolve/qualify its pinned dependency profile and build the four whole-component batches. This starts the new v2 implementation and bounded qualification on the standalone development host. The reviewed direct inputs/features, concrete task/worker/browser contract and first-release work packages are selected for this build; successful dependency qualification, runtime acceptance and scale results still require actual evidence. Resolve one complete dependency graph under the recorded profile; do not silently substitute a direct dependency or incompatible feature set.

The first-release capability/stage/milestone review metadata records this scoped selection without fabricating ownership or readiness. The later ten-host milestone remains future scope. This authorization does not apply shared-profile amendments, alter the historical lock/copied snapshot, restore deleted prototype code, grant new credentials/access, provision other hosts or authorize production rollout. Final human rollout and protected checks remain.

## Available JSON Version For The V2 Build

Matthew directed "Let's just use a different version?" after the managed registry withheld serde_json1.0.154. The build selects the available1.0.151 with the same std/raw_value/float_roundtrip features. This changes the direct version, not the application's numeric or schema requirements. The library's known Value-to-f32 infinity-comparison defect remains; owned runtime decoding and the pinned schema validator avoid that comparison and require focused actual-path regression checks. arbitrary_precision remains off, so its separate negative-zero defect is outside this profile. No registry policy change, alternative source, package fork or upstream-fix claim is made. The existing build authorization and source/feature checks continue to apply.

## Native Coding Handoff Representation

The existing v2 build authorization covers the selected native-to-coding handoff. Its engineering representation now uses immutable task workflow configuration, a closed complete/delegate response, unchanged original-schema validation, an explicit complete-result byte bound, and exact durable delegation replay. The handoff batch introduced private application wire revision2, database schema3 and worker state format2. Earlier private fixture formats are refused unchanged; no actual database or worker-state migration is performed. This is the representation disposition required by the existing task contract, not a new provider, storage, access or permission choice. Supplied-executor inner request usage remains unknown; reserved accounting is not a claim of a full aggregate coding budget. Source-bound tests and actual provider qualification remain separate, and the pending browser/model execution denial is not bypassed by these local fixtures.

## Native Validation Recovery Representation

The existing restart/accounting requirements are implemented using the existing managed process owner, with durable registration before validator input, identity-bound cleanup and ownership retained through dropped-future quarantine. Database schema4 binds controller helper tracking to its durable database and local registry; unsupported earlier schemas and mismatched or partial bindings are refused without migration or repair. Worker state remains format2 with explicit per-attempt tracking provenance; historical unfinished untracked native work is not silently upgraded. Stable kernel file locks exclude simultaneous local owners. No dependency, feature, provider, access, tenant or shared-profile selection changes. Qualification records distinguish actual helper cleanup from unknown remote requests, retained request reservations and unavailable token usage.

## Source-Linked Artifact Preparation

The existing change-preparation requirement now has a concrete packaging tool using the selected build profile, without a dependency or compiler-flag change. Its exact-source recipe, deterministic application package and external expected-digest verification are described in [the protocol](protocol.md#source-linked-release-artifact). Two actual fresh offline builds of a private immutable fixture copied from current maintained source produced identical packages. The verified packaged executable passed private controller, schema-validation, task-idempotency, cancellation and process-restart checks. The [execution receipt](../../../../borg-runtime/artifacts/v2/build-60156282/release-artifact-uu1xhxx9/completion.json) binds the source, tests and limits. Private fixture history is not a canonical repository commit. This evidence does not establish protected checks, merge authority, rollout approval, a real browser/model journey or four-host qualification.

## Release Candidate Representation

The existing change-preparation scope now has typed candidate records bound to an accepted coding result, its acknowledged checkpoint and an artifact descriptor. These operations extend private wire revision2; existing operation shapes remain unchanged. Database schema5 adds the bounded candidate records; schema4 and earlier are refused unchanged, with fresh fixture roots and no migration of retained state. Package inspection occurs in preparation tooling outside database transactions. Ordinary API data remains a preparation claim, including after local byte verification. No checker, repository adapter, protected account, deployment target or human-authorization mechanism is selected by this representation. Those configured producers remain required for their corresponding release and rollout claims. The main requirements, declaration approval maps, shared profile and historical runtime target are unchanged.

## Task Recovery Corrections

The existing v2 recovery and acceptance requirements distinguish executor completion from durable task acceptance and unknown remote effects from local process termination. A source review found that provider disposition could be discarded and a committed validation-infrastructure refusal could strand an otherwise completed result. The correction preserves typed remote disposition and introduces the narrow private ReconcileResult operation described in [the protocol](protocol.md#worker-commands-and-reports). It binds the original candidate and refusal, rechecks current acceptance guards, and never replaces the original receipt or starts provider work. Worker state advances to format3 so older readers cannot ignore pending acceptance before replacement registration; format2 is refused unchanged, with no automatic migration. Database schema5 and earlier operation meanings remain unchanged. The native validation helper's internal revision changes with its matching executable, separately from the application wire and durable database formats.

The same correction batch follows durable delegation lineage when cancelling outstanding descendants beneath a live root, and derives queued-task reasons from current admission checks. Terminal-root cancellation retains its existing behavior. These are implementation corrections within the authorized v2 contract, not new provider, dependency, permission, storage, deployment or shared-profile choices. The [source and validation receipt](../../../../borg-runtime/artifacts/v2/build-60156282/task-recovery-124tfa07/completion.json) records actual results separately from this representation decision.

## Durable Mutation Replay Correction

The authorized v2 implementation audit found that registration and command exchanges lacked mutation receipts and that successive external-settlement observations shared one effect identity. These were gaps against the existing exact-replay requirements, not new product choices. The concrete correction adds separate mutation identities, bounded cleanup accounting, and worker request/response journals with a fresh launch-currentness check. It reuses the existing database, receipt engine and process ownership; no dependency, service, permission or deployment choice is added.

The private compatibility disposition is wire3/database6/worker4 with fresh qualification roots and explicit refusal of older stored state without rewriting it. No migration or invented historical receipts is performed. Operator revision comparison preserves the existing ownership requirement because each ownership-generation change advances the revision atomically; the main exact requirement sentences remain unchanged. The [batch source and validation evidence](../../../../borg-runtime/artifacts/v2/build-60156282/mutation-replay-j8fewqvc/README.md) distinguishes design, saved implementation and actual qualification.

## Committed Reply Context Correction

The existing concrete interface already requires committed mutation identity and revision. The earlier reply envelope exposed transport correlation and the outcome but did not consistently provide that durable context. The correction makes original receipt metadata explicit and distinguishes it from a current read's observation revision. It also closes the consumer gap in which an uncommitted delegation refusal could be treated as a final remote decision. The controller, worker, browser and preparation-tool consumers preserve the original request on unresolved outcomes; no new authority, feature, dependency or storage service is selected.

Private wire4/schema7/worker5 qualify fresh roots and refuse previous representations without rewriting. No historical commit revisions or original receipts are manufactured, and no retained non-fixture migration is performed. The exact main requirements, capability/milestone approval maps and shared/runtime target pins remain unchanged. [Source and actual qualification](../../../../borg-runtime/artifacts/v2/build-60156282/reply-metadata-qrc806qw/README.md) are separate from this engineering record.

## Native Direct-Tool End-To-End Continuation

On 2026-10-14 Matthew directed continued v2 work toward end-to-end, explicitly confirmed "I approve. Keep it going", and clarified that the target is the native model loop. The recorded instruction requires an actual native model request, direct governed tool execution and a subsequent native model response/accepted result; a controlled executor or native-to-Codex delegation is not equivalent evidence. This newer instruction supersedes the earlier implementation cancellation for the authorized v2 work. Normal action review, access boundaries and dependency/deployment limits still apply. The prior browser-coding rejection remains historical and did not execute.

The concrete first direct-tool integration is a fixed read of the calling task's committed controller state, followed by one native model continuation. It reuses the selected provider/model, existing controller API, request reservations, durable observations and acceptance checks. It selects no shell, arbitrary URL, new credential, tool catalogue, dependency or deployment. The bounded workflow is described in protocol.md; its actual implementation and real-provider qualification remain separate evidence. The existing full v2 approval scopes remain unchanged except for this explicitly requested native-tool continuation.

## Initial Model Budget Work Deferred

On 2026-10-14 Matthew directed: "I think we can defer the budget work. Let everything run with max budget." This resolves the previously pending supplied-executor budget choice. Initial v2 does not require fine-grained spending/token controls or hard enforcement of unknown Codex inner-request counts before progressing to release qualification.

The selected default is the maximum finite allowance representable by the existing model_requests field: 4294967295. A default delegated coding allowance fits the parent's representation after its two required native reservations. Explicit accepted limits and legacy request bodies are not rewritten; native request debits, child reservations and reported usage remain truthful. This is not a claim of unlimited provider capacity, known hidden usage or relaxed deadline/output/concurrency/host controls. No dependency, model, access or deployment selection follows. The actual source, tests, native identity changes and current planning evaluation are recorded in the [budget-deferral receipt](../../../../borg-runtime/artifacts/v2/build-60156282/budget-deferral-r6rrqtoe/completion.json).

## Worker-Wide Resource Group Selected

On 2026-10-14 Matthew asked whether cgroups make operation more difficult. The recommendation was one worker-wide group with generous limits, ordinary startup configuration and exhaustion/recovery tests, avoiding per-task reservations, dynamic balancing and a custom resource manager. Disk quotas were identified as separate work. Matthew answered: "Ok. That sounds good."

This selects the simple worker-wide Linux cgroup v2 direction. It does not select numeric limits, a quota backend, a host/service manager, additional privileges or a provisioning action. Shared limits provide no per-task fairness or hostile-workload isolation. Startup puts the worker in its group before descendants launch; actual membership and exhaustion/recovery still need qualification. The [resource document](proposals/worker-resource-envelope.md) distinguishes the selected group scope from outstanding storage/host details. [Selection and validation evidence](../../../../borg-runtime/artifacts/v2/build-60156282/worker-envelope-selection-1qh71rmn/completion.json) remains separate from implementation and host enforcement.

## Worker Resource Verification Representation

The selected worker-wide boundary uses an operator-only optional startup profile containing an absolute cgroup directory and expected finite CPU quota/period, memory, swap and process ceilings. The exact fields are in [the interface](protocol.md#worker-resource-profile). This is an engineering representation of the selected boundary, not a numeric host policy or a new resource-management service. It changes no task wire, database or worker-state format and grants no kernel write authority.

Verification follows actual opened cgroup2 identity and current membership. Existing owned-process cleanup precedes profile refusal; work-producing recovery and fresh admission require verified configured controls. Unconfigured development runs remain explicitly unqualified. Local parser/refusal/integration evidence does not replace real finite-limit inheritance, exhaustion and recovery tests. [Implementation and validation evidence](../../../../borg-runtime/artifacts/v2/build-60156282/worker-envelope-implementation-e42rg9kw/completion.json) records what actually ran.

## Interrupted Checkpoint Publication Recovery

The existing v2 recovery contract requires inspection of the original Git reference when publication completes before its task checkpoint commits. A local controlled-executor crash test reproduced a missing producer: the worker published successfully but died before creating its settlement journal, then restart classified the known completed execution as generic uncertainty. The correction retains a bounded acknowledged completion/preparation phase before publication and recovers only that phase through exact remote readback and original-operation settlement. It adds no task behavior, provider, storage engine, dependency, access or deployment selection.

The private representation is wire4/database7/worker6, with a version1 checkpoint phase. Fresh qualification roots use the new worker format; previous nonempty roots are refused unchanged. No retained non-fixture migration or invented historical receipt is performed. Generic executor/provider uncertainty still blocks repetition, and publication alone does not establish task acceptance. The [batch evidence](../../../../borg-runtime/artifacts/v2/build-60156282/checkpoint-crash-recovery-_cw2bqua/README.md) records reproduced failure, implementation progress and actual qualification separately.

## Agent List In The Browser Product

On 2026-10-14, Matthew requested: "the UI should also have an agent list in addition to a task list". The first-release product now includes that view, using the existing controller agent directory and task associations. Distinct agent identities remain distinct even when they use the same definition, and reassignment does not erase an agent from the directory. This adds no new agent lifecycle, lease, storage engine, permission model or worker-host identity. The requirement is allocated to V2_PRODUCT and its existing product acceptance gate; implementation and browser qualification remain separate evidence.

## Worker Capacity Measurement Selected

On 2026-10-15, the operator was asked: "For the scale target, should acceptance prove that each worker sustains 256 outstanding real model calls alongside eight coding workspaces, or that the provider is processing all 256 at the same time? I recommend the worker-capacity target: we can verify it directly, while the API does not expose enough information to prove the provider's internal concurrency. Other implementation work continues."

The operator answered: "Verify worker capacity (recommended)".

The selected meaning is outstanding real provider calls measured at the worker client boundary. The four-host layout, 256 native calls plus eight coding workspaces simultaneously per worker, and 32/64/128/256 native ramp remain. Locally queued tasks do not count; client/network and provider-internal measurement limits remain explicit. This replaces the earlier provider-admission exclusion as the measurement interpretation. It does not establish achieved capacity, runtime readiness, provider allowance, an additional execution authorization or a later ten-host result. Existing approval and release-authority boundaries remain.

## Shared Collaboration MCP And Native Provider Tools

On 2026-10-15 Matthew directed: "Native tasks should have the collab space exposed. This should be exposed via mcp server to all native tasks and codex tasks." He clarified: "This is what I mean by Borg metadata". When worker capacity, task attempts, checkpoints and release-check records were identified, he added: "These should probably all be available as well if they aren't via mcp". He then confirmed the layering: "Yes. Agreed. The separate api is layered under mcp for agents".

This selects one collaboration MCP surface for both executors over the existing canonical controller operations, with existing collaboration writes and additional named read-only operational observations. It preserves the separate worker/control-plane API, current durable identity/reconciliation rules, explicit executor choice and absence of automatic fallback. Native Responses web search is separately profile-controlled. No new permission engine, project isolation, credentials, access grant, public endpoint, protected-release authority, provider service or additional dependency is selected by this feature direction.

Engineering direction: Borg is the native MCP client/function adapter; the provider does not directly connect to the private controller. The server uses the existing controller listener and handler. Exact pinned Codex discovery/call compatibility and all native/controller lifecycle changes need source-bound implementation evidence. The integration plan records those gaps and current profile/migration work. Existing first-release declaration approval remains; ready/ownership and actual execution evidence are not manufactured by this source update.

## Astra And Maximum Reasoning Effort Selected

On 2026-10-15, Matthew requested Astra for all Borg model execution, initially with max effort for native and ultra for Codex, then clarified: "actually just make it max for both". The current selection is gpt-6-astra with reasoning effort max for both native and Codex. It supersedes the earlier gpt-6.1-sol selection and the interim Codex ultra suggestion. This is a model/effort selection, not a new executor, dependency, credential or access grant. Existing task limits, explicit executor choice and no automatic fallback remain unchanged.

The implementation records this selection in the executor description and technology row T13. Native requests and Codex task configuration carry the exact selected values. Historical Sol execution receipts remain evidence for their original inputs; they are not relabeled as Astra qualification. Current source changes and checks are recorded in [the model-switch artifact](../../../../borg-runtime/artifacts/v2/build-60156282/astra-max-integration-3kqxgok7/).

## Dedicated Borg Peer Ssh Access Selected

On 2026-10-15, Matthew selected one dedicated shared Borg SSH keypair, permitted Borg tunnels and remote shell commands, and requested a copy on borg-builder. The resulting access uses the existing mtolton account on agent-lab, agent-lab-2, agent-lab-3 and agent-lab-4. Private copies exist only on those four hosts and borg-builder; the public key is accepted on the four agent-lab hosts, with agent and X11 forwarding disabled. No new inbound access was added to borg-builder.

The three worker controller/Git tunnel services now use direct private peer SSH, the dedicated key and pinned host keys. The builder's cross-cluster route uses the same key through its existing gateway transport; it is not a claim of gateway-independent builder access. The runtime account does not receive the private key. [The cutover receipt](../../../../borg-runtime/artifacts/v2/build-60156282/borg-peer-ssh-xet6rqqf/cutover-receipt.json) records all three tunnel replacements, unchanged runtime processes and unchanged task records. This access choice does not grant model tools arbitrary SSH or protected rollout authority.

## Template Inference And Fresh Instances

On 2026-10-15 the user confirmed that model and effort belong to the agent template and requested parallel instances from one template. The selected model remains Astra with maximum reasoning effort for both executors. The implementation binds settings to the accepted immutable definition revision, not a worker-wide override. New browser drafts create fresh operation-bound instance identities; exact retries preserve them. Explicit existing identity reuse retains its established serialization and memory. This records the configuration and instance behavior, not evidence of a deployed successor.

## Repository Reads Through Collaboration Mcp

On 2026-10-15 the user requested repository read access through MCP so native tasks can inspect source while coding agents handle editing and builds. The selected scope is bounded tree/file/search/history/diff reads at recorded exact revisions through the existing shared MCP interface. Repository and checkpoint identities remain canonical controller data. No arbitrary host-file access, new repository credentials, general shell, automatic executor fallback or coding workspace is granted by a read. Implementation and deployment qualification remain separate.

## Worker Local Repository Caches And Worktrees

On 2026-10-15 the user accepted the recommendation to keep the central Git repository on agent-lab, with one local repository cache on each execution worker and a separate worktree per coding attempt. Workers continue to edit and build locally. Committed checkpoints return to the central repository; worktrees do not automatically merge concurrent changes. The user replied, "Yes. Do that." This selects the cache/worktree implementation change without adding shared network storage, repository access or a deployment claim. Independent clones remain the last deployed implementation until the successor is qualified and installed.

## Template And Collaboration Representation Transition

The accepted template settings and shared collaboration interface require private wire5/schema9/worker format7. The schema8-to-9 transition validates existing records and changes only the version; format6-to-7 validates stopped worker ownership and known journals before advancing only the format marker. Both preserve historical bodies, original receipts and pending/uncertain work. This is compatibility handling for the already selected features, not a new storage engine, authority, implicit data repair or shared-contract amendment. The previous schema7-to-8 path remains attributable to its historical binary. Local qualification and actual deployed migration remain separate claims.

## Browser Information Parity And Live Activity

On 2026-10-15 the user requested: "Also queue ui changes. Anything visible via mcp should be accessible through the UI. Keep it simple. Additionally make sure the UI live updates, and include a feed along the right hand side which shows Borg activity." This adds browser information parity, automatic state refresh and a right-side activity feed to the existing product increment. Existing legitimate operator actions remain; the interface does not impersonate agent-bound callers or grant protected release authority. The same controller records and query layer remain authoritative, with no additional store, UI framework, credentials or service selected. The existing product owner has started the isolated batch; source adoption is not evidence of a completed or deployed interface.

## Agent Identity And Session Terminology Correction

On 2026-10-15 the user clarified: "Ok. I think we got terminology confused. Template should be agent identity. Change it. And then session is an instantiation" and "Implement the Borg spec in terms of agents and identities and their memories and sessions". This supersedes the earlier interpretation that a fresh template instantiation creates a new durable agent and private memory. The stable definition identity is the durable agent identity; sessions instantiate it and share its explicit memory, with separate execution state and current-session fencing. Definition revisions remain immutable and pinned by sessions.

The user then stated: "We don't need backward compatibility. I think you can reset it right?" The selected development transition is a clean-state reset coordinated with the new build, retaining repositories, source commits, credentials and separate evidence. It discards current controller task/session/memory data and worker execution state after owned processes are stopped. No reset has been executed by this source change. Old migration qualification remains historical; it is not a prerequisite to this transition.

## Shared Memory Change Notifications Accepted

On 2026-10-15 Matthew asked whether agents should be notified when shared memory changes. The recommendation was committed key/revision notices to other active sessions of the same agent, coalesced by key and received at a safe model/tool boundary; existing revision conflict checks remain. The recommendation explicitly excluded interrupting an in-flight tool, creating a session, or making a model call for every write. New sessions read existing memory; process restart does not discard pending changes. Matthew responded: "Ok. We should probably implement it". This selects that behavior for native and supplied-executor sessions. It does not activate automatic observation/learning, general subscriptions, new access, dependencies or deployment. The implementation and actual model-visible delivery still require qualification.

## All Runtime Policy Limits In Metadata

On 2026-10-15 the user directed: "All limits should be in metadata not in code. Either in agent identities or other metadata as appropriate. Global for defaults as appropriate. But all in metadata." This selects explicit metadata for operational policy bounds, global defaults, accepted agent overrides and durable per-task resolution. Existing numeric values are initial seed data, not new product choices. The earlier maximum finite model-request default remains. The native and supplied-executor paths share effective-limit visibility without granting agent tools authority to alter operator settings.

The implementation inventory exposed interacting context/schema/transport/journal bounds, a journal reader-only index cap, and fixed native single-call handling. Acceptance must exercise raising a value through its actual consumers, not merely moving constants into a file. The implementation also distinguishes policy from fixed representations and unknown provider limits. Exact replay, ownership, session exclusivity and protected authority are preserved. This records selected behavior; source integration, tests and deployed enforcement remain separate evidence in [the metadata-limits batch](../../../../borg-runtime/artifacts/v2/build-60156282/metadata-limits-v41pfogc/).

## Repository Catalogue Metadata Direction

The user asked: "Do we support one or many repositories? How are they configured? This should be driven by metadata hopefully and not statically configured." The preceding implementation supported multiple operational read-map entries but lacked durable registration identities, revision selection and catalogue UI/tool access. The catalogue direction is now specified in the main implementation requirements; the earlier proposal remains historical.

The selected source contract adds many durable registrations with one primary association per accepted task, operator catalogue mutations, safe browser/tool reads, immutable revision resolution and explicit source/checkpoint destinations. It uses the existing controller database and receipt path. Operator configuration records already provisioned locations; it does not acquire material, change credentials, grant access, hard-delete registrations, import startup maps or retarget accepted work. The normative and concrete interface definitions are in the main specification and repository-catalogue section of the protocol. Exact implementation and test results are recorded separately under the runtime repository. This recording does not change capability/stage/milestone adoption or execution readiness, grant host access, or establish deployed compatibility.

## Runtime Source Repository Selected

On 2026-10-15 the user directed: "I have set up a repository for borg runtime -- use mtoltonoai/borg-runtime". The selected runtime source and change-submission repository is the private GitHub repository `mtoltonoai/borg-runtime`. The initial read-only check found an empty repository and existing administrative access. The builder checkout uses that repository as `origin`; source, tests, tooling and build inputs belong there, while generated execution evidence, caches and credentials do not.

The shared and implementation specifications remain in the separate Borg specification repository. The deployed central Git mirror, existing repository registration revisions, accepted task endpoints and acknowledged checkpoints remain unchanged by this selection. Connecting new work and protected submission to the selected upstream uses the existing repository catalogue and exact-operation reconciliation rules. This repository choice does not create credentials or host access, grant agents protected release authority, authorize an unchecked merge, or replace the exact-artifact human authorization required for final rollout.

## Operational Settings Application Selected

On 2026-10-15, the operator selected incorporating the focused operational feedback path into v2 while existing bug fixes continue. The path applies supported configuration changes and then evaluates their observed effect; recording a recommendation or measurement alone does not satisfy the selection.

The [normative boundary](implementation.md#operational-feedback-applies-supported-configuration) and [wire contract](protocol.md#supported-definition-limit-application) use a canonical durable definition-limit update, expected selected/global/policy revisions, complete configuration validation, and an explicit allowed-field policy. A committed update selects an immutable definition for future tasks. Accepted tasks and configured children retain their exact settings. The initial supported field class changes only the eight execution-size/retention fields listed in the protocol; it changes no model, executor, tool capability, task model-budget policy, host control, credential or protected release authority. No new numeric ceiling or per-change human approval queue is introduced.

The application is a real state change with a durable outcome and observable selected revision. Existing task/history/usage records provide subsequent measurement tied to that outcome. Automatic failure-trigger scheduling is a following implementation increment, not a completed feature or a requirement to retry uncertain work. Startup-only global settings remain separate from applied agent revisions; recording desired metadata is not evidence of activation.

This records the focused scope addition, not implementation completion, deployment, a shared-spec amendment or adoption of later scale work. Existing capability and milestone approval maps remain unchanged. Validation and exact before/after provenance are retained in the runtime operational-feedback-v2-y0rhuwvl artifact.

## Maximum Context And Shipping Selected

On 2026-10-15 the user directed: "We should lift all context limits to max and start shipping changes on Borg". This selects the maximum supported context for the existing native and supplied-executor paths and delivery to the already authorized cluster. The [normative requirement](implementation.md#maximum-context-defaults) and [concrete context profile](protocol.md#maximum-context-profile) distinguish the selected 1050000-token model window from encoded byte-carrier ceilings, output budgets, deadlines and host controls. Existing model and effort selections remain unchanged.

The current twelve-field definition-update policy supersedes the earlier eight-field selection only by adding native request/helper/response carriers and the supplied-executor context window. Known numeric comparisons retain their meaning; an absent prior executor window is reported as unspecified. Future tasks use canonical selected-head updates, while accepted inputs and original outcomes remain historical. The wire10 forward-only disposition preserves retained state and requires actual source-bound compatibility checks. The original diagnostic settings mutation is already committed and is not resent to demonstrate the successor.

This direction authorizes the context changes and normal verified shipping work, not new credentials, host access, dependencies, model selection or expansion of protected release authority. Actual test, package, retained-copy and installed readback receipts are separate from source adoption. Existing capability, stage and milestone approval maps remain unchanged.

## Automatic Disposable Data Cleanup

On 2026-10-15 the user directed: "Yes. Fix the automatic cleanup in Borg as well. We should implement it in v2 as the spec allows". This authorizes automatic cleanup as a v2 implementation increment, using metadata rather than code-only retention limits. The initial implementation defaults reclaim safely completed workspaces immediately, retain known failed or cancelled workspaces for 24 hours, and expire unreferenced reconstructible local repository caches after seven idle days. These are the defaults chosen for this increment, not historical settings or a claim that the current installed package already cleans them.

The [normative cleanup boundary](implementation.md#automatic-cleanup-preserves-durable-work) retains active work, uncertain outcomes and unresolved publication/recovery dependencies. Failed or cancelled uncheckpointed edits may be discarded only after the configured interval and all holds clear. Historical external checkpoint references do not by themselves pin a reconstructible worker cache. Missing historical cleanup metadata grants no cleanup authority; explicit current worker metadata selects this local-copy policy without rewriting accepted task inputs.

Durable task results, history, audit metadata, checkpoint references, exact operation receipts and recovery journals remain retained in this increment. Their safe retirement and disk quotas remain separate work; the change does not claim bounded total database growth or the full shared retention implementation. The implementation and actual deletion tests are recorded separately from this source adoption. No new dependency, service, credential, access grant or protected rollout authority is selected. Existing capability, milestone and readiness status remains unchanged.

## Collaboration Workflow Expansion Queued

On 2026-10-16, Matthew directed: "Ok great. Let’s get that work queued up. All of it." This referred to all four functional batches in [Collaboration Workflows](proposals/collaboration-workflows.md): task coordination and resolver waits; questions and the complete supported decision queue; versioned documents and exact-content review; and durable resolution delivery with actual consumers. The [recorded queue](../../../../borg-runtime/artifacts/v2/build-60156282/metadata-limits-v41pfogc/operational-feedback-v2-y0rhuwvl/collaboration-work-queue-01.json) retains the existing owners and exact work-package source.

This selects those bounded implementation increments now. It supersedes the earlier task-first deferral only for the selected document, comment, question, answer, review, resolver-wait, decision-queue and resolution-delivery behavior. It does not adopt the complete shared collaboration outcome. Team routing, authenticated-person identity beyond the current host trust, imported answers, automatic document pruning, broad block editing, general subscriptions/person topics, lifecycle replacement, encryption, fine-grained permissions and the other recorded deferrals remain outside this expansion.

The execution plan expresses the selected work as six milestones: V2_COLLAB_CONTRACT for the integrator-owned common typed contract, actual shared record primitives and compatibility/catalogue bounds; V2_COLLAB_WORK, V2_COLLAB_QUESTIONS, V2_COLLAB_DOCUMENTS and V2_COLLAB_DELIVERY for the four parallel component batches; and V2_COLLAB_JOIN for all actual producers and consumers together. The four components can proceed behind the common contract; typed peer fixtures qualify their seams, not final delivery. The final join consumes the existing coding, native, browser, delegation and recovery paths, and is a prerequisite for the existing release review. Existing capability/stage/milestone selections retain their status; the later ten-host stage and milestone remain proposed.

The integrator retains shared protocol, schema/wire compatibility, router/catalogue, metadata limits and whole-plan ownership. The existing controller store, canonical receipts, task/session associations, immutable accepted inputs/prerequisites/profiles, deadline/accounting, cancellation and uncertain-effect rules remain. Memory notices remain non-triggering. Product questions or reviews confer no protected rollout or host/repository/provider/credential authority and add no default human gate to ordinary work. No new engine, inbox, credential, technology or host-access selection follows.

The source-bound review metadata records this selected implementation scope without declaring ownership paths, readiness, completed gates or behavioral conformance. The [native extraction and allocation packet](../../../../borg-runtime/artifacts/v2/collaboration-queue-native-mf17y4ic/README.md) preserves all 265 prior exact v2 identities and adds 69, for 334 planning associations. Native association completeness is not implementation/test evidence; the new behavior and integrated recovery require actual qualification. Shared outcomes, the historical runtime lock and its copied source, the missing gate baseline and final rollout authorization remain unchanged.

## Shared Charter Behavior Selected

On 2026-10-16 Matthew directed: "We should pull in the charter stuff from the shared spec." This selects reusable agent-definition charters, exact authoritative source and clause references, effective turn bindings, native and supplied-executor delivery, durable reply metadata and audit checking. It supersedes the earlier per-inner-reply charter citation/audit deferral. The original decision remains historical provenance; the initial shared compatibility mapping is not silently relabeled as implementation evidence.

The selected charter configuration path applies explicit pending changes at safe turn boundaries without rewriting accepted TaskInput, original definition, inference, tool profile, budget, limits or prior turn records. Native prompt changes use mid-conversation input and debounced low-priority compaction under the new charter. Supplied-executor delivery follows its pinned supported step boundary. This is a narrow charter configuration increment, not live adoption of every definition field or activation of the conditional external definition-source contract.

The fourteen shared charter-reference obligations are adopted without turning reference validity into semantic compliance. Invalid/mismatched references are retained with their replies for audit; no citation-driven feedback, retry, reset, blocking, compaction, admission policy or model judge is selected. Native compaction follows a charter configuration change independently of the audit result. Existing output validation and recovery/currentness rules remain in force. Streaming boundaries, empty applicability and charter-specific retention are still shared unspecified details requiring an explicit concrete representation before consumer qualification.

Four new milestones use the existing capabilities and stage: V2_CHARTER_RECORDS, V2_CHARTER_NATIVE, V2_CHARTER_CODEX and V2_CHARTER_JOIN. The release review consumes their actual joined evidence. Existing graph identities, readiness, deployment, dependencies, security profile and protected authority remain unchanged. This selects source/implementation work and ordinary qualification; it does not claim every pinned Codex inner model reply is observable or any native compaction/audit implementation already exists. The [charter incorporation plan](proposals/charter-incorporation.md) records exact source and acceptance associations. Shared Specification As A Product remains deferred. Other governance changes are owned by the separate user-approved governance proposal and are not inferred from these charter references.

## Focused Governance Additions Approved

On 2026-10-16, after the recommendation of four focused additions grounded in shared governance, Matthew answered "Ok. Do it." The selected work is completion and voluntary-stop checks for required work and pending applicable inputs with a concrete reason and bounded continuation; governed versioned configuration changes building on the existing limit-update path with effective before/after comparison and no self-weakening; immutable decision-rule versions with audit, shadow evaluation and evidence-bound promotion; and independent stop, pause and recovery with an explicitly expiring audited temporary override when ordinary policy is broken.

The selection requires implementation and actual acceptance evidence. It does not declare the existing narrow checks sufficient. The first configuration class remains the existing twelve-field limit operation, with accepted tasks unchanged. Rule/policy promotion and override are operator-controlled paths, not new agent fields. Programmable decider engines, model judges and elaborate decision caching remain deferred. The fine-grained permission-engine deferral, existing host trust, real protected-release human approval and mandatory validation remain. Neither the shared source nor its outcome/gate selection is amended or activated by this incorporation. The concurrent charter selection is recorded separately.

## Shared Retry Handling Selected

On 2026-10-16 UTC the user directed: "Ok it seems like we should implement the shared spec around retries with the obvious exceptions for codex sessions". This selects platform-owned retry handling from the shared failure table while keeping the supplied executor's internal model/tool retry behavior executor-owned. It does not authorize duplicate sessions, resending unresolved tools or treating local process cleanup as provider reconciliation.

The existing task allowance and deadline remain the outer bounds. No separate small retry cap or numeric timing/circuit default was selected. Timing, jitter, provider delay handling and coordinated model/endpoint circuit/authentication policy are explicit metadata. Adoption is distinct from implementation, controlled qualification, actual provider qualification and capacity qualification. The existing 429 and mixed-run failure evidence remains immutable.
