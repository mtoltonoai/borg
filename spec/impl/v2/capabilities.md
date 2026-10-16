# V2 Capability Specification

This is the sole authored capability/dependency inventory for the proposed v2 delivery plan. Source lines quote native v2-owned requirements; they are references, not duplicate authored obligations. This inventory does not claim coverage of the shared2105-entry contract. Profile selections remain recorded in decisions.md; declaration adoption and actual readiness are kept in separately bound review/progress metadata.

Source snapshot: 5dc96026b71b18687165376918375fd824f345c4a1d61c6928f49939181da2e8
Source selection digest: 04967de989a49a8602257f54a1bf400c52960f9e51a829e7e8394728e82b20b7

The snapshot is a non-authoritative evaluation of v2-owned main-spec requirements only; it is neither the shared target nor the real runtime lock. Allocation comments identify a primary acceptance location; generated native links and a separate exact-allocation check verify them. Interface-only batches qualify their own component seam; actual product integration is closed by the integrated journey/recovery milestones.

---
ID: V2_CONTROL
Title: Controller and durable task state
Prerequisites: none
Deliverable: One local SQLite controller owns task/attempt/command/result/memory transitions and the private API.
Acceptance: Real-store duplicate/conflict, commit-loss, restart, admission and authenticated API cases pass.
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | A recorded executor conversation identifier MUST NOT be treated as a durable checkpoint by itself.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | A task mutation MUST verify its current ownership generation.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | A terminal task result MUST be committed before it is reported as final.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | A worker MUST access task state through the controller's application interface.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | An accepted task MUST have a durable input record.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | An execution attempt MUST have a durable identity before it begins.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | Committed platform task state MUST survive an application-process restart while the durable store remains intact.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | The controller's database MUST reside on local storage.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | The platform MUST persist explicitly recorded agent memory in its database.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#accepted-state-has-one-durable-owner | The platform MUST persist task history in its database.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent definition MUST identify the executor used for its tasks.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent identity MUST persist independently of any executor process.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent's behavior MUST be described by configuration.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An execution attempt MUST bind the definition revision it uses.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#deletion-does-not-promise-cryptographic-erasure | A deletion result MUST identify the retained-data classes excluded from its guarantee.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | A rejected or deferred admission MUST expose its reason.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | A task MUST start only after its prerequisites have succeeded.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | A task waiting for delegated work MUST release its execution slot.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | A task's dependency set MUST remain immutable after submission.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | Admission MUST enforce configured queue and resource bounds.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | Task dependencies MUST refer to existing tasks.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | A controller transaction MUST NOT remain open across worker, provider or repository I/O.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | A mutating request MUST carry an operation identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | An attempt assignment MUST commit its worker command in the same transaction.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | An exact retry of a committed mutating request MUST return its recorded outcome.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | Reusing a mutating request's identity with different input MUST fail without changing its recorded outcome.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | Task submission MUST commit its input and submission outcome atomically.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#task-state-separates-execution-from-acceptance | A retry MUST create a new attempt without changing the task identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#task-state-separates-execution-from-acceptance | A succeeded task MUST NOT be reopened by a retry request.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#task-state-separates-execution-from-acceptance | An accepted terminal result MUST commit the task state and corresponding history atomically.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#task-state-separates-execution-from-acceptance | An executor outcome MUST remain distinct from the task's accepted outcome.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | A cross-host connection carrying credentials or private content MUST use an authenticated encrypted transport.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | Task execution MUST remain subject to the worker's configured host access controls.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | An endpoint relying on the host's authenticated access MUST NOT be exposed directly on an untrusted network.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | Every product request MUST be authenticated before it is accepted.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | Hosts MAY connect through authenticated encrypted port forwarding.
<!-- Allocation: V2_STORE#ACCESS | permission -->
Source: spec/impl/v2/implementation.md#transport-and-access-controls-remain | Same-host communication MAY rely on ordinary host access controls.
<!-- Allocation: V2_STORE#ACCESS | permission -->
Source: spec/impl/v2/implementation.md#the-security-profile-uses-existing-host-access | The initial profile MUST NOT require a fine-grained application permission engine.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent identity MUST be the stable identity of its definition.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | Every instantiation of an agent MUST have a distinct session identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | Every session MUST carry the agent identity it runs as.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent identity MUST support multiple concurrent sessions.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | A session MUST have at most one active execution attempt at a time.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | Recovery of a session MUST retain its session identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | A session MUST retain its own execution state.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | A memory MUST be keyed by agent identity rather than by session.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent's memories MUST persist across every session of that agent identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | A task MUST retain the definition revision accepted at submission.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | The application MUST identify the schema version of its stored task state.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | An unsupported stored schema version MUST be refused without rewriting its data.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | Corrupt stored task state MUST be refused without rewriting its data.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#durable-commands-have-stable-identity | A change to stored task state representation MUST have an explicit migration disposition.
<!-- Allocation: V2_STORE#STATE | delivery-process -->
Source: spec/impl/v2/implementation.md#dependencies-and-admission-are-bounded | Task admission MUST select the oldest eligible task within the requested executor's available capacity.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | The collaboration tool interface MUST expose the existing task-space reads to both executors.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | The collaboration tool interface MUST expose configured task-space mutations under their current operation rules.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | The collaboration tool interface MUST use the controller's canonical operation layer.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | The collaboration tool interface MUST provide read-only views of worker capacity, task attempts, checkpoints and release-check records.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | Operational queries MUST NOT grant authority to mutate worker controls or protected release state.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | Collaboration tool responses MUST exclude credential values.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-collaboration-tool-interface | The collaboration tool interface MUST validate its caller's current attempt before a new mutation.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->

Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent definition MUST specify the model used by its executor.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An agent definition MUST specify the reasoning effort used by its executor.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#definitions-select-execution | An execution attempt MUST use the inference settings of its accepted definition revision.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-reads-through-the-shared-tool-interface | The shared agent interface MUST provide read-only repository access within the task's accepted repository scope.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#repository-reads-through-the-shared-tool-interface | A repository read result MUST identify its exact source revision.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#repository-reads-through-the-shared-tool-interface | An agent repository-read operation MUST NOT execute repository-supplied code.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#repository-reads-through-the-shared-tool-interface | An agent repository-read operation MUST NOT modify repository content.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->

Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | A committed memory update MUST make its changed key and committed revision available to other active sessions of the same agent.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | A session MUST receive pending memory-change notices at its next safe model or tool boundary.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | Pending memory-change notices MUST remain recoverable after a process restart.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | Memory-change notification delivery MUST NOT interrupt a tool that has begun execution.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | Memory-change notices MUST NOT create a session.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | A memory-change notice MUST NOT initiate a model request by itself.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#shared-memory-changes-reach-sessions | Pending notices for the same memory key MAY be coalesced to the latest committed revision.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | Every configurable runtime limit MUST be supplied by metadata rather than a policy constant in executable code.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | Global metadata MUST provide explicit defaults for limits without a more specific configured value.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | An agent definition MUST carry the execution-limit overrides specific to that agent revision.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | An accepted task MUST retain its resolved execution-limit values.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | An accepted task MUST identify the metadata revisions used to resolve its execution limits.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | Changing default metadata MUST NOT silently change the limits of an already accepted task.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | Recovery MUST use the execution limits retained by the task being recovered.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | A component MUST validate its configured limits before admitting work governed by those limits.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | An invalid or inconsistent limit configuration MUST be reported without silently clamping its values.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | A component MUST NOT impose an undisclosed policy cap below the configured limit.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | An operation refused by a configured limit MUST identify the limiting metadata field.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | The shared agent interface MUST expose the effective limits applicable to its caller.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | The runtime MUST distinguish configured policy limits from non-configurable representation and external-service constraints.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | A retained operation MUST NOT be discarded solely because a later limit configuration is lower.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | A worker MUST verify the configured host controls before claiming that those controls are enforced.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->
Source: spec/impl/v2/implementation.md#maximum-context-defaults | Default context limits MUST use the maximum supported by the selected model and executor.
<!-- Allocation: V2_STORE#METADATA_LIMITS | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | The runtime MUST support multiple repository registrations described by metadata.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A repository registration MUST have a stable repository identity.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A repository registration revision MUST be immutable.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | An operator repository-catalogue mutation MUST use the canonical durable controller operation path.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A repository-catalogue update MUST compare the current repository head generation supplied by the operator.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | An accepted task MUST retain its selected repository registration revision.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A task MUST have at most one primary repository association.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A newly resolved repository association MUST reject a repository registration disabled for new tasks.
<!-- Allocation: V2_STORE#ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A catalogue revision or enable-state change MUST NOT change an accepted task's repository association.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | The shared agent interface MUST provide bounded repository-catalogue list and get operations.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | Agent tools MUST NOT mutate repository registrations.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A repository registration operation MUST NOT acquire repository material.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A repository registration operation MUST NOT grant repository access.
<!-- Allocation: V2_STORE#ACCESS | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A model-visible repository projection MUST NOT expose controller storage keys.
<!-- Allocation: V2_STORE#COLLAB_MCP | behavior -->

Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | The shared agent interface MUST support diagnosis of configuration refusals using existing bounded operational records.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | The shared agent interface MUST expose supported definition-limit changes through the canonical controller operation layer.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST compare the expected selected definition revision before applying a new immutable revision.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST compare the expected active global metadata and change-policy revisions before application.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST be restricted to the fields allowed by the configured change policy.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST validate the complete resulting effective configuration before commit.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A permitted definition-limit change MUST atomically commit its immutable definition, selected head and durable operation outcome.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST preserve the executor, inference settings and tool capabilities of its predecessor.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | Submitting an explicitly pinned definition MUST NOT change an existing agent's selected definition revision.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A configuration change outcome MUST distinguish future-task selection from settings retained by already accepted tasks.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | Evaluation of a configuration change MUST identify its committed change outcome and the definition revisions of the observed work.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#operational-feedback-applies-supported-configuration | A definition-limit change MUST NOT grant host administration, credentials or protected release authority.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | Every collaboration record MUST have a durable identity.
<!-- Allocation: V2_COLLAB_CONTRACT#RECORDS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | The controller MUST derive a collaboration mutation's actor provenance from its invocation context.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | An application-supplied actor label MUST NOT be recorded as authenticated-human identity.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | A mutation of an existing collaboration record MUST compare the expected record revision.
<!-- Allocation: V2_COLLAB_CONTRACT#RECORDS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | A collaboration request MUST refuse unsupported recipient kinds.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | A committed document version MUST be immutable.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | A document version number MUST NOT be reused.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | The controller MUST refuse a supplied document content identity that differs from its computed content identity.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | A posted collaboration comment MUST be immutable.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | A document edit MUST create a new version without changing an earlier version.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#document-versions-and-comments-are-immutable | The initial collaboration profile MUST retain committed document versions.
<!-- Allocation: V2_COLLAB_DOCUMENTS#DOCUMENTS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A posted question's prompt, options and typed decision fields MUST be immutable.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A recorded answer MUST be immutable.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A question answer MUST match the direct recipient recorded on the question to the caller context established by the controller.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | An answer replacement or withdrawal MUST be accepted only from the question's recorded direct recipient.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | An answer replacement or withdrawal MUST retain the original answer record.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | An open question MUST remain cancellable or supersedable by its current responsible record owner when the asker is inactive.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A question cancellation or supersession MUST record its acting caller context and reason.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A question cancellation or supersession MUST preserve the original poser.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A replacement question MUST record the acting poser and the question it supersedes.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | An answered question MUST NOT be cancelled or superseded.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A record-owner change MUST preserve its open questions.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A blocking question MUST take no default answer.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A question's prompt MUST state the recommended option.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A question routed to the trusted-host operator MUST ask exactly one decision.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A moot mark MUST record its acting caller context and the answer that made the decision moot.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A moot mark MUST remove the marked answer's product effect.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#questions-preserve-their-posed-content-and-outcomes | A moot mark MUST NOT create a product decision effect.
<!-- Allocation: V2_COLLAB_QUESTIONS#QUESTIONS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A review MUST bind the exact immutable subject version or result reference it evaluates.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A review decision MUST match its recorded direct reviewer to the caller context established by the controller.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A review decision MUST retain the reviewed subject identity.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A recorded review decision MUST be immutable.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A new document version MUST NOT inherit the decision of an earlier version's review.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | Withdrawing a review MUST record its acting caller context and reason.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#reviews-bind-the-exact-subject-version | A review withdrawal MUST publish committed activity identifying the affected review.
<!-- Allocation: V2_COLLAB_DOCUMENTS#REVIEWS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A collaboration wait MUST identify a concrete task, question or review resolver.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A wait on an agent, team or external party MUST be refused.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A wait on a resolver that is already closed MUST be refused.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A wait that would close a cycle through waits and parent links MUST be refused.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | The first open collaboration wait MUST retain the task's prior workflow state and assignee.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | Ending the last collaboration wait on a nonterminal task MUST restore its recorded workflow state and assignee.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A resolved wait MUST retain the exact terminal outcome occurrence that resolved it.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | An unblock activity MUST carry the typed outcome of each wait it ends.
<!-- Allocation: V2_COLLAB_WORK#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | A product question or review decision MUST NOT confer protected rollout authority.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | A product question or review decision MUST NOT grant host, repository, provider or credential access.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | The collaboration expansion MUST NOT require human approval of ordinary work that has no explicitly requested product wait.
<!-- Allocation: V2_COLLAB_CONTRACT#PROVENANCE | behavior -->

Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A voluntary completion decision MUST evaluate the current canonical required work for its task.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A voluntary completion decision MUST refuse completion while required work remains unfinished.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A voluntary completion decision MUST refuse completion while an applicable required input remains unconsumed.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A refused voluntary completion MUST record a bounded reason identifying the remaining required work or input.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A permitted continuation after refused voluntary completion MUST receive the recorded remaining-work reason as input.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | A continuation after refused voluntary completion MUST preserve the task's original identity, accepted settings, remaining deadline, accounting and unresolved effects.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | Consecutive continuations caused by refused voluntary completion MUST be bounded by versioned metadata.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | The consecutive blocked-completion count MUST reset only on an independently originated event that changes the required work or applicable inputs.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | Exhausting the blocked-completion bound MUST leave the task visibly blocked without authorizing another model turn.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | Failure of the completion check MUST leave the task visibly blocked without authorizing another model turn.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#voluntary-completion-checks-remaining-work | An authorized cancellation or emergency stop MUST NOT wait for voluntary completion admission.
<!-- Allocation: V2_STORE#STOP_ADMISSION | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A governed configuration change MUST identify the immutable classification-rule revision used to evaluate it.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A governed configuration change MUST compare its effective before and after values composed with the same active defaults.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A governed configuration field without a sound comparison rule MUST be treated as widening.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | Version identifiers and provenance metadata MUST NOT affect the effective-configuration comparison.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | An agent-originated configuration change MUST NOT weaken governance applicable to its session, definition or ancestor definition.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A widening configuration change MUST be refused unless the authenticated invocation is trusted for that configured class of change.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A governed configuration change MUST commit its comparison evidence with its immutable revision and original operation outcome.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#configuration-changes-preserve-governance | A governance-rule or change-policy revision MUST be applied through an operator-controlled path outside agent-writable work.
<!-- Allocation: V2_STORE#CONFIGURATION_APPLY | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | Every enforced governance rule MUST have an immutable version identity.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | A governance decision record MUST identify its rule version, exact subject, caller provenance, outcome and bounded reason.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | An enforcing governance decision MUST commit its audit event atomically with the transition it gates.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | Replay of a recorded governance decision MUST retain its original decision identity and outcome.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | A new or changed ordinary governance rule MUST run in shadow before enforcement.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | A shadow evaluation MUST NOT delay or change the enforcing decision or its effects.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | A shadow result MUST record whether substituting its verdict at the selected decision point would have changed the actual outcome.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | Promotion of a governance rule MUST bind passing evaluation evidence to the exact rule and configuration revisions being promoted.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#decision-rules-have-versions-and-evidence | A mandatory safety check MUST NOT be disabled or converted to shadow evaluation by governance configuration.
<!-- Allocation: V2_STORE#DECISION_RULES | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | An authorized operator stop or pause MUST remain available when ordinary governance is unavailable or refuses progress.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | An ordinary configuration change MUST NOT lift a hold imposed by an emergency control.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | An emergency-control record MUST identify the actual authenticated invocation boundary rather than a caller-supplied identity.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | A temporary governance override MUST require the configured trusted operator invocation path.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | A temporary governance override MUST record its exact scope, prior policy, replacement policy, reason and expiry in a durable audit event.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | An expired temporary governance override MUST restore its prior policy unless a normal governed change has explicitly superseded that override.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | Recovery MUST reconcile temporary-override expiry before admitting work under the override.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | Expiry or removal of a temporary governance override MUST NOT resume held work without the normal recovery checks.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->
Source: spec/impl/v2/implementation.md#emergency-control-is-independent-of-ordinary-policy | A temporary governance override MUST NOT bypass protected-release authorization, mandatory validation, ownership fencing or uncertain-effect reconciliation.
<!-- Allocation: V2_STORE#EMERGENCY_CONTROLS | behavior -->

Source: spec/impl/v2/implementation.md#definitions-carry-reusable-charters | An agent definition MUST identify its reusable charter source.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#a-charter-has-normative-requirements | A charter's behavioral requirements MUST use the normative key words with the meanings defined by the constitution.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#a-charter-has-normative-requirements | Each charter requirement MUST be a self-contained sentence carrying one obligation under a stable heading.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#a-revision-identifies-authoritative-source | A charter revision reference MUST resolve to the exact authoritative source of that revision under the applicable access and retention rules.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#a-revision-identifies-authoritative-source | A charter revision reference MUST reuse an existing source identity when that identity resolves the complete authoritative charter source.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#a-revision-identifies-authoritative-source | A rendered or compacted representation of a charter MUST NOT replace that charter's authoritative source.
<!-- Allocation: V2_CHARTER_RECORDS#SOURCE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | The platform MUST maintain one coordinated circuit breaker for each configured model and endpoint.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A circuit-breaker probe outcome MUST be settled against its exact original request identity and probe generation.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A provider authentication failure MUST be coordinated across workers for the same dependency and credential-configuration identity with metadata-controlled backoff and a retained alert.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Every transition into a retry-related blocked state MUST publish its reason as a durable event.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Every retry-related blocked state MUST have an explicit canonical exit transition available through the supported adapter or control interface.
<!-- Allocation: V2_STORE#STATE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A transient failure of a platform-owned durable-store transaction MUST be retried with metadata-controlled backoff while retaining the original operation identity.
<!-- Allocation: V2_STORE#STATE | behavior -->

---
ID: V2_WORKSPACE
Title: Coding worker and Git checkpoint execution
Prerequisites: V2_CONTROL
Deliverable: One worker mode supervises pinned Codex, workspace builds/tests, process cleanup and Git/submission reconciliation.
Acceptance: Real coding journey, duplicate launch, stop uncertainty, lost publication acknowledgment and stale-generation cases pass.
Dependency: V2_CONTROL | inferred | Uses the controller task contract and accepted state.
Source: spec/impl/v2/implementation.md#backups-are-outside-the-initial-milestone | A recovery procedure MUST NOT fabricate previously committed tasks when their storage has been lost.
<!-- Allocation: V2_E2E#BOUNDARIES | behavior -->
Source: spec/impl/v2/implementation.md#backups-are-outside-the-initial-milestone | The initial deployment MUST disclose its storage-loss boundary.
<!-- Allocation: V2_E2E#BOUNDARIES | behavior -->
Source: spec/impl/v2/implementation.md#checkpoint-publication-is-reconciled | A checkpoint publication MUST have a durable operation record before it is sent.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#checkpoint-publication-is-reconciled | A publication with an uncertain outcome MUST be reconciled against its original remote reference and commit.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#checkpoint-publication-is-reconciled | A superseded attempt's publication MUST NOT advance the task's selected checkpoint.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#checkpoint-publication-is-reconciled | Checkpoint publication MUST NOT overwrite an unrelated remote reference.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#checkpoint-publication-is-reconciled | The publication record MUST identify the repository, exact commit and intended remote reference.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#loss-returns-to-the-last-checkpoint | A replacement workspace MUST be reconstructed from an identified durable checkpoint.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#loss-returns-to-the-last-checkpoint | The task record MUST distinguish preserved work from work whose outcome is unknown after worker loss.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | A task MUST NOT be reported successful solely because its executor exited successfully.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | The initial system MUST accept an authenticated task through its product interface.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | The initial system MUST durably record an accepted task before reporting acceptance.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | The initial system MUST execute a coding task in a workspace through the supplied executor.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | The initial system MUST expose the task's actual execution status through its product interface.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#observable-end-to-end-result | The initial system MUST return references to the source changes and validation results produced by a successful task.
<!-- Allocation: V2_E2E#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#the-security-profile-uses-existing-host-access | The initial profile MUST NOT depend on a key-management or secret-management service.
<!-- Allocation: V2_E2E#BOUNDARIES | behavior -->
Source: spec/impl/v2/implementation.md#the-security-profile-uses-existing-host-access | The initial profile MUST NOT require custom per-call credential issuance.
<!-- Allocation: V2_E2E#BOUNDARIES | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-is-qualified-at-its-actual-boundary | A lost start response MUST be reconciled before another executor turn is started for that command.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-is-qualified-at-its-actual-boundary | A worker MUST record the association between its task attempt and the supplied executor's thread and turn identifiers.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-is-qualified-at-its-actual-boundary | An unknown executor request MUST fail without silently granting authority.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-owns-its-inner-loop | An unavailable executor history MUST NOT be reported as a recovered conversation.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-owns-its-inner-loop | The adapter MUST record the supplied executor's reported outcome separately from the platform's task acceptance result.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-owns-its-inner-loop | The platform MUST keep accepted task results independently of the supplied executor's process.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-owns-its-inner-loop | The supplied executor adapter MUST use the documented interface of its pinned executor release.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#the-supplied-executor-owns-its-inner-loop | The supplied executor release MUST be pinned.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A checkpoint MUST NOT be reported durable before its external storage acknowledges it.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A checkpoint MUST identify the exact work it preserves.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A checkpoint reference MUST be committed to the task record before recovery selects it.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A workspace MUST identify its repository and starting revision.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A workspace mutation MUST be assigned to one current task owner.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#work-starts-from-a-durable-source | A workspace's starting revision MUST resolve to an immutable source revision available outside the worker.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#worker-commands-do-not-create-duplicate-execution | A duplicate launch in the same worker incarnation MUST return the existing execution or outcome instead of starting another execution.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#worker-commands-do-not-create-duplicate-execution | A launch command MUST bind its attempt, generation, worker incarnation and payload identity.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#worker-commands-do-not-create-duplicate-execution | A restarted worker MUST account for surviving prior-incarnation processes before accepting new work.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#worker-commands-do-not-create-duplicate-execution | A stale-generation report MUST NOT change the current task result, checkpoint or agent memory.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#worker-commands-do-not-create-duplicate-execution | A worker MUST reject a launch addressed to an earlier incarnation.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#workers-are-trusted-disposable-environments | A worker MUST enforce the attempt's execution deadline.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#workers-are-trusted-disposable-environments | Each attempt MUST have configured resource and output bounds.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#workers-are-trusted-disposable-environments | Each worker MUST run with its execution descendants in a single worker-wide resource group.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#workers-are-trusted-disposable-environments | Worker commands MUST run without administrator privileges.
<!-- Allocation: V2_E2E#INTEGRATED_WORKER | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | An accepted repository association MUST distinguish the source endpoint from the checkpoint destination.
<!-- Allocation: V2_WORKER#WORKSPACE | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A checkpoint publication MUST identify its accepted repository registration revision.
<!-- Allocation: V2_WORKER#CHECKPOINT | behavior -->

Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Automatic cleanup MUST use per-kind retention settings from worker metadata.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | A worker MUST reclaim eligible disposable workspaces and local repository caches without an operator command.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cleanup MUST preserve data required by an active attempt.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cleanup MUST preserve data required to reconcile an uncertain or unacknowledged operation.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | A workspace MUST become eligible for cleanup only after its canonical task is terminal and its bound attempt's execution descendants have stopped.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Workspace cleanup MUST preserve source edits required to complete or reconcile an outstanding checkpoint publication.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cache cleanup MUST preserve a cache while a worktree, accepted task or unresolved recovery record depends on that local cache.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cleanup MUST verify the exact owned path and repository identity before removing content.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cleanup MUST use bounded work under its configured metadata policy.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Restart MUST reconcile incomplete cleanup before treating its resources as reclaimed.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | A cleanup outcome MUST distinguish reclaimed, retained and uncertain resources.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->
Source: spec/impl/v2/implementation.md#automatic-cleanup-preserves-durable-work | Cleanup MUST preserve durable task results, histories, checkpoints, audit metadata, operation receipts and recovery journals in the initial profile.
<!-- Allocation: V2_WORKER#AUTOMATIC_CLEANUP | behavior -->

Source: spec/impl/v2/implementation.md#definitions-carry-reusable-charters | The supplied executor MUST deliver the effective charter to its model.
<!-- Allocation: V2_CHARTER_CODEX#DELIVERY | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Retries internal to a supplied executor MUST remain owned by that executor.
<!-- Allocation: V2_WORKER#EXECUTOR | behavior -->

---
ID: V2_NATIVE
Title: Bounded native requests and delegation
Prerequisites: V2_CONTROL
Deliverable: Bounded provider requests, output-schema validation, deadlines, resource limits and durable child-task delegation.
Acceptance: Invalid output, provider errors, deadline/cancellation and parent-child recovery use the real controller path.
Dependency: V2_CONTROL | inferred | Uses the controller task contract and accepted state.
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | A child's terminal outcome MUST settle its parent-wait association before the parent resumes from that outcome.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | A delegated task MUST retain its parent task association.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | A delegation MUST commit its child task and parent-wait association atomically.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | A delegation MUST create a separately identified task.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | A delegation request MUST NOT change configured host, repository or provider access.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#delegation-is-a-task-handoff | The task's model-request accounting MUST include platform-mediated requests and configured child-task reservations.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | A native task MUST declare a finite model-request budget.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | A native task MUST declare its output schema.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | The native executor MUST terminate its attempt when the task's execution deadline is reached.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | The native executor MUST validate a result against that schema before accepting it.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->

Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | The native executor MUST support a bounded direct tool invocation followed by a model continuation.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | A native tool invocation MUST be restricted to the task's configured tool interface.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#the-native-executor-is-bounded | A native model continuation MUST consume the actual result of its preceding tool invocation.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | The native executor MUST support repeated configured tool calls within the task's finite execution bounds.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | An agent definition MUST explicitly configure its native provider-tool capabilities.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | The native executor MUST refuse a provider tool that is absent from the task's accepted configuration.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | The platform MUST NOT switch a task's executor automatically after a model or tool failure.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | An execution attempt MUST retain a tool call's exact argument binding before sending a mutation.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#native-tool-profiles-and-continuations | A native model continuation MUST use the recorded outcome of its corresponding tool call.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#repository-associations-are-versioned-metadata | A configured child repository association MUST be resolved when its parent workflow is accepted.
<!-- Allocation: V2_DELEGATE#DELEGATION | behavior -->

Source: spec/impl/v2/implementation.md#definitions-carry-reusable-charters | The native executor MUST deliver the effective charter to its model.
<!-- Allocation: V2_CHARTER_NATIVE#DELIVERY | behavior -->
Source: spec/impl/v2/implementation.md#native-charter-changes-preserve-prompt-continuity | A native charter change MUST be applied as a mid-conversation instruction.
<!-- Allocation: V2_CHARTER_NATIVE#UPDATE_COMPACTION | behavior -->
Source: spec/impl/v2/implementation.md#native-charter-changes-preserve-prompt-continuity | A native charter change MUST schedule a low-priority compaction that merges the change into the system prompt.
<!-- Allocation: V2_CHARTER_NATIVE#UPDATE_COMPACTION | behavior -->
Source: spec/impl/v2/implementation.md#native-charter-changes-preserve-prompt-continuity | Native charter changes MUST be debounced, so that a burst of them costs one compaction.
<!-- Allocation: V2_CHARTER_NATIVE#UPDATE_COMPACTION | behavior -->
Source: spec/impl/v2/implementation.md#native-charter-changes-preserve-prompt-continuity | A native compaction that follows a charter change MUST write its summary under the new charter.
<!-- Allocation: V2_CHARTER_NATIVE#UPDATE_COMPACTION | behavior -->
Source: spec/impl/v2/implementation.md#native-charter-changes-preserve-prompt-continuity | A native charter change arriving while a compaction is already due MUST be included in that compaction.
<!-- Allocation: V2_CHARTER_NATIVE#UPDATE_COMPACTION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A platform-owned model request rejected as transient MUST be retried with backoff and jitter within the task's remaining model-request allowance and execution deadline.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A retry of a throttled platform-owned model request MUST honor a valid provider retry-after value.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Every platform-owned model request dispatched by a retry MUST have an acknowledged durable request reservation before dispatch.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A platform-owned retry MUST revalidate its current task, attempt, ownership and cancellation state before dispatch.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Retry timing, jitter and circuit-breaker bounds MUST come from versioned metadata.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A platform-owned model request rejected as invalid MUST NOT be retried.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A context-window rejection MUST NOT be retried with unchanged input.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A provider policy refusal MUST be reported as a policy block.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A provider quota exhaustion MUST be distinguished from transient throttling.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Exhaustion of the retry allowance or deadline MUST leave an explicit reason-bearing blocked state without another provider dispatch.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A failed platform-owned provider request MUST retain bounded diagnostics sufficient to identify the observed failure class and available provider retry or quota information.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Retained provider diagnostics MUST exclude configured credentials and submitted prompt content.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Each newly dispatched platform-owned model attempt MUST carry a distinct retained request-attempt identity.
<!-- Allocation: V2_NATIVE#NATIVE | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A complete model reply that violates its declared output format MUST be retried with the specific validation error within the remaining request allowance and deadline.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A complete validated provider terminal response identifying an output-cap truncation MUST be continued from retained output within the remaining request allowance and deadline.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A platform-owned idempotent tool call that is unreachable or timed out MUST be retried within its accepted budget and deadline.
<!-- Allocation: V2_NATIVE_JOIN#NATIVE_INTEGRATION | behavior -->

---
ID: V2_PRODUCT
Title: Browser collaboration interface
Prerequisites: V2_CONTROL, V2_WORKSPACE
Deliverable: Simple browser collaboration and operational views matching shared tool information, repository browsing, live state updates and a right-side activity feed, with existing task actions through the common controller API.
Acceptance: Real browser actions and visible outcomes agree with controller state; no second store, permission engine or grant provisioning is introduced.
Dependency: V2_CONTROL | inferred | Uses the controller task contract and accepted state.
Dependency: V2_WORKSPACE | inferred | Browser acceptance includes a real coding task, checkpoint and recovered result.
Source: spec/impl/v2/implementation.md#initial-model-budget-policy | The default task model-request allowance MUST be the maximum finite value supported by the task representation.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | Task ownership MUST remain distinct from session and partition ownership.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The collaboration product MUST NOT create an independent agent inbox.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST expose task cancellation.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST present accepted task results.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST show task dependencies.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST support retrying a failed task.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST support task assignment.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The collaboration product MUST use the platform's task state and event interfaces.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The collaboration product MUST provide a browser user interface.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST provide an agent list.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST support task submission.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST show actual task status.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST show reported task progress.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST show available recent task output.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST link the task's published source checkpoints.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST present the information exposed through the shared agent interface.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST update displayed state without requiring a manual page reload.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#shared-product-state | The browser interface MUST display a live activity feed on the right side of the page.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->

Source: spec/impl/v2/implementation.md#limits-are-versioned-metadata | The browser interface MUST expose the effective limits applicable to displayed work.
<!-- Allocation: V2_PRODUCT#PRODUCT | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | A committed collaboration change MUST receive the next collaboration revision in commit order.
<!-- Allocation: V2_COLLAB_JOIN#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | A collaboration mutation MUST commit its record change and corresponding activity atomically.
<!-- Allocation: V2_COLLAB_JOIN#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-records-have-stable-identity-and-provenance | A collaboration change activity MUST identify its typed record subject.
<!-- Allocation: V2_COLLAB_JOIN#JOURNEY | behavior -->
Source: spec/impl/v2/implementation.md#the-supported-decision-queue-is-complete | The decision queue MUST include every open question directly routed to its viewer.
<!-- Allocation: V2_COLLAB_JOIN#QUEUE | behavior -->
Source: spec/impl/v2/implementation.md#the-supported-decision-queue-is-complete | The decision queue MUST include every open review directly routed to its viewer.
<!-- Allocation: V2_COLLAB_JOIN#QUEUE | behavior -->
Source: spec/impl/v2/implementation.md#the-supported-decision-queue-is-complete | A bounded decision-queue page MUST provide a continuation when more matching decisions remain.
<!-- Allocation: V2_COLLAB_JOIN#QUEUE | behavior -->
Source: spec/impl/v2/implementation.md#the-supported-decision-queue-is-complete | A notice outside the decision queue MUST NOT substitute for an owed decision's queue entry.
<!-- Allocation: V2_COLLAB_JOIN#QUEUE | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A task with an open collaboration wait MUST be ineligible for a new execution attempt.
<!-- Allocation: V2_COLLAB_JOIN#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A requested collaboration wait MUST be committed before initiating the current attempt's quiescence.
<!-- Allocation: V2_COLLAB_JOIN#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A task suspended on a collaboration wait MUST release its execution slot after its current execution has stopped.
<!-- Allocation: V2_COLLAB_JOIN#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-waits-are-separate-from-execution-prerequisites | A resolver's terminal transition MUST commit its typed outcome, affected wait resolutions and delivery associations atomically.
<!-- Allocation: V2_COLLAB_JOIN#WAITS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | An answer delivery MUST target its active asker or otherwise the record's current responsible owner.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A committed resolution delivery MUST carry an identity that deduplicates its delivery retries.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | Pending resolution delivery MUST remain recoverable after a controller restart.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A committed resolution that makes a waiting task eligible MUST wake it through the existing platform command mechanism.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A resolution notice MUST be delivered to an active executor at a supported safe input boundary.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A resolution notice MUST NOT cancel a running side-effecting tool.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A resolution-delivery acknowledgment MUST follow durable retention of the outcome for its receiving continuation.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A repeated resolution delivery MUST NOT start a second continuation for the same resolution occurrence.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#committed-resolutions-reach-their-consumers | A resolution notice MUST NOT reopen a terminal task.
<!-- Allocation: V2_COLLAB_JOIN#CONSUMERS | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | The browser interface MUST expose every supported collaboration operation permitted to its caller.
<!-- Allocation: V2_COLLAB_JOIN#SURFACES | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | The shared agent interface MUST expose every supported collaboration operation permitted to its current caller.
<!-- Allocation: V2_COLLAB_JOIN#SURFACES | behavior -->
Source: spec/impl/v2/implementation.md#collaboration-surfaces-preserve-existing-authority | Adding collaboration operations MUST NOT expand an already accepted task's configured tool profile.
<!-- Allocation: V2_COLLAB_JOIN#SURFACES | behavior -->

Source: spec/impl/v2/implementation.md#definitions-carry-reusable-charters | A session MUST retain the charter revision effective at each turn boundary.
<!-- Allocation: V2_CHARTER_JOIN#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#charter-changes-apply-at-safe-boundaries | A charter change MUST apply to a running session at its next turn boundary.
<!-- Allocation: V2_CHARTER_JOIN#ADOPTION | behavior -->
Source: spec/impl/v2/implementation.md#charter-changes-apply-at-safe-boundaries | A charter change MUST NOT apply between a tool call and the delivery of its result.
<!-- Allocation: V2_CHARTER_JOIN#ADOPTION | behavior -->
Source: spec/impl/v2/implementation.md#charter-changes-apply-at-safe-boundaries | A session MUST check for a pending charter change when it loads.
<!-- Allocation: V2_CHARTER_JOIN#ADOPTION | behavior -->
Source: spec/impl/v2/implementation.md#charter-changes-apply-at-safe-boundaries | A charter change MUST NOT wake an idle session solely to adopt it.
<!-- Allocation: V2_CHARTER_JOIN#ADOPTION | behavior -->
Source: spec/impl/v2/implementation.md#charter-changes-apply-at-safe-boundaries | A charter change MUST reach the supplied executor only at a supported step boundary.
<!-- Allocation: V2_CHARTER_JOIN#ADOPTION | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | Every turn reply MUST carry structured charter-reference metadata.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | A turn reply's charter-reference metadata MUST identify the authoritative charter revision effective at that turn's boundary.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | A turn reply's charter-reference metadata MUST identify the specific charter clauses governing its response or actions.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | A charter clause reference MUST resolve to the clause's source file, section, and exact sentence within the identified charter revision.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | A turn reply's durable record MUST retain its charter-reference metadata.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#every-turn-reply-carries-charter-references | A record that cites a turn reply for audit inspection MUST preserve its association with that reply's charter-reference metadata.
<!-- Allocation: V2_CHARTER_JOIN#REPLIES | behavior -->
Source: spec/impl/v2/implementation.md#reference-checks-produce-audit-information | The platform MUST check each turn reply's charter references against the authoritative charter revision effective for that turn.
<!-- Allocation: V2_CHARTER_JOIN#AUDIT | behavior -->
Source: spec/impl/v2/implementation.md#reference-checks-produce-audit-information | The platform MUST record invalid or mismatched charter references with the reply they came from for audit inspection.
<!-- Allocation: V2_CHARTER_JOIN#AUDIT | behavior -->
Source: spec/impl/v2/implementation.md#reference-checks-produce-audit-information | A charter reference check MUST NOT be treated as proof of behavioral compliance.
<!-- Allocation: V2_CHARTER_JOIN#AUDIT | behavior -->

---
ID: V2_QUALIFY
Title: Integrated journey, recovery and release
Prerequisites: V2_CONTROL, V2_WORKSPACE
Deliverable: The runnable application, combined recovery, protected merge/release path and declared scale evidence.
Acceptance: Actual first journey and later mixed-load/recovery/release checks pass; supplied-executor outcomes remain distinct from accepted evidence.
Dependency: V2_CONTROL | inferred | Uses the controller task contract and accepted state.
Dependency: V2_WORKSPACE | inferred | Actual coding executor and checkpoint producer are required for integrated qualification.
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | A cancellation acknowledgment MUST distinguish requested cancellation from verified execution termination.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | A cancellation request MUST commit cancellation intent and its stop command atomically.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | An automatic retry MUST wait until prior execution is known unable to continue.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | An automatic retry MUST wait until relevant uncertain external effects are reconciled.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | Reconnecting or resuming an attempt MUST NOT reset its execution deadline.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | Reconnecting or resuming an attempt MUST NOT reset its recorded model-request accounting.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#cancellation-commits-intent-before-interruption | Successful completion and cancellation MUST be ordered by the controller's committed task revision.
<!-- Allocation: V2_RECOVERY#CANCEL | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | Change preparation MUST produce a deployable artifact linked to its source revision.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | Human authorization MUST NOT bypass applicable mandatory validation.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The final rollout MUST require human authorization bound to that artifact and target.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The final rollout MUST require successful applicable mandatory validation for the exact artifact and target.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The system MUST support compiling its own runtime changes.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The system MUST support editing its own runtime source.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The system MUST support submitting its own runtime changes.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#changes-can-be-prepared-without-per-change-human-approval | The system MUST support testing its own runtime changes.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#configured-actions-and-automatic-merge | A merge MUST NOT trigger a rollout without the rollout's required human authorization.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#configured-actions-and-automatic-merge | An agent MAY merge a change after protected checks have passed for its exact revision.
<!-- Allocation: V2_RELEASE#RELEASE | permission -->
Source: spec/impl/v2/implementation.md#configured-actions-and-automatic-merge | External actions MUST remain subject to the repository and provider access controls in force.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | A test result MUST identify the exact source and executable it exercised.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Native requirement coverage MUST use the retained traceability tool's extracted requirement identities.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Planning associations MUST remain distinct from implementation and executed-test evidence.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Release acceptance MUST exercise controller-restart recovery.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Release acceptance MUST exercise the supported end-to-end task journey.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Release acceptance MUST exercise worker-loss recovery.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Release acceptance MUST verify rejection of stale task owners.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | Release acceptance MUST verify that a settled external effect is not repeated by recovery.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#evidence-identifies-what-actually-ran | The release review MUST assess uncovered critical paths identified by coverage measurements.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | Every coding workspace MUST be able to run repository builds and tests.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | Initial production qualification MUST exercise 256 concurrent outstanding real native model requests on each execution worker.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | Initial production qualification MUST exercise eight active coding workspaces on each execution worker.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | Initial production qualification MUST use four hosts.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | Locally queued tasks MUST NOT be counted as outstanding native model requests.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#initial-four-host-qualification | The native-request and coding-workspace targets MUST be exercised during the same test interval.
<!-- Allocation: V2_SCALE#SCALE | behavior -->
Source: spec/impl/v2/implementation.md#integrate-from-the-first-increment | A component delivery MUST include a compiled real-consumer integration.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#integrate-from-the-first-increment | A component delivery MUST include the results of its relevant tests.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#integrate-from-the-first-increment | The execution plan MUST define one runnable end-to-end acceptance path before component work is divided.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#integrate-from-the-first-increment | The integrated acceptance path MUST be exercised after changes to its interfaces.
<!-- Allocation: V2_RELEASE#EVIDENCE | delivery-process -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | A recovery attempt MUST preserve the original task identity.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | A retry MUST preserve the original external-operation identity where the receiving interface supports deduplication.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | A saved executor conversation MUST be associated with the same task before it is resumed.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | An external effect with an uncertain outcome MUST be reconciled before the platform retries that effect.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | An interrupted attempt MUST retain its last committed status and checkpoint references.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | Recovery MUST NOT assume that uncheckpointed workspace content survived worker loss.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | Recovery that cannot resume available saved executor history MUST create a new attempt.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | Resuming a conversation against a restored workspace MUST identify the recovered checkpoint to the executor.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#interrupted-work-is-reconciled | The platform MUST NOT report an interrupted command as stopped until its worker confirms termination or the worker is known unable to continue it.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#sensitive-authority-stays-outside-agent-writable-work | Worker credentials MUST be limited to the deployment's intended operations.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#sensitive-authority-stays-outside-agent-writable-work | Logs and task results MUST NOT include credentials.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#sensitive-authority-stays-outside-agent-writable-work | Production rollout authority MUST remain outside the agent-writable workspace.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#sensitive-authority-stays-outside-agent-writable-work | Release acceptance evidence MUST be produced by a protected checker outside the agent-writable workspace.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#sensitive-authority-stays-outside-agent-writable-work | The worker MUST NOT receive platform-administration credentials.
<!-- Allocation: V2_RELEASE#RELEASE | behavior -->
Source: spec/impl/v2/implementation.md#the-combined-scale-target-remains | Each counted host MUST support at least 1,000 active agents concurrently.
<!-- Allocation: V2_LATER_SCALE#TEN_HOST_BEHAVIOR | behavior -->
Source: spec/impl/v2/implementation.md#the-combined-scale-target-remains | Each counted host MUST support at least 10 active workspaces concurrently.
<!-- Allocation: V2_LATER_SCALE#TEN_HOST_BEHAVIOR | behavior -->
Source: spec/impl/v2/implementation.md#the-combined-scale-target-remains | Scale qualification MUST exercise both per-host targets during the same interval.
<!-- Allocation: V2_LATER_SCALE#TEN_HOST_BEHAVIOR | behavior -->
Source: spec/impl/v2/implementation.md#the-combined-scale-target-remains | The production architecture MUST support at least 10 hosts operating simultaneously.
<!-- Allocation: V2_LATER_SCALE#TEN_HOST_BEHAVIOR | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A local transport close or process cleanup MUST NOT establish that an ambiguous provider request had no effect.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A platform-owned retry of an uncertain provider effect MUST use the receiving interface's supported outcome reconciliation or deduplication before resending that effect.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | Platform recovery of a supplied executor MUST NOT create another session or repeat an uncertain start merely to implement a provider retry.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->
Source: spec/impl/v2/implementation.md#platform-owned-model-retries | A new circuit-breaker probe after a prior unknown outcome MUST NOT replay the original uncertain request.
<!-- Allocation: V2_RECOVERY#RECOVERY | behavior -->

