# Collaboration workflows for v2

Prepared on 2026-10-16 after the request to restore more of the shared collaboration functionality. This is the concrete specification and implementation work package for that expansion. Exact requirement adoption, protocol changes and new implementation are not claimed by this proposal. Scope: usable collaboration over existing controller records, browser and MCP, preserving v2 storage, security and execution choices.

## Recommended coherent deliverable

Add versioned documents and immutable comments; routed questions and document/review decisions; a complete queue for the supported recipients; task waits on concrete task/question/review resolvers; and committed resolution notifications that make the waiting work eligible again. Browser and MCP use the same canonical operations and receipts. Ordinary work gains no new mandatory human approval. A product answer or review never authorizes deployment, host changes, credentials or access.

Keep accepted execution prerequisites immutable. Add separate mutable collaboration waits, with cycle checks over waits and parent links. Separate workflow waiting from scheduler ineligibility caused by capacity, failed prerequisites or reconciliation. Resolve a wait regardless of positive or negative outcome; deliver the actual outcome without treating it as task success or a positive go-ahead.

## Exact original requirement crosswalk

All tuples below use file `spec/decisions/collaboration-platform/build.md`. The section identifier and quoted sentence together identify the original requirement. Line references describe the current inspected source; identities are the file, section and sentence, not the line numbers.

### Task waits and coordination

- `coordination-logic-lives-in-the-collaboration-platform` (line 237): "Fanning work out, collecting results, looping with a cap, and re-delegating MUST be implemented in the collaboration platform as tasks and assignments rather than as a layer in the agent platform."
- `a-block-is-a-wait-on-a-resolver` (line 241): "A task MUST be blocked only while it waits on a task, a question, a document's operator review, or a review record."
- Same section (line 243): "A task that has an open wait MUST be blocked."
- Same section (line 245): "A wait on an agent, a team, or an external party MUST be refused, so that nothing waits on a principal that may vanish."
- Same section (line 249): "A resolver MUST end the waits on it in the transaction that resolves it, whatever its outcome."
- Same section (line 251): "A task whose last wait ends MUST return to the status it held before its first wait, with its assignee."
- Same section (line 253): "The event that unblocks a task MUST carry the outcome of each wait that ended."
- Same section (line 255): "A wait that would close a cycle through waits and parent links MUST be refused."
- Same section (line 257): "A wait on a resolver that is already closed MUST be refused."
- Same section (line 259): "A task MUST be at most the configured number of levels below its top-level ancestor."

Current partial behavior: `borg-runtime/src/controller/operations.rs:250` implements one configured child per current waiting parent, atomic child/wait creation, depth and cycle checks, and execution-slot release; `src/controller/operations/admission.rs:43` checks immutable task prerequisites. There is no general typed resolver-wait interface in `src/protocol.rs:454` or `src/controller/collaboration_calls.rs:16`.

Needed v2 additions: resolver kind/identity and typed terminal outcome; expected-revision add/remove wait operations; atomic resolution plus history/wake records; separate workflow-wait status; bounded multiple waits; continuation after verified stop/reconciliation. Restoring the prior logical workflow state must not resurrect a stopped attempt or undo cancellation/terminal task state. Existing prerequisites, deadlines, budgets and uncertain-effect rules remain independent admission conditions.

### Questions, comments and answers

- `a-question-outlives-its-asker` (line 93): "An open question MUST remain cancellable or supersedable by a principal other than its asker, so that a retired asker leaves no question that cannot be closed."
- Same section (line 95): "A cancel or supersede MUST record the acting principal and a reason."
- Same section (line 97): "A cancel or supersede MUST keep the original asker as the question's poser."
- Same section (line 99): "A supersede by a principal other than the asker MUST pose the replacement under the acting principal and link the old question to the new one."
- Same section (line 101): "When a record's owner changes, its open questions MUST stay open."
- `a-decision-is-recorded-under-the-principal-that-made-it` (line 107): "A question MUST be answered, and its answer superseded or withdrawn, only by the principal it is routed to, from an authenticated session."
- Same section (line 109): "An answer MUST record the authenticated principal and the authentication path it came through."
- Same section (line 111): "An answer whose author is not an authenticated person MUST NOT be presented as an operator's answer."
- Same section (line 125): "An answered question MUST NOT be cancelled or superseded."
- Same section (line 131): "A posted comment, a question's typed fields, and a recorded answer MUST be immutable."
- Same section (line 137): "A blocking question MUST take no default answer."
- Same section (line 139): "A question's prompt MUST state the recommended option."
- Same section (line 141): "An answer MUST be delivered to the asker if the asker is active, and otherwise to the current owner of the record."
- Same section (line 143): "A question routed to an operator MUST ask exactly one decision."
- Same section (line 145): "An option offered to an operator MUST NOT describe a step the operator would take by hand."

Current behavior: no question/comment/answer variants in the canonical Operation or MCP ToolArgs enums. Needed v2 additions: immutable posed content, append-only answer/revision records, explicit cancel/supersede/withdraw/moot transitions, recipient and current responsible owner, and durable delivery association. Distinguish superseding an answer from superseding its already-answered question. A view must show author kind and provenance honestly.

Direct-recipient support is a concrete initial slice. Team membership/routing and imported answers remain unsupported unless explicitly included; do not claim the team-answer or import clauses complete. Record ownership may change without changing the accepted execution agent, definition, session or inputs. Current AssignTask refuses retargeting the accepted definition (`src/controller/operations.rs:134-171`); this increment must not silently reinterpret it.

### Documents, versions and reviews

- `a-durable-store-behind-it` (line 31): "The service MUST refuse a content id a client supplies that differs from the one the service computed."
- Same section (line 33): "Pruning document versions MUST NOT remove a current, an approved, or a pinned version, or one that a served definition version references."
- Same section (line 35): "A document version number MUST NOT be reused."
- `a-decision-is-recorded-under-the-principal-that-made-it` (line 115): "A document's operator review MUST be decided only by an operator from an authenticated session."
- `the-decision-queue-is-complete-by-construction` (line 227): "Withdrawing a document from operator review MUST record the acting principal and a reason."
- Same section (line 229): "Withdrawing a document from operator review MUST publish an event to every operator."
- Same section (line 231): "Archiving or deprecating a document in operator review MUST be refused while its review is open."

Current behavior: no document-version or review-record operations. Needed v2 additions: bounded document content, immutable version identity/content, current version pointer, comments, review targeted to exact version/content identity, decision provenance, and atomic review-resolution/wait events. Editing creates a new version and does not retarget an existing review or inherit its approval. Keep versions initially; introduce pruning only with the reference protections above. A standalone review can be a concrete decision over an exact task/result/checkpoint reference; its schema and allowed terminal outcomes need v2 wording because the original only identifies a review record as a resolver.

Document review is optional product workflow. It does not automatically gate all edits, all definitions or task execution. Declarative definition composition from approved documents is conditional in the original and is not adopted by adding versioned documents.

### Decision queue and positive go-aheads

- `the-decision-queue-is-complete-by-construction` (line 223): "The decision queue MUST include every open decision routed to its viewer directly or through a team, and every document in the viewer's operator review."
- Same section (line 225): "A decision owed to a person MUST NOT reach that person only through a message or a pointer outside the queue."
- `a-decision-is-recorded-under-the-principal-that-made-it` (line 117): "A gated action MUST proceed only on a recorded positive go-ahead whose typed fields match the action."
- Same section (line 119): "A denial, an absent block, or a cancelled, withdrawn, moot, expired, or imported answer MUST NOT count as a go-ahead."
- Same section (line 121): "A go-ahead MUST bind to one principal, so that a reassigned gated task needs a new go-ahead."

Current behavior: release/check observations exist, but no product decision queue. Needed v2 additions: canonical query over all supported open question/review records, bounded paging that does not hide owed decisions, and stale-view-safe resolution. Recommend informational questions and explicit resolver waits first; optional typed product go-aheads only where the workflow explicitly requests a gate. No default gate is added to ordinary work. If product gates are included, expiry, principal binding, descendant propagation and operator-only removal require their original safeguards; a generic boolean approval field is insufficient.

The original explanatory paragraph at line 105 explicitly says product questions/go-aheads are not grants, do not widen authority, and do not select human approval of runtime decisions. Existing final-rollout authorization and mandatory protected validation (`spec/impl/v2/implementation.md:719-731`) remain separate and cannot be satisfied by a product review.

### Committed changes and resolution wakes

- `changes-within-a-tenant-are-ordered-by-one-revision` (line 21): "Changes within one tenant MUST be ordered by one gapless revision in commit order."
- `it-publishes-every-record-change` (line 159): "Every change to a record MUST be published to that record's topic."
- Same section (line 161): "A published event's id MUST derive from the service's own ordered revision, so that the platform applies each change exactly once."
- Same section (line 165): "An event payload MUST identify each record it refers to by both its cited reference and its durable id, fixed at commit."
- Same section (line 167): "An event payload MUST carry the fields that changed, cut to the payload bound with a truncated flag."
- `it-keeps-no-delivery-mechanism-of-its-own` (line 213): "The service MUST NOT keep an agent inbox, a notification poll, or a liveness watchdog of its own."

Current behavior: controller history, command wakes, browser activity and durable memory notices exist. `src/controller/memory_notifications.rs:290` implements memory notice observation, prepared batches and acknowledgment. Existing memory rules forbid new sessions/model requests from those notices (`spec/impl/v2/implementation.md:199-207`). Keep those rules unchanged.

Needed v2 additions: transactionally record each question/review/wait resolution, its intended task/session destination and stable dedupe identity; use the platform's existing wake/command machinery to resume eligible waiting work at a safe boundary. Recover undelivered resolution after restart or lost acknowledgment. Do not interrupt running side effects, invent a new task/session, extend accepted deadlines/budgets, or retry uncertain work. Terminal tasks remain terminal; follow-up work requires a separately accepted task. The change is an actionable resolution, not a memory notice.

This bounded direct-delivery path is only partial relative to the original general record-topic subscription and person-topic requirements. Browser activity alone is not delivery to an agent. Do not claim general publish/subscribe or person notifications merely from adding resolution wakes.

## Compatibility distinctions to record explicitly

1. **Identity:** v2 host/SSH trust remains (`implementation.md:637-663`). Server-bound provenance and current-attempt identity can distinguish trusted-host operator actions from agent actions, but a client-provided person name cannot establish authenticated-human identity. Record the actual authentication path. If the current transport cannot distinguish an operator from code sharing that access, mark original authenticated-person/operator-decision clauses unsupported; do not invent PKI, sign-on, access grants or a role engine. Team membership, signed person attestations and external imports remain outside this batch.
2. **Lifecycle:** keep imperative session ownership and current cancellation/recovery. Questions must outlive their asking attempt and route through the record's current responsible owner. Do not add agent-wide pause/retirement, a desired root-session set or definition-source composition implicitly. Original `lifecycle-intent-is-the-only-pause-path` retirement side effects remain unimplemented until lifecycle scope is explicitly added.
3. **Storage/security:** use current controller durability, application schema/migration discipline and existing limits metadata. Original tenant encryption, self-durable scale/restore guarantees and service-wide multi-node behavior are not reintroduced by collaboration records. In particular, the document-under-tenant-keys clause remains a recorded v2 difference.
4. **Execution semantics:** preserve `implementation.md:517` immutable prerequisites, `:519` successful-prerequisite admission, `:489-495` stop/effect reconciliation and original deadline/accounting. A resolved workflow wait releases that wait; it does not prove execution eligibility or success. Preserve current accepted definition/session identity on resumption.
5. **Explicit deferral adjustment:** narrow the broader-document/question/review deferrals at `implementation.md:67,87` to the supported increment. Keep unrelated exclusions at `:143-145`, memory behavior at `:199-207`, and security exclusions at `:635-647`. Reconcile the proposal against current v2 capability/phase allocations; the original specification remains unchanged.
6. **Browser/MCP:** extend the existing shared operation layer and exact replay/currentness handling to the new records. Both surfaces show versions, question/review status, waits, outcomes and decision queues; writes retain the caller restrictions of their operation. Displaying an operator-only action in MCP does not grant authority to perform it.

## Integration order and proposed discriminating acceptance cases

First define actor/provenance, record identities, revisions and the typed resolver outcomes. Then documents/comments and questions can be implemented independently over that agreed boundary. Integrate resolver waits and the decision queue, then committed resolution wakes and browser/MCP parity. No new storage engine, general event framework or universal workflow language is needed.

Proposed tests, not executed evidence: simultaneous answer/review resolution; exact replay after lost acknowledgment; closed resolver and parent/wait cycle refusal; resolution atomically ending multiple waits with typed negative outcomes; owner change without erasing questions or mutating execution inputs; editing a document while its earlier version remains in review; withdrawal/archive races; agent answer never rendered as operator authorization; resolution during cancellation/side effects; restart before delivery or after uncertain acknowledgment; duplicate wake without duplicate continuation; unchanged memory notices; full supported decision queue across pages; browser and MCP reading the same committed outcome.

## Whole-component implementation batches

The coordinator owns shared protocol types, module registration, database/wire migration, the MCP catalogue and routing, shared limits metadata and integrated acceptance. Four component batches use those frozen contracts and each delivers its domain behavior, browser forms/views, MCP operations through the shared router, and meaningful tests.

1. **Task coordination:** new collaboration/work types, controller/collaboration_work and product/collaboration_work modules, with a collaboration_work integration test. Explicit multiple resolver waits remain separate from immutable execution prerequisites. Test cycle/refused closed resolver, concurrent resolution, negative outcomes, cancellation, restart and no premature execution.
2. **Questions and decision queue:** new collaboration/questions, controller/collaboration_questions and product/collaboration_questions modules, with a collaboration_questions integration test. Include immutable questions/answers, recipients, responsible owners, exact replay and bounded complete queues. Product answers never constitute protected deployment authorization.
3. **Documents and review:** new collaboration/documents, controller/collaboration_documents and product/collaboration_documents modules, with a collaboration_documents integration test. Include immutable document versions/comments, current-head conflicts, exact-version review, withdrawal and restart. Review never transfers automatically to an edited version.
4. **Resolution delivery and actual consumers:** new collaboration/notifications, controller/collaboration_notifications and worker/collaboration_notice modules, with collaboration_notifications and collaboration_notice_consumers integration tests. This owner integrates the existing native/Codex worker boundary and browser live updates after the common contract is ready. Commit the resolved state and delivery association before wake; preserve cancellation, original accounting and uncertain-effect handling.

These are work orders, not four disconnected implementations. Each uses the existing controller store, mutation receipts and actual callers. Owners may build independently once the shared contract compiles; final qualification joins their completed work. Root remains the sole maintained-source integrator.

## Common compatibility and acceptance work

Current task activity requires a task ID. New document/question/review activity therefore needs a typed non-task subject without fabricated task records. A collaboration product revision is distinct from the existing controller operation/receipt revision.

Current startup accepts database schema 12 and wire 11. The expansion needs a deliberate retained-state migration and wire compatibility disposition; it must not reset existing tasks, memory, operation receipts or repositories. Exact version values are determined with the concrete protocol patch, not asserted by this proposal.

Adding tool schemas changes the native aggregate validation size. Measure the actual serialized catalogue against metadata bounds (the current diagnostic v4 profile uses schema_bytes 19746). Preserve already-accepted profiles rather than silently widening their tools or limits.

The first complete collaboration journey is: an agent delegates a task, poses a routed blocking question, releases its execution slot, receives the committed answer without duplicate continuation, resumes with its original session association, and submits a versioned document or exact code result for review. The same records and decisions are visible through MCP and the browser. Restart and lost-ack variants must retain the original question, answer, document version, waits and outcome. This journey is not complete merely because individual forms or handlers work.

## Provenance

The current user direction was: "I think we need to bring a lot of those requirements in for it to be functional". The exact-source crosswalk was prepared at borg-runtime/artifacts/v2/collaboration-crosswalk-20261016.md. It is a source reading, not executed behavior. The root coordinator also reviewed current protocol, controller and browser source and assigned the component planning review independently. Current implementation coverage remains tied to its earlier captured specification/source; this proposal does not enlarge that report's denominator or count as implementation evidence.
