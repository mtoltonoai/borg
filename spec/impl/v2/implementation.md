# Implementation v2 Specification

> This review draft states the intended v2 profile. The capitalized requirement keywords have the constitution's meanings. Each keyword sentence states one requirement or permission under a stable heading. Descriptive choices and limits explain applicability; they do not establish execution, approval or conformance. The [compatibility review](compatibility-review.md) identifies proposed differences from the unchanged shared contract.

## Scope And Fixed Objectives

The working deployment profile retains one tenant with dedicated resources. Owned runtime code remains Rust; tooling and external dependencies keep their distinct roles. The [technology review](technology-review.md) records which prior selections are retained as recommendations, reconsidered, removed from the proposed initial scope or deferred. Exact new dependency profiles are not selected by this draft.

The authored delivery declarations are the [Implementation Capability Specification](capabilities.md) and [Phased Implementation Specification](execution-plan.md). They describe proposed work allocation, not execution permission. The [dependency profile](dependency-profile.md) and [wire-interface proposal](protocol.md) make the implementation review concrete; their exact source bytes are included in planning provenance. Shared amendments remain a separately reviewable, unapplied change.

### Observable End-To-End Result

The initial system MUST accept an authenticated task through its product interface.

The initial system MUST durably record an accepted task before reporting acceptance.

The initial system MUST execute a coding task in a workspace through the supplied executor.

The initial system MUST expose the task's actual execution status through its product interface.

The initial system MUST return references to the source changes and validation results produced by a successful task.

A task MUST NOT be reported successful solely because its executor exited successfully.

Success requires the task's declared acceptance checks to pass. A first demonstration covers a real repository edit, compile, relevant tests and submission of the resulting change. It does not require the implementation to build its own model harness, sandbox platform or backup system first.

### Shared Product State

The collaboration product MUST use the platform's task state and event interfaces.

The collaboration product MUST NOT create an independent agent inbox.

Task ownership MUST remain distinct from session and partition ownership.

The collaboration product MUST provide a browser user interface.

The browser interface MUST provide an agent list.

The browser interface MUST support task submission.

The browser interface MUST support task assignment.

The browser interface MUST show task dependencies.

The browser interface MUST show actual task status.

The browser interface MUST show reported task progress.

The browser interface MUST show available recent task output.

The browser interface MUST expose task cancellation.

The browser interface MUST support retrying a failed task.

The browser interface MUST present accepted task results.

The browser interface MUST link the task's published source checkpoints.

The browser interface MUST present the information exposed through the shared agent interface.

The browser interface MUST update displayed state without requiring a manual page reload.

The browser interface MUST display a live activity feed on the right side of the page.

The browser uses the same existing authority boundaries and canonical records as the shared tool interface. Information parity includes collaboration state, worker capacity, attempts, checkpoints, release checks and scoped repository reads. Agent-bound progress, memory and delegation outcomes remain visible without impersonating an active agent or granting protected control. Live refresh preserves unsent form input, distinguishes stale or disconnected views from committed state, and never retries a write merely because the view reconnects. The activity feed reads existing durable activity; it does not create a second event store.

The first product release provides an agent list alongside the task list and task detail views for submission, assignment, dependencies, status, progress/output, cancellation, retry and code/result references. Agent rows identify the controller's known agent identities, their immutable definition revisions and concurrent sessions with task links. Session rows identify the agent they instantiate; shared memory is shown under that agent. Agent identities are not worker hosts. The product uses the controller's existing application interface and durable records; no separate product store or browser-framework choice is introduced. Available progress/output is bounded task-level information, not a complete replayable transcript. The initial controller-worker smoke test can run without the browser, but the collaboration product is not complete with only a command-line client. The selected collaboration increment adds versioned documents, immutable comments, direct-recipient questions and reviews, a decision queue, typed resolver waits and committed-resolution delivery. The supported workflows appear in both the browser and shared agent interface. This is delivery scope, not a claim that the increment implements the complete shared collaboration outcome.

### Shared Collaboration Tool Interface

The collaboration tool interface MUST expose the existing task-space reads to both executors.

The collaboration tool interface MUST expose configured task-space mutations under their current operation rules.

The collaboration tool interface MUST use the controller's canonical operation layer.

The collaboration tool interface MUST provide read-only views of worker capacity, task attempts, checkpoints and release-check records.

Operational queries MUST NOT grant authority to mutate worker controls or protected release state.

Collaboration tool responses MUST exclude credential values.

The collaboration tool interface MUST validate its caller's current attempt before a new mutation.

The common interface is MCP for native and Codex tasks. It is layered over the existing controller API and operation handler, which remain the single durable owner of task transitions, mutation receipts and reconciliation. Existing task, agent, history, explicit-memory, result and checkpoint records are reused; operational views are projections of existing records rather than a second metadata service or store. Agent use of this interface does not replace the worker/control-plane API.

The initial space is the existing trusted one-tenant task product. No project identifier, private-project boundary or fine-grained permission engine is inferred. Caller task/attempt binding is checked by the adapter and controller, but a caller-supplied identity is not authentication; the underlying ordinary API still relies on host/SSH trust. This interface does not establish containment of code that can reach that API directly. Existing collaboration operations retain their revision, ownership, accepted-input and lifecycle restrictions. Configured child creation preserves its template, parent association and accounting rather than becoming unrestricted task submission. Versioned documents, comments, direct-recipient questions and reviews, the supported decision queue, resolver waits and committed-resolution delivery extend this same operation layer. General team routing, imported answers and broader collaboration facilities remain outside this increment.

### Collaboration Records Have Stable Identity And Provenance

The supported records are documents, document versions, comments, questions, answers, reviews and task collaboration waits. They use the existing controller database, application schema discipline, limits metadata, mutation receipts and canonical operation layer. A collaboration revision orders product changes and is distinct from the controller's operation or receipt revision. Activities carry their actual typed record subject; a document or question does not require a fabricated task.

Recipients are direct trusted-host operator or existing agent identities. The controller derives actor context from the host-controlled product path or the current agent task/attempt path. This records the origin the existing trust boundary can establish; it does not independently authenticate a human, create a principal registry or protect against code sharing that host access. Team recipients, imported answers and person attestations remain unsupported. Existing currentness rules apply to agent mutations.

Every collaboration record MUST have a durable identity.

A committed collaboration change MUST receive the next collaboration revision in commit order.

A collaboration mutation MUST commit its record change and corresponding activity atomically.

A collaboration change activity MUST identify its typed record subject.

The controller MUST derive a collaboration mutation's actor provenance from its invocation context.

An application-supplied actor label MUST NOT be recorded as authenticated-human identity.

A mutation of an existing collaboration record MUST compare the expected record revision.

A collaboration request MUST refuse unsupported recipient kinds.

### Document Versions And Comments Are Immutable

A document has an immutable version history and a current-version reference. Creating a version is separate from requesting review. The controller computes the content identity from the accepted version content. This increment retains version history; pruning, archive automation, a general block editor and composing definitions from approved documents remain outside its scope. Document content uses the selected v2 storage/security profile and does not acquire application-key encryption guarantees.

A committed document version MUST be immutable.

A document version number MUST NOT be reused.

The controller MUST refuse a supplied document content identity that differs from its computed content identity.

A posted collaboration comment MUST be immutable.

A document edit MUST create a new version without changing an earlier version.

The initial collaboration profile MUST retain committed document versions.

### Questions Preserve Their Posed Content And Outcomes

A question records its poser, one direct recipient, current responsible record owner, prompt, recommended option and typed decision fields. The responsible owner is a collaboration role, separate from the immutable accepted execution agent, definition and session. Questions survive the end of an asking attempt. Answer replacement, withdrawal and moot state are new recorded transitions referencing the original immutable answer; they are not edits to its recorded content. These product records do not establish authenticated-person authorship beyond the stated host-trust provenance.

A posted question's prompt, options and typed decision fields MUST be immutable.

A recorded answer MUST be immutable.

A question answer MUST match the direct recipient recorded on the question to the caller context established by the controller.

An answer replacement or withdrawal MUST be accepted only from the question's recorded direct recipient.

An answer replacement or withdrawal MUST retain the original answer record.

An open question MUST remain cancellable or supersedable by its current responsible record owner when the asker is inactive.

A question cancellation or supersession MUST record its acting caller context and reason.

A question cancellation or supersession MUST preserve the original poser.

A replacement question MUST record the acting poser and the question it supersedes.

An answered question MUST NOT be cancelled or superseded.

A record-owner change MUST preserve its open questions.

A blocking question MUST take no default answer.

A question's prompt MUST state the recommended option.

A question routed to the trusted-host operator MUST ask exactly one decision.

A moot mark MUST record its acting caller context and the answer that made the decision moot.

A moot mark MUST remove the marked answer's product effect.

A moot mark MUST NOT create a product decision effect.

### Reviews Bind The Exact Subject Version

A review identifies one immutable document version or exact accepted task result/checkpoint, together with its direct reviewer. Its decision records the actual reviewer context. An operator-directed review is labeled with trusted-host provenance, not independent proof of a human identity. Creating a new document version does not alter the subject of an existing review. Standalone reviews use explicit subjects and terminal outcomes; they do not introduce a general policy language.

A review MUST bind the exact immutable subject version or result reference it evaluates.

A review decision MUST match its recorded direct reviewer to the caller context established by the controller.

A review decision MUST retain the reviewed subject identity.

A recorded review decision MUST be immutable.

A new document version MUST NOT inherit the decision of an earlier version's review.

Withdrawing a review MUST record its acting caller context and reason.

A review withdrawal MUST publish committed activity identifying the affected review.

### The Supported Decision Queue Is Complete

The queue is a projection of the canonical question and review records, not another delivery store. It includes direct questions and reviews owed to its viewer under the existing trust boundary. Team resolution and imported answers are outside the supported recipient set. Completed decisions remain inspectable in record history.

The decision queue MUST include every open question directly routed to its viewer.

The decision queue MUST include every open review directly routed to its viewer.

A bounded decision-queue page MUST provide a continuation when more matching decisions remain.

A notice outside the decision queue MUST NOT substitute for an owed decision's queue entry.

### Collaboration Waits Are Separate From Execution Prerequisites

Accepted execution prerequisites remain immutable and success-gated. Collaboration waits are separately recorded, mutable workflow relationships to concrete task, question or review records. Scheduler ineligibility from capacity, unsuccessful accepted prerequisites or unresolved execution is distinct from a collaboration wait. A wait records the prior logical workflow state and assignee; ending it does not restore a stopped execution attempt. Terminal cancellation or completion wins over later wait resolution. A negative resolver outcome ends the wait and is delivered as that outcome, not as task success or permission to act.

A collaboration wait MUST identify a concrete task, question or review resolver.

A wait on an agent, team or external party MUST be refused.

A wait on a resolver that is already closed MUST be refused.

A wait that would close a cycle through waits and parent links MUST be refused.

A task with an open collaboration wait MUST be ineligible for a new execution attempt.

The first open collaboration wait MUST retain the task's prior workflow state and assignee.

A requested collaboration wait MUST be committed before initiating the current attempt's quiescence.

A task suspended on a collaboration wait MUST release its execution slot after its current execution has stopped.

A resolver's terminal transition MUST commit its typed outcome, affected wait resolutions and delivery associations atomically.

Ending the last collaboration wait on a nonterminal task MUST restore its recorded workflow state and assignee.

A resolved wait MUST retain the exact terminal outcome occurrence that resolved it.

An unblock activity MUST carry the typed outcome of each wait it ends.

### Committed Resolutions Reach Their Consumers

Resolution delivery uses the existing controller state and platform wake/command mechanism. It is distinct from the non-triggering memory notices. The recorded destination identifies an existing task/session or the responsible direct recipient; delivery never creates a new session or implicit follow-up task. Agent input arrives at a supported safe model/tool boundary. A waiting task resumes only after all waits end and the unchanged execution admission, original deadline/budget, cancellation and uncertain-effect rules permit it. The current recipient keeps unresolved decisions visible when no active session can consume them. General record-topic fanout, arbitrary subscriptions and person-notification topics remain deferred.

An answer delivery MUST target its active asker or otherwise the record's current responsible owner.

A committed resolution delivery MUST carry an identity that deduplicates its delivery retries.

Pending resolution delivery MUST remain recoverable after a controller restart.

A committed resolution that makes a waiting task eligible MUST wake it through the existing platform command mechanism.

A resolution notice MUST be delivered to an active executor at a supported safe input boundary.

A resolution notice MUST NOT cancel a running side-effecting tool.

A resolution-delivery acknowledgment MUST follow durable retention of the outcome for its receiving continuation.

A repeated resolution delivery MUST NOT start a second continuation for the same resolution occurrence.

A resolution notice MUST NOT reopen a terminal task.

### Collaboration Surfaces Preserve Existing Authority

Questions and reviews are optional collaboration workflows. They do not require human review of ordinary editing, compilation, testing, submission or task execution. Explicit waits may suspend work for an answer, and every answer outcome ends its waits; resumption is not an authority grant. Automatic task-start go-ahead controls, descendant approval gates, team membership, signed person attestations, imports and declarative definition composition remain outside this increment. Protected final rollout keeps its existing artifact/target authorization and mandatory checks.

The browser and both executor tool interfaces expose the same supported records and decisions through the canonical operation layer, subject to their existing caller restrictions. Agent callers receive no trusted-host operator or protected-release authority. New operations require an explicitly configured accepted tool profile; existing tasks are not silently widened. Existing schema/migration handling, exact receipt replay and limit metadata apply to every added record and operation.

A product question or review decision MUST NOT confer protected rollout authority.

A product question or review decision MUST NOT grant host, repository, provider or credential access.

The collaboration expansion MUST NOT require human approval of ordinary work that has no explicitly requested product wait.

The browser interface MUST expose every supported collaboration operation permitted to its caller.

The shared agent interface MUST expose every supported collaboration operation permitted to its current caller.

Adding collaboration operations MUST NOT expand an already accepted task's configured tool profile.

### Repository Reads Through The Shared Tool Interface

The shared agent interface MUST provide read-only repository access within the task's accepted repository scope.

A repository read result MUST identify its exact source revision.

An agent repository-read operation MUST NOT execute repository-supplied code.

An agent repository-read operation MUST NOT modify repository content.

The shared MCP catalogue provides bounded directory listing, file reading, literal text search, commit history and diffs over exact committed source. It resolves repository identities and revisions from accepted task configuration or acknowledged checkpoints, not an arbitrary host path or URL supplied by the model. Native and Codex tasks use the same read semantics. Source content is untrusted tool data. Missing source is reported as unavailable; reading does not acquire credentials, fetch an unconfigured repository, execute hooks or create a writable workspace. Editing, builds and arbitrary repository commands remain coding-executor work.

### Repository Associations Are Versioned Metadata

The runtime MUST support multiple repository registrations described by metadata.

A repository registration MUST have a stable repository identity.

A repository registration revision MUST be immutable.

An operator repository-catalogue mutation MUST use the canonical durable controller operation path.

A repository-catalogue update MUST compare the current repository head generation supplied by the operator.

An accepted task MUST retain its selected repository registration revision.

A task MUST have at most one primary repository association.

An accepted repository association MUST distinguish the source endpoint from the checkpoint destination.

A newly resolved repository association MUST reject a repository registration disabled for new tasks.

A catalogue revision or enable-state change MUST NOT change an accepted task's repository association.

A configured child repository association MUST be resolved when its parent workflow is accepted.

A checkpoint publication MUST identify its accepted repository registration revision.

The shared agent interface MUST provide bounded repository-catalogue list and get operations.

Agent tools MUST NOT mutate repository registrations.

A repository registration operation MUST NOT acquire repository material.

A repository registration operation MUST NOT grant repository access.

A model-visible repository projection MUST NOT expose controller storage keys.

The catalogue uses the existing controller store and its exact mutation receipts. Operator registration, revision and enable-state changes record configuration for already provisioned resources. The head generation changes for revisions and enable toggles; immutable registration revisions retain their original endpoints and store keys. Disabling affects new admission, including newly submitted workflow templates, while accepted tasks and their already resolved configured children retain their exact bindings. Each coding task has one primary association; a native task can instead have one explicit read association. Cross-repository atomic checkpoints remain outside this profile.

The browser and shared agent tools read the same safe catalogue projections. Operator forms select a registration identity and exact current revision when creating a task. Endpoint projections are computed by the controller, not supplied by a model or accepted from external task submissions. Controller reads resolve a private store key beneath one trusted operational root, with no fetch or implicit import. Worker caches bind the accepted registration revision and both endpoint identities; immutable revisions do not relabel existing caches or retarget published checkpoints.

### Initial Scope Is The Task Product

The initial release excludes general topic subscriptions/fanout, registered webhooks and recurring user timers; distributed keyed-session and partition placement; full replayable provider-neutral transcripts, tail/upload/attachment services; general automatic experiments and learning/observation pipelines beyond the focused operational feedback path; and collaboration archive/dwell/overdue automation, external ingestion, person-notification topics and operator-recovery entry workflows beyond the focused operational feedback path. These facilities remain deferred. Versioned documents, comments, direct-recipient questions and reviews, the supported decision queue, typed resolver waits and committed-resolution delivery are included without selecting general publish/subscribe or person-notification topics. Task assignment, progress, completion, worker command wakes, execution deadlines, accepted results, explicit agent memory, source/definition provenance and observed usage remain in scope.

Definitions are immutable accepted task inputs. The selected charter path separately records explicit pending and effective turn bindings without rewriting those inputs. Limit defaults and agent-specific overrides are resolved at admission as described in Limits Are Versioned Metadata; this does not introduce live policy inheritance or keyed-instance reconciliation. Scheduling uses bounded oldest-eligible admission rather than inherited priority classes, dynamic quota rebalance or provider batch routing. Ordinary task data has an explicit application schema version and deliberate migration handling, without inheriting a permanent tagged-envelope or forever-backward-readable guarantee. Encryption, key/secret services, fine-grained application permissions, hostile-workload containment, automatic workspace snapshots, controller failover and initial backups remain outside this profile as specified below.

## Agent Charters And Reply References

This increment adopts the shared charter-reference requirements and the charter-specific configuration behavior below. A charter is the reusable identity and purpose portion of an agent definition; a task instruction supplies the work for one task. The charter's exact authoritative source remains distinct from model input rendered from it and from any compacted summary. Existing immutable definition or document-version identities are reused when they resolve the complete source; this does not select a new definition-source service, specification-governance product, credential boundary or permission engine.

### Definitions Carry Reusable Charters

An agent definition MUST identify its reusable charter source.

The native executor MUST deliver the effective charter to its model.

The supplied executor MUST deliver the effective charter to its model.

A session MUST retain the charter revision effective at each turn boundary.

The accepted TaskInput and its original definition revision stay immutable. The initial charter binding comes from that definition. A later explicitly accepted charter change has a separate pending/effective session binding, with its original operation identity and exact source revision; it is not a rewrite of accepted inference, executor, tool profile, task budget or limits. Selecting a new future-task head alone does not change an existing turn. Historical tasks and replies without charter data remain historical rather than receiving invented citations.

### Charter Changes Apply At Safe Boundaries

A charter change MUST apply to a running session at its next turn boundary.

A charter change MUST NOT apply between a tool call and the delivery of its result.

A session MUST check for a pending charter change when it loads.

A charter change MUST NOT wake an idle session solely to adopt it.

A charter change MUST reach the supplied executor only at a supported step boundary.

The turn boundary is before the next model request is built, after every result of the preceding turn has committed. A supplied-executor step boundary has neither a model call nor a tool call in flight. Publication, adoption and delivery are distinct durable facts; a desired source revision is not effective merely because it is stored. Recovery preserves the prior effective revision until the pending transition is safely adopted and does not resend an uncertain model request or tool effect. The configuration path retains existing caller authority and canonical mutation receipts; the future-only limit-update tool does not acquire charter-writing authority.

### Native Charter Changes Preserve Prompt Continuity

A native charter change MUST be applied as a mid-conversation instruction.

A native charter change MUST schedule a low-priority compaction that merges the change into the system prompt.

Native charter changes MUST be debounced, so that a burst of them costs one compaction.

A native compaction that follows a charter change MUST write its summary under the new charter.

A native charter change arriving while a compaction is already due MUST be included in that compaction.

This is the separately selected response to a native charter configuration change, not a consequence of a missing or invalid reply citation. The selected native executor/provider compaction path still needs implementation and qualification under configured bounds. It does not select a general transcript, blob service, policy engine or an independent supplied-executor compactor. A summary or prompt-cache representation never becomes authoritative charter source. Pinned supplied-executor delivery and reply observability require actual adapter qualification; a startup prompt or final result alone does not prove every turn.

### A Charter Has Normative Requirements

A charter's behavioral requirements MUST use the normative key words with the meanings defined by the constitution.

Each charter requirement MUST be a self-contained sentence carrying one obligation under a stable heading.

### A Revision Identifies Authoritative Source

A charter revision reference MUST resolve to the exact authoritative source of that revision under the applicable access and retention rules.

A charter revision reference MUST reuse an existing source identity when that identity resolves the complete authoritative charter source.

A rendered or compacted representation of a charter MUST NOT replace that charter's authoritative source.

### Every Turn Reply Carries Charter References

Every turn reply MUST carry structured charter-reference metadata.

A turn reply's charter-reference metadata MUST identify the authoritative charter revision effective at that turn's boundary.

A turn reply's charter-reference metadata MUST identify the specific charter clauses governing its response or actions.

A charter clause reference MUST resolve to the clause's source file, section, and exact sentence within the identified charter revision.

A turn reply's durable record MUST retain its charter-reference metadata.

A record that cites a turn reply for audit inspection MUST preserve its association with that reply's charter-reference metadata.

### Reference Checks Produce Audit Information

The platform MUST check each turn reply's charter references against the authoritative charter revision effective for that turn.

The platform MUST record invalid or mismatched charter references with the reply they came from for audit inspection.

A charter reference check MUST NOT be treated as proof of behavioral compliance.

These checks establish reference validity, not semantic compliance. This section requires audit recording and does not itself require corrective feedback, retry, reset, compaction, admission blocking, or a model judge. It does not prohibit separately authorized policy from consuming the audit records. Existing configuration adoption, output governance, and compaction requirements continue to apply. Streaming boundaries, empty applicability, and charter-specific retention rules remain unspecified by this section.

## One Agent Model And Two Executors

### Definitions Select Execution

An agent's behavior MUST be described by configuration.

An agent definition MUST identify the executor used for its tasks.

An agent definition MUST specify the model used by its executor.

An agent definition MUST specify the reasoning effort used by its executor.

An execution attempt MUST use the inference settings of its accepted definition revision.

An execution attempt MUST bind the definition revision it uses.

An agent identity MUST persist independently of any executor process.

An agent identity MUST be the stable identity of its definition.

Every instantiation of an agent MUST have a distinct session identity.

Every session MUST carry the agent identity it runs as.

An agent identity MUST support multiple concurrent sessions.

A session MUST have at most one active execution attempt at a time.

Recovery of a session MUST retain its session identity.

A session MUST retain its own execution state.

A memory MUST be keyed by agent identity rather than by session.

An agent's memories MUST persist across every session of that agent identity.

A task MUST retain the definition revision accepted at submission.

Executor choice does not create a different agent identity type. The execution choices are a bounded native model worker and Codex for extended tool-driven work. Both use the selected model, gpt-6-astra, with reasoning effort max, subject to actual access and configured limits. The immutable template revision carries both settings. The native request and Codex task settings use that accepted selection; a worker-wide default does not override it and an executor does not silently substitute another model or effort. Every newly accepted session binds explicit settings; a worker default does not supply missing configuration.

The template is the agent's durable identity. Its stable definition identity is independent of its immutable configuration revisions. A session is an instantiation of that agent, not a new agent or a new memory namespace. Sessions of one agent can execute concurrently with their own task associations, attempts, workspace or executor context, progress and results. They share the agent's explicitly recorded memory. A task identifies work; a session identifies its agent instantiation; an attempt identifies one fenced execution of that work. Submission replay, retry, recovery and continuation preserve the recorded session association instead of creating another session implicitly. New instantiations receive new session identities. Changing a definition creates a revision for future instantiations without changing the accepted revision of an existing session. Shared memory writes retain current-session authority and expected-memory-revision checks; a conflicting update requires a fresh read and a new explicit operation rather than silently overwriting another session's memory.

### Shared Memory Changes Reach Sessions

A committed memory update MUST make its changed key and committed revision available to other active sessions of the same agent.

A session MUST receive pending memory-change notices at its next safe model or tool boundary.

Pending memory-change notices MUST remain recoverable after a process restart.

Memory-change notification delivery MUST NOT interrupt a tool that has begun execution.

Memory-change notices MUST NOT create a session.

A memory-change notice MUST NOT initiate a model request by itself.

Pending notices for the same memory key MAY be coalesced to the latest committed revision.

A notice identifies a changed key and revision; the receiving session reads current memory when it needs the value. The existing agent memory scope, current-session checks and expected-revision write conflicts remain. A session does not receive a notification caused by its own write. Multiple sessions can receive the same change without sharing their conversations or execution state. Native and supplied-executor sessions use the same committed change source.

Delivery uses the existing durable controller state and wake mechanism. A bounded cursor and pending batch survive restart; a lost wake or uncertain delivery acknowledgment does not silently advance past an undelivered change. Repeated notification of the same revision is harmless and does not authorize repeating a mutation. Newly started sessions use the existing memory reads for prior values; the durable notification start point covers subsequent updates. Notices enter an active executor at its supported safe input boundary without interrupting work, creating a new task or session, or calling a model for each write. A completed or idle execution is not restarted merely because memory changed. Automatic observation-derived memory and a general subscription system remain deferred.

### The Native Executor Is Bounded

A native task MUST declare a finite model-request budget.

A native task MUST declare its output schema.

The native executor MUST validate a result against that schema before accepting it.

The native executor MUST terminate its attempt when the task's execution deadline is reached.

The native executor MUST support a bounded direct tool invocation followed by a model continuation.

A native tool invocation MUST be restricted to the task's configured tool interface.

A native model continuation MUST consume the actual result of its preceding tool invocation.

The native path performs classification, routing, summarization and structured analysis using a bounded model/tool loop. Its shared collaboration tools and profile-selected provider tools do not introduce a general shell, workspace editing, a second compaction system or an interactive coding harness. Task and attempt bounds do not limit the lifetime of an agent identity or session.

The worker binds collaboration calls to the calling task and current attempt. Model arguments cannot select the caller, controller endpoint, credential or protected authority. Each model request consumes the existing finite accounting and each final result passes the original output schema and controller acceptance checks. The earlier two-request current-task inspection is historical bounded qualification, not the complete collaboration interface.

### Native Tool Profiles And Continuations

The native executor MUST support repeated configured tool calls within the task's finite execution bounds.

An agent definition MUST explicitly configure its native provider-tool capabilities.

The native executor MUST refuse a provider tool that is absent from the task's accepted configuration.

The platform MUST NOT switch a task's executor automatically after a model or tool failure.

An execution attempt MUST retain a tool call's exact argument binding before sending a mutation.

A native model continuation MUST use the recorded outcome of its corresponding tool call.

Borg acts as the MCP client for native work. It translates the shared catalogue into Responses function tools, executes the returned calls against the same collaboration MCP interface used by Codex, and supplies the actual recorded function results on continuation. Provider-hosted web search is a separate, explicitly enabled profile capability; an absent provider-tool selection enables none. Other hosted tools, connectors and arbitrary remote MCP servers are not selected. The model remains the existing selected model. Configured coding-child delegation is an explicit task handoff, never a fallback on native failure.

Finite request allowance, deadline, bounded context, argument/result size and controller capacity apply throughout. A lost model response remains an uncertain provider outcome; it does not authorize another request. A lost collaboration mutation acknowledgment is reconciled under its original operation identity. Cancellation does not undo a committed collaboration mutation. Historical task inputs and receipts are not silently rewritten to acquire the new profile.

### Limits Are Versioned Metadata

Every configurable runtime limit MUST be supplied by metadata rather than a policy constant in executable code.

Global metadata MUST provide explicit defaults for limits without a more specific configured value.

An agent definition MUST carry the execution-limit overrides specific to that agent revision.

An accepted task MUST retain its resolved execution-limit values.

An accepted task MUST identify the metadata revisions used to resolve its execution limits.

Changing default metadata MUST NOT silently change the limits of an already accepted task.

Recovery MUST use the execution limits retained by the task being recovered.

A component MUST validate its configured limits before admitting work governed by those limits.

An invalid or inconsistent limit configuration MUST be reported without silently clamping its values.

A component MUST NOT impose an undisclosed policy cap below the configured limit.

An operation refused by a configured limit MUST identify the limiting metadata field.

The shared agent interface MUST expose the effective limits applicable to its caller.

The browser interface MUST expose the effective limits applicable to displayed work.

The runtime MUST distinguish configured policy limits from non-configurable representation and external-service constraints.

A retained operation MUST NOT be discarded solely because a later limit configuration is lower.

A worker MUST verify the configured host controls before claiming that those controls are enforced.

Global defaults cover controller admission, transport, storage inventories, repository queries, product presentation and worker operation. Agent revisions can specialize native-model/tool and supplied-executor bounds; explicit task limits retain their existing meaning. Resolution is global defaults followed by the accepted agent revision and the task's explicit values. The accepted values and their source revisions are durable data, not a live lookup during replay. Host resource profiles remain operator metadata; changing a value does not create a kernel control or a new access grant.

The metadata catalogue records each unit, scope, default source and supported representation. Policy limits include item and byte counts, concurrency, tool-call counts, deadlines, timeouts, retry/backoff intervals, parser/work bounds and output retention. Fixed protocol identities, digest widths and machine or dependency representation limits are declared as read-only constraints rather than falsely presented as adjustable policy. Provider-account limits or supplied-executor internal limits that are not observable remain explicitly unknown. Ownership fencing, exact operation replay, one active attempt per session and protected authority remain semantic rules, not editable quotas.

Startup metadata is available before registration, parser admission and orphan recovery. Existing journals retain their applicable policy binding; a new configuration cannot reinterpret an uncertain operation as absent or authorize a repeated effect. Cross-field validation covers the complete path, including schema, transport, result and journal bounds. Raising a policy limit requires no replacement constant in another component, while unavoidable external constraints remain visible. Configuration changes and their qualification evidence are recorded separately from claims of deployed enforcement.

### Maximum Context Defaults

Default context limits MUST use the maximum supported by the selected model and executor.

The selected model context window is 1050000 tokens. The supplied executor receives that explicit window through its per-task configuration. Native context and the request, helper-value and response-wire byte carriers use the maximum of their existing unsigned32-bit framing representation, 4294967295 bytes. These byte ceilings are not token counts or a promise that a model accepts that many bytes. Buffers grow with actual received data; selecting a ceiling does not reserve that amount of memory. Model output, task request allowances, deadlines and host resource controls retain their separate meanings.

New defaults apply at admission. Existing agent heads with complete group overrides require a canonical validated revision change before future tasks use the new context values. Accepted tasks, retained operations and historical executor defaults remain unchanged. Missing historical executor-window metadata means unspecified, not zero or a known maximum. Source configuration, effective executor readback and actual workload qualification are separate evidence.

### Operational Feedback Applies Supported Configuration

The shared agent interface MUST support diagnosis of configuration refusals using existing bounded operational records.

The shared agent interface MUST expose supported definition-limit changes through the canonical controller operation layer.

A definition-limit change MUST compare the expected selected definition revision before applying a new immutable revision.

A definition-limit change MUST compare the expected active global metadata and change-policy revisions before application.

A definition-limit change MUST be restricted to the fields allowed by the configured change policy.

A definition-limit change MUST validate the complete resulting effective configuration before commit.

A permitted definition-limit change MUST atomically commit its immutable definition, selected head and durable operation outcome.

A definition-limit change MUST preserve the executor, inference settings and tool capabilities of its predecessor.

Submitting an explicitly pinned definition MUST NOT change an existing agent's selected definition revision.

A configuration change outcome MUST distinguish future-task selection from settings retained by already accepted tasks.

Evaluation of a configuration change MUST identify its committed change outcome and the definition revisions of the observed work.

A definition-limit change MUST NOT grant host administration, credentials or protected release authority.

The first path uses the existing task, definition, agent, usage and operation records. An ordinary agent task reads a failure and its effective settings, submits a closed revision change, and reads the committed outcome and subsequent task evidence. The controller performs deterministic validation; the model does not approve its own request. One applying operation is sufficient: there is no separate proposal database or per-change human approval queue. Existing ownership, exact receipt replay and unknown-operation reconciliation remain in force.

The initial change class is explicitly selected in versioned metadata. It contains only the protocol's listed execution-limit fields and adds no numeric ceiling below their existing validation, representation and external constraints. Increasing a known allowed resource limit is recorded as widening; unchanged known values are neutral and decreasing only these known resource allowances is narrowing. A change from an unspecified executor context window records that the prior value is unspecified, rather than inventing a numeric comparison or implying that other fields did not widen. This bounded class permission does not authorize changing policy, model, executor, capabilities, task budgets, host controls or protected checks. Global and worker startup settings are not hot-reloaded by this operation. Future submissions that select the new head use the new revision; already accepted tasks and configured child inputs retain their exact original snapshots.

General automatic failure-trigger scheduling and experiments remain separate increments. A proposed automatic diagnostic trigger would deduplicate one committed terminal failure under a stable origin and suppress diagnostic descendants, while using ordinary task admission and finite task bounds. It would not retry the original failed or uncertain operation. No such scheduler is claimed by the definition-write path.

### Initial Model Budget Policy

The default task model-request allowance MUST be the maximum finite value supported by the task representation.

Fine-grained spending, token and supplied-executor inner-request budget enforcement are deferred beyond the initial release. Default submissions use the maximum existing allowance. This does not enlarge the configured native workflow, make unknown executor usage known, or promise unlimited provider capacity. Observed usage remains visible without treating observation as enforcement.

Existing native request debits and configured child-task reservations remain finite, durable accounting. Delegated allowances fit within their parent's remaining representation, including any required continuation. Accepted task inputs and explicitly configured lower limits remain unchanged; no stored task is rewritten to adopt the default. Deadlines, cancellation, output/result bounds, ownership, process cleanup, concurrency and actual host/provider limits remain applicable.

### The Supplied Executor Owns Its Inner Loop

The supplied executor adapter MUST use the documented interface of its pinned executor release.

The supplied executor release MUST be pinned.

The adapter MUST record the supplied executor's reported outcome separately from the platform's task acceptance result.

An unavailable executor history MUST NOT be reported as a recovered conversation.

The platform MUST keep accepted task results independently of the supplied executor's process.

The integration contract is task-level. Codex owns its inner request construction, tool sequencing and context management. Progress can be streamed during a task without making each internal action a committed platform transition. The initial integration does not require reconstruction of every internal step or full visibility of every internal model attempt.

The selected interface is Codex app-server over local stdio, with a small worker-side adapter. The exact release and its lifecycle, event, interruption and recovery behavior need qualification before use. Experimental interfaces are not assumed stable because they appear in documentation. No remote app-server transport, fork of its inner loop or production-support guarantee is selected by this interface choice. Process-to-session mapping remains an implementation detail to qualify against the actual resource envelope.

### Platform-Owned Model Retries

A platform-owned model request rejected as transient MUST be retried with backoff and jitter within the task's remaining model-request allowance and execution deadline.

A retry of a throttled platform-owned model request MUST honor a valid provider retry-after value.

Every platform-owned model request dispatched by a retry MUST have an acknowledged durable request reservation before dispatch.

A platform-owned retry MUST revalidate its current task, attempt, ownership and cancellation state before dispatch.

Retry timing, jitter and circuit-breaker bounds MUST come from versioned metadata.

The platform MUST maintain one coordinated circuit breaker for each configured model and endpoint.

A circuit-breaker probe outcome MUST be settled against its exact original request identity and probe generation.

A platform-owned model request rejected as invalid MUST NOT be retried.

A context-window rejection MUST NOT be retried with unchanged input.

A provider policy refusal MUST be reported as a policy block.

A provider quota exhaustion MUST be distinguished from transient throttling.

A provider authentication failure MUST be coordinated across workers for the same dependency and credential-configuration identity with metadata-controlled backoff and a retained alert.

Exhaustion of the retry allowance or deadline MUST leave an explicit reason-bearing blocked state without another provider dispatch.

A failed platform-owned provider request MUST retain bounded diagnostics sufficient to identify the observed failure class and available provider retry or quota information.

Retained provider diagnostics MUST exclude configured credentials and submitted prompt content.

A local transport close or process cleanup MUST NOT establish that an ambiguous provider request had no effect.

A platform-owned retry of an uncertain provider effect MUST use the receiving interface's supported outcome reconciliation or deduplication before resending that effect.

Each newly dispatched platform-owned model attempt MUST carry a distinct retained request-attempt identity.

A complete model reply that violates its declared output format MUST be retried with the specific validation error within the remaining request allowance and deadline.

A complete validated provider terminal response identifying an output-cap truncation MUST be continued from retained output within the remaining request allowance and deadline.

Retries internal to a supplied executor MUST remain owned by that executor.

Platform recovery of a supplied executor MUST NOT create another session or repeat an uncertain start merely to implement a provider retry.

Every transition into a retry-related blocked state MUST publish its reason as a durable event.

Every retry-related blocked state MUST have an explicit canonical exit transition available through the supported adapter or control interface.

A new circuit-breaker probe after a prior unknown outcome MUST NOT replay the original uncertain request.

A platform-owned idempotent tool call that is unreachable or timed out MUST be retried within its accepted budget and deadline.

A transient failure of a platform-owned durable-store transaction MUST be retried with metadata-controlled backoff while retaining the original operation identity.

This increment adopts the selected shared model failure handling for requests that the platform owns. The accepted task's finite model-request allowance and original deadline are the controlling retry bounds; it does not select a separate small retry-count default. Backoff, jitter, circuit thresholds, open intervals and diagnostic bounds are metadata. A provider delay longer than the remaining deadline ends blocked rather than being shortened. Every physical POST keeps its charge even if rejected or its usage is unknown. A complete throttling rejection and an ambiguous timeout or lost reply are different cases. Invalid requests, context limits, policy refusals, exhausted quota and authentication failures have distinct dispositions; unknown provider effects are not converted to rejection by elapsed time or local cleanup.

The coordinated breaker and authentication state use the existing controller transaction and receipt owner. Breakers are keyed by the exact configured model and endpoint. A process-local worker breaker is not evidence of cluster-wide coordination. Original operation receipts, current caller binding and probe generation fence lost acknowledgments, concurrent half-open probes and stale completions. Recording an unknown probe can start a metadata cooldown; after that cooldown a new independent request can probe the destination without declaring the old request absent or replaying its effect. The original unknown outcome remains unresolved. Authentication suppression is keyed by dependency and credential-configuration identity across models, not by the narrower model/endpoint health key. No second service, inbox, task, provider credential or automatic executor fallback is introduced.

Codex remains the supplied executor: its inner model/tool retries and private conversation are not reimplemented or counted as visible native POSTs. Platform-owned start, turn and tool-operation uncertainty retains the existing original-identity lookup and no-duplicate-session behavior. New native policy is accepted into future task snapshots; absent historical policy remains disabled and old accepted inputs are not rewritten. Provider-side retrieval, deduplication and cancellation are used only when the selected adapter actually supports and qualifies them. The current nonstored Responses adapter has no such reconciliation proof for a lost model response.

These clauses are implementation work, not a claim that the present native adapter already retries, coordinates circuits, repairs invalid outputs or continues truncated output. Each failure-table branch needs its own actual evidence. Confirmed transient rejection retries reuse required input; format repair carries validation feedback, and a validated output-cap continuation carries retained partial output. A local byte limit, partial wire body, malformed envelope or helper failure does not establish provider output-cap truncation. Corrected invalid input is a new logical request admitted through the normal task interface, not a retry of the invalid request. A failed status with a text reason alone is not the specified blocked-state/event/exit behavior. Controlled provider fixtures establish classification and transitions; a small permitted provider journey establishes the actual adapter join. The earlier mixed-capacity failure remains historical evidence and is not repaired by relabeling it. Applicable platform-owned store and idempotent-tool retry paths are included; side-effecting tool uncertainty retains original-operation lookup and no blind resend. Notification decider engines, partition ownership, general event quarantine and tenant-role assumption are outside the current selected interfaces and remain explicitly deferred or not applicable rather than claimed implemented.

### Delegation Is A Task Handoff

A delegation MUST create a separately identified task.

A delegated task MUST retain its parent task association.

A delegation MUST commit its child task and parent-wait association atomically.

A child's terminal outcome MUST settle its parent-wait association before the parent resumes from that outcome.

A delegation request MUST NOT change configured host, repository or provider access.

The task's model-request accounting MUST include platform-mediated requests and configured child-task reservations.

A native worker can request extended work through the same task interface used by the collaboration product. The handoff carries instructions and relevant artifact references. It is not migration of a live native conversation into the supplied executor.

## Focused Governance

The four approved additions below adopt a bounded implementation of selected shared governance behavior. They are required implementation work, not claims that existing limit checks, task acceptance or stop commands already provide it. Programmable decider engines, model judges, elaborate decision caching and the wider shared-governance outcome remain deferred. The shared source and gate selection are unchanged.

### Voluntary Completion Checks Remaining Work

A voluntary completion decision MUST evaluate the current canonical required work for its task.

A voluntary completion decision MUST refuse completion while required work remains unfinished.

A voluntary completion decision MUST refuse completion while an applicable required input remains unconsumed.

A refused voluntary completion MUST record a bounded reason identifying the remaining required work or input.

A permitted continuation after refused voluntary completion MUST receive the recorded remaining-work reason as input.

A continuation after refused voluntary completion MUST preserve the task's original identity, accepted settings, remaining deadline, accounting and unresolved effects.

Consecutive continuations caused by refused voluntary completion MUST be bounded by versioned metadata.

The consecutive blocked-completion count MUST reset only on an independently originated event that changes the required work or applicable inputs.

Exhausting the blocked-completion bound MUST leave the task visibly blocked without authorizing another model turn.

Failure of the completion check MUST leave the task visibly blocked without authorizing another model turn.

An authorized cancellation or emergency stop MUST NOT wait for voluntary completion admission.

These checks govern a task's claim that it is done and its ordinary voluntary stop at an observable supported executor boundary. Required work is derived from the accepted task, declared acceptance checks and explicit canonical waits; required input is an addressed outcome owed to that continuation under the existing delivery rules. An optional unanswered question, a review with no explicit wait, or an advisory memory notice does not create required work, a human approval gate or an extra model request. The implementation does not infer semantic completeness from arbitrary prose.

The existing native response and supplied-executor adapter boundaries need actual integration. An executor exit, pending notice, assigned continuation, or committed wait is not evidence that the input was consumed. Completion and applicable-input changes are ordered by the durable owner before acceptance. A refused stop continues only through the existing safe boundary and admission checks; it never interrupts an already dispatched effect, resets a budget, replaces a task or silently repeats an uncertain operation. A blocked task has an explicit existing event or resume path; bounded continuation is not an automatic retry loop.

### Configuration Changes Preserve Governance

A governed configuration change MUST identify the immutable classification-rule revision used to evaluate it.

A governed configuration change MUST compare its effective before and after values composed with the same active defaults.

A governed configuration field without a sound comparison rule MUST be treated as widening.

Version identifiers and provenance metadata MUST NOT affect the effective-configuration comparison.

An agent-originated configuration change MUST NOT weaken governance applicable to its session, definition or ancestor definition.

A widening configuration change MUST be refused unless the authenticated invocation is trusted for that configured class of change.

A governed configuration change MUST commit its comparison evidence with its immutable revision and original operation outcome.

A governance-rule or change-policy revision MUST be applied through an operator-controlled path outside agent-writable work.

The first implementation extends the checks and evidence on the existing closed definition-limit operation, not its twelve-field vocabulary. Its selected-definition, active-global and change-policy revision comparisons, complete effective validation, immutable executor/inference/tool profile and original receipt remain. The configured existing limit-change class may admit known widening allowances without a new per-change approval queue. This is not permission to edit the rule or policy deciding the request. An unspecified previous value remains explicitly unspecified; no fabricated number or neutral classification hides uncertainty.

Rule and change-policy version selection uses the existing trusted-host operator boundary, separately from bound agent calls. Current caller and protected invocation provenance are established by the runtime, never by model arguments or a claimed person identity. Unknown classification is conservative; it cannot create an unconfigured authority class. Accepted tasks and retained operations keep their original snapshots. No accepted-task hot reload, fine-grained permission engine, model or executor selector, host credential, external action or protected-release bypass is added.

### Decision Rules Have Versions And Evidence

Every enforced governance rule MUST have an immutable version identity.

A governance decision record MUST identify its rule version, exact subject, caller provenance, outcome and bounded reason.

An enforcing governance decision MUST commit its audit event atomically with the transition it gates.

Replay of a recorded governance decision MUST retain its original decision identity and outcome.

A new or changed ordinary governance rule MUST run in shadow before enforcement.

A shadow evaluation MUST NOT delay or change the enforcing decision or its effects.

A shadow result MUST record whether substituting its verdict at the selected decision point would have changed the actual outcome.

Promotion of a governance rule MUST bind passing evaluation evidence to the exact rule and configuration revisions being promoted.

A mandatory safety check MUST NOT be disabled or converted to shadow evaluation by governance configuration.

The initial rules use closed deterministic decisions at the approved completion, configuration and control boundaries. Rule content and thresholds are versioned data over concrete mechanisms, not an arbitrary programmable decider engine. Every selected rule has bounded evaluation and an explicit failure outcome. Missing, failed, stale or incomplete evidence is not passing promotion evidence. Model judgments cannot approve authority.

Shadow work uses a bounded retained input and exact selected rule/configuration binding. It records an actual counterfactual decision at that point even when enforcement already refused; disagreement is not inferred merely from different rule outputs. It does not dispatch a tool, alter the governed task or gated transition, consume the agent's model budget or delay the foreground decision. Persisting its bounded audit and evaluation result is permitted. Shadow failure is visible and cannot become automatic promotion. Restart and lost acknowledgments retain original decision identities. The audit uses existing event and receipt mechanisms, records no credentials, and carries references or bounded safe summaries instead of unrestricted content. Caching, automatic experiments, training-label pipelines and automatic rule synthesis remain deferred.

### Emergency Control Is Independent Of Ordinary Policy

An authorized operator stop or pause MUST remain available when ordinary governance is unavailable or refuses progress.

An ordinary configuration change MUST NOT lift a hold imposed by an emergency control.

An emergency-control record MUST identify the actual authenticated invocation boundary rather than a caller-supplied identity.

A temporary governance override MUST require the configured trusted operator invocation path.

A temporary governance override MUST record its exact scope, prior policy, replacement policy, reason and expiry in a durable audit event.

An expired temporary governance override MUST restore its prior policy unless a normal governed change has explicitly superseded that override.

Recovery MUST reconcile temporary-override expiry before admitting work under the override.

Expiry or removal of a temporary governance override MUST NOT resume held work without the normal recovery checks.

A temporary governance override MUST NOT bypass protected-release authorization, mandatory validation, ownership fencing or uncertain-effect reconciliation.

The selected trust root remains ordinary configured host access and the existing protected invocation boundary. Independent control means independence from the ordinary policy evaluator, not survival of a destroyed store or an assertion of new authenticated-person, quorum, team or hostile-worker isolation guarantees. An agent tool cannot select operator provenance or invoke an emergency override.

Stop and pause retain durable control intent and truthful physical termination evidence. A pause prevents applicable fresh work from being admitted; it does not fabricate a completed task, erase a pending input, or claim a running effect stopped. Recovery may inspect and repair the selected governance configuration through the protected path while ordinary policy is broken. Temporary replacement preserves the old policy version and has an explicit bounded expiry. Restart, unknown outcomes and expiry races retain exact control/operation identities; inability to establish current expiry keeps ordinary admission closed. Restoring policy does not clear a separate emergency hold.

The protected-release path still requires real artifact-and-target-bound human authorization and successful mandatory checks. A recorded product decision, trusted-host label, shadow result or temporary override is not that authorization. No shared governance outcome, general pipeline, new credential or permission system is activated by these additions.

## Durable Tasks And Honest Recovery

### Accepted State Has One Durable Owner

An accepted task MUST have a durable input record.

An execution attempt MUST have a durable identity before it begins.

A task mutation MUST verify its current ownership generation.

A terminal task result MUST be committed before it is reported as final.

Committed platform task state MUST survive an application-process restart while the durable store remains intact.

The controller's database MUST reside on local storage.

A worker MUST access task state through the controller's application interface.

The platform MUST persist task history in its database.

The platform MUST persist explicitly recorded agent memory in its database.

The selected persistence mechanism is SQLite behind one controller. Workers do not open its database files remotely. The persistence boundary is task input, definition reference, ownership, status, mediated external-operation identities, checkpoint references, task history, explicit agent memory and accepted results. Supplied-executor internal memory is outside that boundary. Exact preservation of an unfinished supplied-executor conversation is deferred beyond the initial contract; copying a thread identifier does not persist the referenced history.

Automatic controller failover is outside the initial profile. Controller downtime interrupts coordination and does not permit reporting uncommitted results as accepted. Restart uses the intact local database. Database destruction remains within the disclosed storage-loss boundary. SQLite's serialized writes need qualification against the actual workload; this selection is not evidence of the scale target.

A recorded executor conversation identifier MUST NOT be treated as a durable checkpoint by itself.

### Interrupted Work Is Reconciled

An interrupted attempt MUST retain its last committed status and checkpoint references.

An external effect with an uncertain outcome MUST be reconciled before the platform retries that effect.

A retry MUST preserve the original external-operation identity where the receiving interface supports deduplication.

The platform MUST NOT report an interrupted command as stopped until its worker confirms termination or the worker is known unable to continue it.

A recovery attempt MUST preserve the original task identity.

Recovery that cannot resume available saved executor history MUST create a new attempt.

Recovery MUST NOT assume that uncheckpointed workspace content survived worker loss.

A saved executor conversation MUST be associated with the same task before it is resumed.

Resuming a conversation against a restored workspace MUST identify the recovered checkpoint to the executor.

Application-process restart recovers committed task state and reconciles actual worker status. After an executor-process failure, a saved conversation can be resumed only if its history is available and associated with the task. If the relationship between that history and the recovered workspace cannot be reconciled, recovery starts a new attempt from the durable task and checkpoint. After worker loss, unavailable local conversation state is not reconstructed by claim. A valid previously committed terminal result remains terminal.

The platform preserves the distinction between conversation continuation and a new attempt at the same task. Inner executor actions that bypass platform mediation do not acquire platform exactly-once guarantees. Existing host, repository and provider access controls also apply to those actions; uncertain externally visible outcomes still require reconciliation before retry.

### Backups Are Outside The Initial Milestone

The initial milestone has no backup, point-in-time recovery, backup restore drill or disaster-recovery guarantee. This does not make the application database disposable across ordinary process restarts. It does mean committed task data can be permanently lost if its only storage is destroyed, corrupted or administratively removed. Replication, if later selected, is not assumed to be a backup.

The initial deployment MUST disclose its storage-loss boundary.

A recovery procedure MUST NOT fabricate previously committed tasks when their storage has been lost.

Backups and restore qualification are deferred to a later operational milestone. No production-loss tolerance, backup frequency, recovery-point objective, recovery-time objective or additional storage service is selected here.

## Task And Worker Contract

### Durable Commands Have Stable Identity

A mutating request MUST carry an operation identity.

An exact retry of a committed mutating request MUST return its recorded outcome.

Reusing a mutating request's identity with different input MUST fail without changing its recorded outcome.

Task submission MUST commit its input and submission outcome atomically.

An attempt assignment MUST commit its worker command in the same transaction.

A controller transaction MUST NOT remain open across worker, provider or repository I/O.

The controller owns local database access through one bounded request queue. Task, attempt, command and external-operation identities are separate opaque values. An attempt carries its task, generation, worker incarnation and execution deadline. Task history and explicit agent memory are ordinary database records, not another event service. Physical table/index layout and exact scalar encodings are implementation details of this contract.

The application MUST identify the schema version of its stored task state.

An unsupported stored schema version MUST be refused without rewriting its data.

Corrupt stored task state MUST be refused without rewriting its data.

A change to stored task state representation MUST have an explicit migration disposition.

### Task State Separates Execution From Acceptance

An accepted terminal result MUST commit the task state and corresponding history atomically.

An executor outcome MUST remain distinct from the task's accepted outcome.

A retry MUST create a new attempt without changing the task identity.

A succeeded task MUST NOT be reopened by a retry request.

The task states are queued, active, succeeded, failed and cancelled. Queued includes a visible blocked reason when dependencies or capacity prevent admission. Attempt phases are assigned, running, stop-requested and stopped. Connectivity and unresolved external effects are separate observations; neither a disconnected worker nor an executor's zero exit status is a task result. Execution can be stopped while an earlier external operation still needs reconciliation.

### Worker Commands Do Not Create Duplicate Execution

A launch command MUST bind its attempt, generation, worker incarnation and payload identity.

A duplicate launch in the same worker incarnation MUST return the existing execution or outcome instead of starting another execution.

A worker MUST reject a launch addressed to an earlier incarnation.

A restarted worker MUST account for surviving prior-incarnation processes before accepting new work.

A stale-generation report MUST NOT change the current task result, checkpoint or agent memory.

The worker may submit stale observations for reconciliation, but they cannot authorize a new effect or overwrite current accepted state. Worker identity comes from configured authenticated host access, not the identity string in its report. Registration and reconnection first reconcile durable assignments. A fresh process identifier by itself is not evidence that an older process tree stopped.

### Cancellation Commits Intent Before Interruption

A cancellation request MUST commit cancellation intent and its stop command atomically.

A cancellation acknowledgment MUST distinguish requested cancellation from verified execution termination.

Successful completion and cancellation MUST be ordered by the controller's committed task revision.

An automatic retry MUST wait until prior execution is known unable to continue.

An automatic retry MUST wait until relevant uncertain external effects are reconciled.

Reconnecting or resuming an attempt MUST NOT reset its execution deadline.

Reconnecting or resuming an attempt MUST NOT reset its recorded model-request accounting.

If success commits first, cancellation returns the terminal result. If cancellation commits first, later executor output is retained as observation rather than accepted as task success. Controller restart reconciles the existing attempt; it does not allocate a successor merely because the controller restarted. Lost transport, missed liveness observations, interrupt acknowledgment and executor EOF are not termination proof. Unresolved process or external-effect status is exposed as needing reconciliation; elapsed disconnection alone is not permission to repeat work.

### Checkpoint Publication Is Reconciled

A checkpoint publication MUST have a durable operation record before it is sent.

The publication record MUST identify the repository, exact commit and intended remote reference.

Checkpoint publication MUST NOT overwrite an unrelated remote reference.

A publication with an uncertain outcome MUST be reconciled against its original remote reference and commit.

A superseded attempt's publication MUST NOT advance the task's selected checkpoint.

The remote reference is specific to the attempt/checkpoint operation. If publication succeeds but the controller commit or acknowledgment is lost, reconciliation settles that same operation before recording its checkpoint. A moving branch or an arbitrary remote commit cannot fill the gap. Pull-request creation and merge use the same stable-operation/outcome discipline where their provider permits observation; when an outcome cannot be established, automatic repetition remains blocked.

### Dependencies And Admission Are Bounded

Task dependencies MUST refer to existing tasks.

A task's dependency set MUST remain immutable after submission.

A task MUST start only after its prerequisites have succeeded.

A task waiting for delegated work MUST release its execution slot.

Admission MUST enforce configured queue and resource bounds.

Task admission MUST select the oldest eligible task within the requested executor's available capacity.

A rejected or deferred admission MUST expose its reason.

Referencing only existing tasks gives dependencies an acyclic creation order. Failed or cancelled prerequisites leave dependent tasks visibly blocked. Queues distinguish coding work and native requests. Oldest refers to committed submission order, not a client's clock. Eligibility includes satisfied prerequisites, no active execution attempt for the task's session, valid bounds and capacity for the requested executor; an ineligible older task does not stop a ready task of another kind. Builds and tests run directly in their coding workspace, under ordinary task and host-resource limits; there is no separate build-admission queue. Workers receive commands over a connection that waits for changes; a timeout is not interpreted as new work. Reconnect catches up from durable command identities, so losing a wakeup does not lose a task.

### The Supplied Executor Is Qualified At Its Actual Boundary

A worker MUST record the association between its task attempt and the supplied executor's thread and turn identifiers.

A lost start response MUST be reconciled before another executor turn is started for that command.

An unknown executor request MUST fail without silently granting authority.

The initial adapter uses one directly supervised local app-server process per active coding workspace and one active root turn per process. This is a proposed implementation decomposition to qualify, not a vendor production guarantee. Worker-local executor state is distinct from the controller's durable task state and from an operator's personal session directory.

The exact pinned interface needs qualification for process-tree cleanup and honest recovery. Repository builds and tests run directly in each coding workspace; no separate command permit or build coordinator is required. The trusted-worker profile does not claim hostile-process containment. Exact request/event records and discriminating cases are in [the wire-interface proposal](protocol.md).

## Disposable Workspaces

### Work Starts From A Durable Source

A workspace MUST identify its repository and starting revision.

A workspace's starting revision MUST resolve to an immutable source revision available outside the worker.

A workspace mutation MUST be assigned to one current task owner.

A checkpoint MUST identify the exact work it preserves.

A checkpoint MUST NOT be reported durable before its external storage acknowledges it.

A checkpoint reference MUST be committed to the task record before recovery selects it.

The recorded starting revision is the initial recovery point. The selected source checkpoint is a Git commit acknowledged by a repository outside the disposable worker; a moving branch alone is not a checkpoint identity. A local commit, untracked file, cache or build output is not automatically durable. Git checkpoints preserve source, not the task database. Binaries and caches are rebuilt initially. Accepted validation records and result references remain in durable task state. A separate large-artifact service is added only for a concrete preservation need; none is selected for the initial profile. The repository provider and remaining durable-record retention limits remain configuration details to resolve; disposable local cleanup follows the policy below.

Each worker maintains one local Git object cache per configured repository and creates a separate linked worktree for each coding attempt. Clean validation uses a separate worktree at the published checkpoint. Worktrees share committed objects, not working files or indexes. Cache creation and metadata maintenance are coordinated on that worker; they do not remove another active worktree. The central repository retains acknowledged checkpoints and serves exact-revision reads. A worker cache is disposable and is not a replacement for publication outside that worker. This layout does not place distributed workers on one shared writable filesystem or merge their changes automatically.

### Automatic Cleanup Preserves Durable Work

Automatic cleanup MUST use per-kind retention settings from worker metadata.

A worker MUST reclaim eligible disposable workspaces and local repository caches without an operator command.

Cleanup MUST preserve data required by an active attempt.

Cleanup MUST preserve data required to reconcile an uncertain or unacknowledged operation.

A workspace MUST become eligible for cleanup only after its canonical task is terminal and its bound attempt's execution descendants have stopped.

Workspace cleanup MUST preserve source edits required to complete or reconcile an outstanding checkpoint publication.

Cache cleanup MUST preserve a cache while a worktree, accepted task or unresolved recovery record depends on that local cache.

Cleanup MUST verify the exact owned path and repository identity before removing content.

Cleanup MUST use bounded work under its configured metadata policy.

Restart MUST reconcile incomplete cleanup before treating its resources as reclaimed.

A cleanup outcome MUST distinguish reclaimed, retained and uncertain resources.

Cleanup MUST preserve durable task results, histories, checkpoints, audit metadata, operation receipts and recovery journals in the initial profile.

The initial metadata defaults make safely disposable successful-attempt workspaces eligible immediately, retain failed or cancelled workspaces for 24 hours, and make an unreferenced reconstructible repository cache eligible after seven idle days. These delays do not override any safety hold. Eligibility uses canonical terminal and publication records, plus a durably recorded first observation that all cleanup conditions were met; filesystem modification time or a completed worker-command flag alone does not. Successful cleanup preserves any uncommitted nonignored source. After the failed or cancelled retention interval, uncheckpointed local edits may be discarded only when no active or unresolved work depends on them. An external repository or historical checkpoint reference alone does not pin a reconstructible worker-local cache. Cleanup records its policy revision and resumes bounded work after restart. Completed worktrees include their disposable build outputs; the central repository keeps acknowledged checkpoints.

The initial increment retains durable task history, accepted results, audit metadata, operation receipts and recovery journals. This makes durable audit outlive reclaimed disposable content, but does not establish bounded total database growth. Safe retirement of durable replay records and disk quotas remain separate work. Existing accepted execution settings are unchanged; current worker retention metadata governs only disposable local copies.

### Loss Returns To The Last Checkpoint

A replacement workspace MUST be reconstructed from an identified durable checkpoint.

The task record MUST distinguish preserved work from work whose outcome is unknown after worker loss.

Uncheckpointed edits, untracked files, local databases and build caches may be lost. The initial profile omits automatic snapshots after changing calls, complete filesystem history, transparent host migration, snapshot forks and atomic whole-workspace rollback. Source-control operations do not claim those semantics. Cross-repository atomic checkpoints are not provided.

### Workers Are Trusted Disposable Environments

Worker commands MUST run without administrator privileges.

Each attempt MUST have configured resource and output bounds.

A worker MUST enforce the attempt's execution deadline.

Each worker MUST run with its execution descendants in a single worker-wide resource group.

The selected mechanism is one Linux cgroup v2 per worker, established through ordinary host startup configuration before the worker launches execution children. CPU bandwidth, memory/swap and process ceilings are shared by the worker, supplied executor, Git, builds, checks and native helpers. Limits leave generous headroom for concurrent builds and the worker's control and cleanup paths; exact values require measurement on the actual hosts. Production qualification covers actual descendant membership, limit exhaustion and recovery.

This profile has no per-task resource reservations, dynamic resource balancing or custom resource manager. Tasks share capacity, so one task can pressure or terminate other work in the worker's group. Disk-space and file-count enforcement remain a separate filesystem/quota work item; the cgroup does not establish storage capacity or durable-journal headroom. Existing deadlines, cancellation, output/result bounds, ownership and unknown-effect handling remain independent.

The initial worker profile makes no hostile-workload containment guarantee. Separate directories and resource controls reduce accidental interference but do not prove isolation between mutually hostile processes, repositories or dependencies. A compromised worker can lose its work and any authority actually available to it. A container, virtual machine, sandbox platform and worker scheduler are not selected by this profile.

### Sensitive Authority Stays Outside Agent-Writable Work

Production rollout authority MUST remain outside the agent-writable workspace.

Release acceptance evidence MUST be produced by a protected checker outside the agent-writable workspace.

The worker MUST NOT receive platform-administration credentials.

Worker credentials MUST be limited to the deployment's intended operations.

Logs and task results MUST NOT include credentials.

These controls use ordinary operator-controlled host accounts, file permissions and the permissions available on repository and provider credentials. Credentials are configured for the trusted worker deployment, not issued or narrowed by a per-task grant engine. Tasks sharing a worker identity can use the access actually available to that identity; the initial profile does not claim different permissions or confidentiality between those tasks. The checker and rollout authority remain outside agent write access; a different directory alone does not establish that protection. Trusted-worker compromise can expose every credential and resource actually available to that worker.

The host/process mapping still needs a compatibility disposition against the unchanged shared platform-node restrictions. This profile does not select a container, virtual machine or additional host to satisfy those older restrictions.

## Storage Protection

The initial profile omits Borg-managed content encryption, custom sealed-record envelopes, per-session content-key hierarchies and cryptographic erasure. Database/storage administrators with readable storage access may read application content. Provider or disk encryption can protect storage media without supplying the removed application-key guarantees; its availability is not assumed.

### The Security Profile Uses Existing Host Access

The initial profile MUST NOT depend on a key-management or secret-management service.

The initial profile MUST NOT require custom per-call credential issuance.

The initial profile MUST NOT require a fine-grained application permission engine.

The selected deployment trusts its operator-controlled hosts and execution workers. SSH authentication, tunnels and port forwarding provide the optional cross-host access mechanism. No OpenBao, managed key service, owned key server, custom assertion-signing infrastructure, service mesh or separately managed certificate infrastructure is part of this profile. Ordinary SSH keys and external-service credentials still exist; the operator supplies them using the selected tools' existing configuration and ordinary host access controls. This does not create a new credential-delivery service.

The initial release defers application role/grant graphs, per-agent and per-task access policies, delegated capability tokens and programmable permission evaluation. It retains authenticated access and existing operating-system, repository and provider controls. Submission and delegation do not provision credentials or modify those controls. Ownership generations, stale-report refusal, deadlines and resource budgets remain correctness checks, not fine-grained permission machinery.

### Transport And Access Controls Remain

A cross-host connection carrying credentials or private content MUST use an authenticated encrypted transport.

Hosts MAY connect through authenticated encrypted port forwarding.

Same-host communication MAY rely on ordinary host access controls.

An endpoint relying on the host's authenticated access MUST NOT be exposed directly on an untrusted network.

Every product request MUST be authenticated before it is accepted.

Task execution MUST remain subject to the worker's configured host access controls.

The authenticated host account or SSH access boundary can establish the trusted operator identity; the application does not need a separate sign-on system or a signature on each request. It does not accept caller-supplied identity as authentication. Forwarded service listeners are local to the receiving host. External services retain their supported authentication and encrypted transport, such as provider HTTPS. A local connection does not require its own tunnel or transport-encryption layer. Task ownership generations, stale-writer rejection and external-effect reconciliation remain correctness obligations, independent of credential issuance.

### Deletion Does Not Promise Cryptographic Erasure

A deletion result MUST identify the retained-data classes excluded from its guarantee.

Deleting an application record does not prove that old database pages, replicas, exports or future backups are unreadable. The initial profile does not introduce a signing or key-custody service to replace the removed content-key system. The shared assertion and credential-service contracts remain explicit compatibility differences rather than hidden dependencies of this profile.

## Production Scale And Runtime Change Delivery

### Initial Four-Host Qualification

Initial production qualification MUST use four hosts.

Initial production qualification MUST exercise 256 concurrent outstanding real native model requests on each execution worker.

Initial production qualification MUST exercise eight active coding workspaces on each execution worker.

The native-request and coding-workspace targets MUST be exercised during the same test interval.

Every coding workspace MUST be able to run repository builds and tests.

Locally queued tasks MUST NOT be counted as outstanding native model requests.

The selected layout has one controller host and three execution-worker hosts. The initial target is therefore 768 outstanding real native model requests alongside 24 active coding workspaces, with builds and tests available in every coding workspace. Controller duties remain separate from agent builds. The host profile and qualification details are in [the production target](production-target.md).

The selected measure is worker capacity: real provider calls outstanding at the worker client boundary, from dispatch until the client receives the terminal response or records failure or cancellation. Task admission, local task queues and reserved request allowances are separate from dispatched calls. Client connection-pool, DNS, TLS, network and provider wait can be part of an outstanding call; this measure does not establish simultaneous provider-internal processing. Report those measurement limits and actual outcomes separately. This interpretation follows [the operator selection](decisions.md#worker-capacity-measurement-selected).

Qualification ramps native requests through 32, 64, 128 and 256 per worker with coding/build work running. A further 512-per-worker step is stress-only, not an adopted production concurrency level. Measured resource use, response latency, provider limits and execution budgets constrain actual admission. An unavailable provider allowance or an overloaded host is a qualification limit, not evidence that locally queued work satisfies the target. Ancillary latency objectives and soak/resource budgets in the linked document remain qualification proposals.

### The Combined Scale Target Remains

The following clauses retain the later ten-host scale objective. They do not describe the first four-host production acceptance level. Qualifying the smaller level does not establish the larger one.

The production architecture MUST support at least 10 hosts operating simultaneously.

Each counted host MUST support at least 1,000 active agents concurrently.

Each counted host MUST support at least 10 active workspaces concurrently.

Scale qualification MUST exercise both per-host targets during the same interval.

For the later ten-host objective, the hybrid workload mix, definition of active agent work, machine capacity, latency targets and test duration remain explicit review choices. Dormant definitions or additional uncounted workspace hosts do not satisfy the target. Smaller tests use the intended architecture; they do not claim full scale. The target does not imply capacity survives host loss.

### Changes Can Be Prepared Without Per-Change Human Approval

The system MUST support editing its own runtime source.

The system MUST support compiling its own runtime changes.

The system MUST support testing its own runtime changes.

The system MUST support submitting its own runtime changes.

Change preparation MUST produce a deployable artifact linked to its source revision.

The final rollout MUST require human authorization bound to that artifact and target.

The final rollout MUST require successful applicable mandatory validation for the exact artifact and target.

Human authorization MUST NOT bypass applicable mandatory validation.

This preserves the earlier runtime self-delivery objective. It does not grant a workspace deployment credentials or adopt any particular repository, CI or deployment product.

### Configured Actions And Automatic Merge

An agent MAY merge a change after protected checks have passed for its exact revision.

A merge MUST NOT trigger a rollout without the rollout's required human authorization.

External actions MUST remain subject to the repository and provider access controls in force.

The initial coding workflow includes editing, building, testing, committing, pushing and opening pull requests. Automatic merging uses protected repository checks for the exact revision. This workflow does not add package publication, messaging, infrastructure changes or unrelated service mutations. The initial deployment relies on the real access configured on worker accounts and external credentials; it does not claim to enforce a narrower permission list for each task. Wasmi and the fine-grained permission engine are deferred. Repository branch rules remain applicable.

## Development And Evidence

### Integrate From The First Increment

The execution plan MUST define one runnable end-to-end acceptance path before component work is divided.

A component delivery MUST include a compiled real-consumer integration.

A component delivery MUST include the results of its relevant tests.

The integrated acceptance path MUST be exercised after changes to its interfaces.

The [execution plan](execution-plan.md) describes the initial path and subsequent increments. One owner completes each code/build/test/correction batch; the integrator owns the application. Independent work has disjoint source ownership. Readiness is demonstrated by runnable dependencies, not by an uncompiled interface packet.

### Evidence Identifies What Actually Ran

A test result MUST identify the exact source and executable it exercised.

Planning associations MUST remain distinct from implementation and executed-test evidence.

Native requirement coverage MUST use the retained traceability tool's extracted requirement identities.

Release acceptance MUST exercise the supported end-to-end task journey.

Release acceptance MUST exercise controller-restart recovery.

Release acceptance MUST exercise worker-loss recovery.

Release acceptance MUST verify rejection of stale task owners.

Release acceptance MUST verify that a settled external effect is not repeated by recovery.

The release review MUST assess uncovered critical paths identified by coverage measurements.

V2 uses behavior-based release criteria, focused Bach/Bolero tests of owned state machines and measured coverage. It does not inherit v1's blanket 100 percent executable-line and instrumented-branch release gates. Real integration tests cover external dependencies; a substitute is not a claim that supplied-executor internals run under simulation. Applicable behavior and recovery checks remain mandatory even when coverage measurements alone look satisfactory. The shared simulation/coverage contract still needs the explicit compatibility disposition recorded in the review ledger.
