# Collaboration MCP Integration

The selected feature is one collaboration MCP surface for native and Codex tasks,
layered over the existing controller API. This document is the concrete engineering
plan. The main specification owns its requirements; implementation and deployed
qualification remain separate evidence. No new permission engine, credential,
provider service, dependency or public endpoint is selected here.

## One Canonical Operation Layer

The controller's SQLite records and operation handler remain authoritative. MCP
adapts tool calls to those operations. Workers retain their private API; browser
handlers can continue to call the same operation layer in-process. MCP does not
create a second collaboration store or a competing runtime metadata API.

Borg is the MCP client for native tasks. It converts the shared catalogue to
Responses function tools, calls tools/call itself, and supplies the exact recorded
function_call_output. Responses remote MCP is not the selected integration. The
server is a handler on the existing private controller listener. Native and Codex
use the existing private connectivity; there is no public provider-facing tunnel.

Codex receives a task-scoped MCP configuration through its pinned app-server
interface. Its personal/global configuration is not modified. A controlled local
fixture has established HTTP discovery/call viability; exact protocol negotiation,
profile configuration and actual Borg calls still need qualification. The fixture
observed the pinned client's 2025-06-18 MCP negotiation. That is not a claim that it
implements the newer MCP revision or that the collaboration server already exists.

## Collaboration Surface

The exact closed tool schemas are frozen with implementation. The operation
families and allowed meanings are:

| Surface | Existing source of truth | Agent operation |
| --- | --- | --- |
| Tasks | Task and accepted TaskInput | List/get tasks, dependencies, assignment, status, bounded progress/output and accepted results. |
| Agents and definitions | Agent directory and immutable definition records | List/get agents, their tasks and referenced definition revisions. |
| History and attempts | Task history and current attempt associations | Bounded history/attempt queries; retain actual generation, phase, stop and uncertainty facts. |
| Checkpoints | Selected checkpoint, accepted result and recorded publication history | Read exact existing commit/reference associations; do not infer an acknowledged checkpoint from an uncommitted publication. |
| Repository source | Accepted repository/start revision and acknowledged checkpoint references | Bounded tree, file, literal search, history and diff reads at exact commits through trusted configured local repository material; no arbitrary host paths, fetch or workspace commands. |
| Explicit memory | Existing agent memory records | List keys/read and revision-checked write for the caller's agent; caller identity is host-bound. |
| Task transitions | Existing AssignTask, CancelTask and RetryTask | Apply current revision, lifecycle, definition and reconciliation rules; do not fabricate successful execution or extend a deadline. |
| Configured child creation | DelegateTask and accepted child template | Create the configured child with parent/wait/budget accounting and actual quiescent continuation. No arbitrary SubmitTask bypass. |
| Worker capacity | Worker registrations and current admission records | Read registered and occupied capacity with its actual observation revision. Registration is not measured throughput. |
| Release and checks | Existing release candidates and protected stage views | Read candidate/check status and evidence associations. Exclude grant payloads, checker controls and rollout actions. |

Task attempts, selected checkpoints and accepted results already present on Task
are reused, not copied into separate metadata records. New bounded queries expose
history or capacity only where no existing projection provides it. The API keeps
one observation revision and the original mutation commit context. Empty partial
pages carry a continuation cursor; filtering cannot silently turn them into end of
history. Opaque effect payloads, command journals and executor-private identifiers
are not a general-purpose introspection surface.

The initial product is task-first. Documents, questions, review workflows and
archive/dwell/notification automation remain deferred by the current v2 profile.
Exposing collaboration through MCP does not silently select those products.

## Scope And Enforcement

The selected deployment has one trusted tenant space and no project entity.
There is no existing project ID to enforce. The MCP layer uses this same space;
it cannot claim private-project or hostile-worker isolation. The ordinary HTTP
API relies on host/origin/SSH trust, and coding processes can reach that API.
An MCP allowlist therefore constrains tool behavior but is not a security boundary
against arbitrary code bypassing it. A later private-project requirement would
need a separately designed and authorized boundary.

The host binds caller task, attempt, generation and worker incarnation outside
model tool arguments. These associations do not authenticate themselves. The
canonical controller transaction checks current caller state and operation rules
before committing a new mutation and its receipt. Model arguments cannot replace
the caller, controller URL, credentials, repository access or protected authority.
Known credential values and protected control/grant fields are withheld from
agent-visible projections. No tool grants checker, merge or rollout permission.

Current-rule collaboration writes remain available; the additional worker/release
observations are read-only. Memory writes belong to the caller's own agent.
Configured child creation preserves the existing template and budget reservation.
Unrestricted independent task creation is not smuggled through this seam: the
model cannot choose new repository/host/provider access or evade delegation limits.

## Profiles And Provider Tools

New native and coding definitions expose the shared collaboration interface.
The Definition carries the single immutable execution_profile, with CollaborationV1 required explicitly for fresh native and coding tasks. Task and child-template records do not carry competing profile copies. Configured children have their own Definition. Native web
search is supported when explicitly enabled; absent web_search selection means disabled. Missing profile on a historical input preserves its recorded bytes, but does not authorize fresh submission, retry or execution.
The immutable agent template carries model and reasoning effort. The current selected values are gpt-6-astra and max for both native and Codex execution. Accepted task and attempt bindings retain their template revision; worker-wide settings do not override it. A model cannot enable an unadvertised tool,
change its accepted profile, or switch its executor after an error.

The implementation profile uses a closed tool catalogue, bounded schemas, one
owned function call per native response, finite retained context and existing
request/deadline/output limits. These limits do not reintroduce deferred cost/token
budget work. Hosted web-search calls and local MCP operations are counted and
reported separately. Search results retain their source/citation data; they are
untrusted input, not permission to invoke another tool or change policy.

Additional provider capabilities remain disabled: file search needs selected
uploaded files/vector-store ownership and retention; code interpreter needs its
container/data lifecycle; computer use and hosted shell need an execution/access
boundary; image generation needs its output/resource profile; third-party remote
MCP/connectors need an exact service, data-sharing and credential decision. None
is enabled by the phrase "etc" or by the collaboration MCP selection.

## Repository Read Adapter

The same MCP catalogue exposes repository tree, file, literal-search, history and diff reads. The caller selects a recorded source association and exact commit, not an arbitrary repository URL or host path. Trusted host configuration maps permitted repository identities to already available local bare repositories. An empty mapping exposes no repository content; missing objects return unavailable. This adds no repository acquisition, writable clone, second metadata store or external access grant. Native tasks can analyze source and request deliberate coding work only when editing or building is needed.

Git object reads occur outside controller database transactions, with caller currentness checked around the read and bounded process/output/scan work. The adapter does not execute repository hooks, external diff/text conversion, submodules or commands from source content. Returned content is untrusted tool data and retains its exact revision; binary/oversized/refused reads are explicit. Integration tests exercise scope/path escapes, config-mediated helper execution, changed/missing objects, cancellation and equivalent native/Codex results. Concrete local repository mappings are deployment inputs, not model-selected parameters.

## Worker Repository Material

The selected worker layout shares one local object cache per configured repository across separate attempt worktrees. A validation worktree starts from the acknowledged exact checkpoint and is distinct from the coding worktree. A worker cache is disposable; central checkpoint publication remains the recovery boundary. The controller MCP reader uses the central configured committed repository rather than another worker's dirty worktree. Worker-local cache maintenance and worktree lifecycle are coordinated without a shared writable filesystem across hosts.

## Native Loop And Durable Replay

1. Validate the accepted profile, remaining finite allowance, deadline and context
   bound. Persist and obtain acknowledgment for the next model-request reservation.
2. Recheck current task ownership immediately before the single provider POST.
   Preserve response completion, exposure and observed usage separately from local
   schema acceptance. A lost provider response is not automatically retried.
3. A final answer passes the original output schema and controller acceptance.
   Otherwise validate the selected function name and bounded arguments.
4. Before a mutation, retain its exact call/body association and stable Borg
   operation identity. MCP/JSON-RPC request IDs and provider call IDs are transport
   correlations, not substitutes for the durable operation identity.
5. Execute through the controller. A lost reply looks up/retries the same operation;
   changed input under that identity refuses. Retain the actual outcome and commit
   context before the next model request. A refusal stays a refusal.
6. Recheck currentness, cancellation and deadline at every next-use boundary.
   Cancellation does not undo committed writes. Restart preserves original
   reservations, call identities, outcomes and unresolved effects.

The existing fixed two-request path cannot satisfy this feature. Its controller
acceptance currently requires exactly one reservation at observation and two at
completion; that changes together with the variable-step worker/native loop.
Old task/receipt bytes keep their original meaning. New immutable profiles and
explicit private wire/storage migration dispositions precede deployment. No
implicit upgrade of old accepted tasks, invented receipts or automatic fallback
is permitted. The actual 58-file shared snapshot and runtime lock stay unchanged.

## Owned Batches And Acceptance

| Batch | Owner and independent work | Actual integration closure |
| --- | --- | --- |
| Controller/MCP | Existing controller owner: bounded projections, schemas, caller checks and exact receipt replay using existing transactions. | V2_STORE:COLLAB_MCP with real SQLite and private HTTP. |
| Native | Existing native owner: repeated bounded steps, preserved provider items, profile-aware function/search decoding and controlled-peer tests. | V2_NATIVE_JOIN with actual controller read/write/read, then selected-provider evidence. |
| Codex/profile | Existing adapter owner: pinned per-task HTTP MCP discovery/calls, profile configuration and no-model compatibility fixture. | V2_E2E with actual Borg reads/writes, cancellation and restart. |
| Integration | Existing coordinator: shared protocol, worker journal/loop, spec/allocation updates and one canonical composition. | Both executors use the same tool semantics; old receipts/formats remain honestly classified. |

Discriminating tests include exact write replay after lost acknowledgment; changed
arguments under one ID; stale/expired caller; cancel after commit before reply;
restart between each durable boundary; attempted protected-control calls; hidden
credential/grant fields; wrong cursor/query scope; empty filtered pages; bounded
history/result/context; disabled web search; unknown provider output; actual
multi-step read/write/read with schema-valid final result; Codex/native catalogue
and outcome parity; unavailable MCP with no fallback; and configured child wait
without duplicate creation or lost budget. Controlled peers and transport fixtures
are not reported as real-provider, actual Codex-task or scale qualification.

No further product choice is needed to start these batches under the selected
trusted-space boundary. Private project isolation, additional provider resources,
or new persistent host/access setup would be separate concrete decisions if needed.

## Primary Interface References

- [Responses function calling](https://developers.openai.com/api/docs/guides/function-calling)
  documents function declarations and returning call-associated outputs.
- [Responses web search](https://developers.openai.com/api/docs/guides/tools-web-search)
  documents the separate hosted search tool and its configuration.
- [Codex MCP](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) documents stdio
  and Streamable HTTP; the pinned binary fixture determines actual compatibility.
- [MCP versioning](https://modelcontextprotocol.io/specification/2026-07-28/basic/versioning)
  distinguishes current per-request versioning from older negotiated sessions.

The current source-bound integration evidence begins at
[the runtime batch](../../../../../borg-runtime/artifacts/v2/build-60156282/collaboration-mcp-u4bh_7ns/status.json).
