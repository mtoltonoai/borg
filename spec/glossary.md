# Borg — Glossary

> **What this document is.** The controlled vocabulary for the whole specification. Every term used
> normatively in the constitution, the contracts, and the capability specifications is defined here once,
> so the specs use words consistently. This document is descriptive, not normative: it carries no RFC-2119
> requirements. Concrete technologies that realize a term are recorded in [defaults.md](./defaults.md),
> never here.

---

## The platform and its parties

- **Platform** — the multi-tenant agent harness this specification defines: it runs sessions, delivers
  events, calls models, dispatches tools, governs at fixed hook points, and persists state, for tenants
  who supply everything else as configuration and as services.
- **Core** — the part of the platform that executes and enforces. It decides nothing it can be told:
  policies, prompts, tools, models, deciders, thresholds, and triggers are data it reads.
- **Tenant** — the trust and data boundary: everyone who could ever be allowed to see a piece of data,
  together with their agents, sessions, keys, and grants. A team or organization that onboards is a
  tenant; a project is not, and organizes work inside one.
- **Operator** — a person who directs agents' work and on whose behalf sessions may act.
- **Person** — a human principal, authenticated by the people identity provider.
- **Service** — a non-human principal reached over the network and authenticated by request signing or
  by the platform's assertion: tool servers, definition sources, hook destinations, sibling products.
- **Agent** — an identity whose behavior a definition describes and whose sessions the platform runs.
- **Node** — one process of the platform on one host. A **cluster** is the set of nodes and durable-store
  replicas serving one stage. A **cell** is an independent cluster behind a tenant router.
- **Stage** — a deployment environment with its own cluster and resources, from development through
  production; each platform stage pairs with a stage of the collaboration platform.
- **Shared framework** — the library of generic components that the platform, the collaboration
  platform, and the workspace service all build on; a generic mechanism any of them needs is built there
  and pushed down.
- **Previous runtime** — what the first tenant's agents ran on before the platform: an interactive
  coding assistant driven by keystroke injection, woken by timers, and watched by polling daemons.

## Sessions, partitions, and the durable store

- **Session** — one agent identity's continuous transcript and state for its lifetime, however long.
  Which sessions exist is decided outside the core, by definitions and by sessions starting sub-sessions.
- **Transcript** — the ordered record of a session's turns; the model's view of it is derived from the
  durable log and never edited in place.
- **Session state** — the durable record of a session: pinned definition version, phase, cursors, lease
  generation, timers, subscriptions, key-value slot, pending inbox, in-flight dispatch records, and the
  names of its frozen ranges.
- **Durable store** — the replicated, transactional store holding every session's state and inbox, with
  atomic multi-key commits, read-validation of named keys, and a watch that wakes a reader when a key's
  version advances. Anything in a node's memory is a cache of it.
- **Partition** — the unit of ownership: a fixed slice of the session space owned by exactly one node at
  a time, which is the single writer of its sessions' state. A session's partition is a function of its
  id alone.
- **Partition space** — the set of partitions, sized by a configured number of bits of the session hash,
  grown by splitting each partition into exactly two children.
- **Coordinator** — the one component per partitioned set that assigns partitions to nodes from a
  consistent view of membership, by a pluggable **placement policy** under which a node's arrival or
  departure moves only the partitions it wins or loses. It is off the critical path.
- **Generation** — the increasing instance number a partition owner commits under; every commit checks
  it, so a superseded owner's next commit fails.
- **Fence** — any check of a generation, epoch, or lease generation that stops a superseded actor from
  taking effect, so correctness never waits on it noticing.
- **Lease** — a time-bounded claim that lapses unless renewed.
- **Doorbell** — a per-partition key to which a publisher appends a session id in the same transaction
  as the inbox append, so the partition's watch wakes for exactly the sessions with work.
- **Inbox** — a session's own ordered queue of events, applied in order under the session's **cursor**,
  the position below which everything has been applied.
- **Watermark** — the doorbell position below which every named session has caught up.
- **Residency** — whether a session's resident form is loaded on its owner: a session loads when it has
  work, stays while busy or warm, and is **evicted** lowest priority first.
- **Frozen** — the state of a transcript range, or a whole idle session, whose bytes have moved to the
  blob store leaving a pointer behind. The **transcript head** is the bounded unfrozen tail.
- **Index element** — a frozen session's pointer and timers, stored inside its partition's index key
  rather than as keys of their own.
- **Keyed instance** — a session of a keyed definition, identified by tenant, source, definition, and
  key, and created by the first event addressed to it.

## Events and delivery

- **Event** — an opaque payload with a topic, a priority, a dedupe id, an access scope, and the stamped
  identity of its publisher.
- **Topic** — an opaque name, scoped within a tenant, that events are published to.
- **Publish** — appending an event to a topic for fan-out to every subscriber. **Send** — delivering an
  event to one session, or to a keyed definition with its key.
- **Subscription** — the record that a subscriber receives a topic's events: standing (from a
  definition), dynamic (managed on a session's behalf), or viewer (following a page).
- **Subscriber** — a session or a viewer.
- **Fan-out** — delivery of one event to every subscriber's inbox and doorbell, in batches bounded by the
  store's transaction size.
- **Notification** — an event as it arrives at a session, weighed against the session's **interrupt
  threshold**: the dynamic bar a priority must clear to interrupt work in progress rather than wait for
  the next turn boundary.
- **Priority** — a number the core carries on sessions, events, timers, and work; it orders everything
  that queues. Named classes are an adapter convention.
- **Causal parent** — the event that caused another, named on every delivered event.
- **Access scope** — the readers an event's source allowed, carried on the event and propagated as a
  union onto everything derived from it.
- **Dropped-count marker** — what an inbox keeps, per topic, in place of events discarded past its cap,
  so nothing is dropped silently.
- **Viewer** — one live connection from one person's page, opened with a short-lived **viewer token**;
  it subscribes to topics as the page changes and receives **change notices**, small bounded payloads, a
  burst of which collapses to one reload notice per topic.
- **Tail** — a live stream of one session's activity that resumes by event id and can start anywhere in
  the transcript; a **tail token** admits one viewer to one session's tail.
- **Edge** — a queue outside the platform that buffers publishes while the cluster is unreachable.

## Turns, models, and context

- **Turn** — one model request and its reply, with the tool calls the reply makes. A **step** is the unit
  of durable progress within it. A **model attempt** is one request, identified by an attempt id that is
  its idempotency key.
- **Provider** — the party whose model answers, through a **model endpoint**; each provider has a wire
  dialect the platform renders to and decodes from.
- **Capacity source** — an account, a region, and a model endpoint reached through a role. The
  platform's own sources are the default pool; a tenant may bring its own.
- **Scheduler** — the per-node component every model call passes through: it admits by priority then
  deadline, adapts concurrency to throttles and latency, and routes across capacity sources.
- **Latency class** — interactive, background, or batch (slower and cheaper, result as an inbox event).
- **Quota share** — a node's leased portion of a model's quota, rebalanced by demand, with reservations
  per priority class.
- **Prompt cache** — the provider's cache of a request prefix. The **cached prefix** is the leading part
  of a request the cache can serve; the **cache lifetime** is how long it survives idleness; a **refresh
  read** keeps it warm across an expected short gap.
- **Breakpoint** — a marker where a cached prefix may be split; the platform places them at segment ends
  so client and provider cache boundaries coincide.
- **Compaction** — replacing the transcript before a cut point with a summary while keeping the tail
  verbatim; **background** when run as a side request swapped in at a turn boundary. The **keep set** is
  what stays verbatim after the cut: plans, open items, decisions, identifiers.
- **Soft and hard thresholds** — the context sizes at which a background compaction starts and at which
  compaction becomes a precondition of the next turn.
- **Rollback** — removing failed attempts from the model's view only; the durable log keeps them.
- **Trimming** — evicting stale tool results from the model's view behind a placeholder the model can
  resolve by call id.
- **Provider state** — opaque provider-bound content (reasoning, compaction items) the platform
  round-trips unmodified.
- **Charter** — the prompt components of a definition that tell an agent who it is and what it does.
- **Cohort** — the fraction of a definition's sessions that adopt a new version first.

## Tools and dispatch

- **Tool** — a function a model may call; its **spec** (name, description, schema) comes from the server
  that provides it, and its **semantics** (idempotent, side effects, timeout, retry, cancellable,
  reconcile binding) default to the server's annotations and can only be made more conservative.
- **Tool server** — a service that lists tools and answers calls over the tool protocol.
- **Card** — the served form of a definition version as a session loads it; in particular the registry of
  tool servers and tools its sessions may call.
- **Call type** — a tag a tool spec declares (for example a board comment, or a definition write) that
  hook policies are written against, so the core keeps no domain vocabulary.
- **Dispatch** — resolving a tool call against the card, passing the tool-call hook, recording it, leasing
  credentials, running it, passing the tool-result hook, and committing the result.
- **Call id** — the dedupe key of a tool call end to end, given to the executor and to any target that
  honors one.
- **Dispatch record** — the durable record of a call, committed before the call goes out.
- **Outcome unknown** — the result returned to the model for a side-effecting call interrupted before its
  outcome was recorded.
- **Reconcile** — asking a tool server whether a call id's effect landed.
- **Native tool** — a tool the platform implements itself because it acts on the platform's own state.
- **Sub-session** — a session started by another session, narrowed by the call, whose lifecycle returns
  to its parent's inbox. A **fork** is a sub-session that starts from its parent's transcript up to the
  fork point.
- **Egress point** — the single path the platform's outbound calls leave through, admitting only the
  destinations a binding names.
- **Workspace** — a disposable environment for shell, files, and builds, provided by the workspace
  service as a tool server on the card.

## Governance

- **Hook point** — one of the fixed places the core asks policy before acting: model request, model
  output, tool call, tool result, notification arrival, stop request, control operation.
- **Decision request** — what the core builds at a hook: principal and chain, hook and call types, the
  content, what the session is doing, and its usage, with a size bound.
- **Decider** — a component that maps a decision request to a decision. Kinds: pattern, classifier,
  program (sandboxed), policy (declarative and analyzable), judge (a model call), external (a service or
  a person).
- **Pipeline** — the ordered deciders a definition names per hook; cheap deterministic ones first, a block
  short-circuits the rest.
- **Decision** — allow, flag (proceed with an event), block (with a reason), defer (park the call until a
  decision arrives), or transform (proceed with a patch).
- **Defer** — a parked call awaiting a decision, with a decision id, the principals allowed to resolve
  it, a deadline, and an outcome on timeout.
- **Stop admission** — the stop-request hook's decision of whether a session may stop or has work to go
  on with.
- **Shadow mode** — a decider evaluating and recording without enforcing, so its readiness to enforce is
  measured.
- **Episode** — a recorded fail-retry-pass sequence, a shadow result, or an operator override, which
  feeds back as training labels.
- **Widening change** — a configuration change that increases what a session may do or weakens a check;
  **narrowing** decreases it; **neutral** does neither.
- **Governance tier** — the class of operations that change policy, grants, source bindings, and the
  decider catalog; a tier above it changes governance policy itself.
- **Genesis bundle** — a tenant's initial trust root: default policies, the identity adapter's authority,
  first grants, catalog pins, and source bindings with their ceilings.
- **Emergency override** — a fully audited path, checked by the core itself, that replaces a policy that wedges
  governance.
- **Gate spec** — an approved statement that a class of change may proceed when named evidence holds, run
  outside the agent's reach.
- **Evidence** — a checker's own record (proved, refuted, or unknown, with bounds and who ran it), never
  an agent's account of it.
- **Sound checker** — a checker with no model inside it, the only kind that may widen authority.
- **Earned autonomy** — a temporal policy that widens what a role may do as its record grows and narrows
  it after a rollback.

## Identity and delegation

- **Principal** — the identity an action is attributed to: a person, a service, a definition (tenant,
  source, name), or a session. Ids name their identity provider and their tenant.
- **Chain** — the on-behalf-of sequence a session acts under (operator, concierge, sub-agent), carried
  with every call and event.
- **Requester** — the principal whose event a shared session is acting on; the **effective authority** is
  the intersection of the session's grants and the requester's.
- **Grant** — a record of delegated authority: who delegates, to which session or definition, which
  actions and resources, its expiry, conditions, and tenant. A sub-session's grants are references to
  its parent's.
- **Ceiling** — the upper bound on what a role, binding, or definition may ever be granted.
- **Assertion** — a short-lived signed statement of a session's principal, chain, tenant, scopes, lease
  generation, and call id, bound to one call and audience, presented at trust boundaries.
- **Credential broker** — the service that exchanges an assertion and a credential reference for a
  short-lived credential scoped to the principal, chain, and target, handed only to the executor.
- **Credential reference** — the name by which a tool spec asks for a credential without holding it.
- **Identity adapter** — the higher-level component that maps external identities to principals and
  writes grants.
- **Attribution** — the operator identity carried into every call a session makes for them, so the
  external audit trail names the operator.
- **Clearance** — a grant naming the domains a session may see; a repository carries a **domain**.
- **Human-only action** — an action company policy reserves for a person; a session reaching one defers.
- **Two-person gap** — the risk that the operator who directed agent-authored work also approves it.

## Tenancy, keys, and storage

- **Root key** — a tenant's key in the key service (the tenant's own, or one the service holds for it)
  from which **branch keys** are derived per epoch and random **data keys** per resource.
- **Envelope** — a resource's ciphertext together with its wrapped keys and authenticated data binding it
  to its owner and range.
- **Trusted key list** — the node's configured set of permitted root keys; a stored value never chooses
  a key.
- **Decrypt cache** — the per-node cache of plaintext keys whose lifetime is the revocation window.
- **Blob store** — the owner-scoped store of immutable, encrypted blobs named by the hash of their stored
  bytes, backed by an object store with a node-local cache.
- **Owner** — the one session (or tenant-owned holder) that wrote a blob and is the only writer of its
  references and deletions. A **pin** is an explicit shared reference with a holder and an expiry.
- **Catalog** — the small, versioned set of platform-shared content: decider modules, weights, templates.
- **Retention** — tenant configuration, per kind of data, for how long it is kept; deletion is key
  destruction.
- **Usage record** — one record per model attempt, tool call, or decider call, naming tenant, chain,
  session, cause, outcome, cache reads and writes, prompt size, and definition digest.
- **Observation record** — a pointer to a transcript range published to a queue when a session crosses a
  threshold, with who, why, and how much.
- **Audit event** — a record of a decision or an issued credential, holding ids and decisions but never
  content, kept apart from session data.

## Definitions and sources

- **Definition** — the desired configuration of one agent identity: prompt components, model and
  parameters, tool servers, deciders, budgets, deadlines, priority, standing subscriptions and schedules,
  compaction, cache, observation settings, hook destinations, lifecycle, and instancing.
- **Version** — an immutable definition addressed by its digest; versions form a parent chain.
- **Definition source** — a service bound to one tenant that serves definitions over the definition
  contract: an index at a rising revision, versions by digest, and a change feed.
- **Binding** — the governed association of a tenant with a source, with its credential and ceilings.
- **Controller** — the platform's per-binding component that reads the index, validates versions, passes
  them through governance, and materializes the desired set.
- **Desired set** — the sessions a tenant's definitions call for, which partitions reconcile against.
- **Lifecycle intent** — a definition's run, paused, or retired state, as served by its source.
- **Instancing** — singleton, or keyed by requester or by a publisher's key.
- **Adoption** — a session taking up a newly accepted version at its next turn boundary.
- **Standing schedule** — a definition's recurring timer, created with each instance.
- **Delivery switch** — a per-agent flag held by the definition source that says which runtime acts as
  that agent, honored by the source and by the previous runtime during a migration.

## Timers

- **Timer** — a deferred event to its own session's inbox, durable, named uniquely within the session,
  fired exactly once per **occurrence** by the session's partition.
- **Guard** — a condition (held events on topics, silence on topics, a slot value, or one decision hook)
  that must hold for a fire to wake the model.
- **Flex** — how late an occurrence may be delivered so that it shares a wake.
- **Deadline heap** — a partition's one ordered set of its sessions' earliest durable deadlines.
- **Empty wake** — a wake in which the model made no side-effecting call, send, or publish.

## The frontend

- **Frontend** — the one authenticated API through which anything calls the platform,
  scoped to a tenant and passing the control-operation hook.
- **Control operation** — start, adopt, pause, stop, resume, quarantine, reconfigure, or steer a session;
  internal primitives, exposed externally only as emergency controls.
- **Emergency controls** — pause, stop, quarantine, and resume per session, definition, or tenant,
  working without the definition source.
- **Webhook** — an authenticated inbound endpoint, mapped to a principal, through which an external
  system publishes.
- **Event hook** — an outbound delivery of a session's lifecycle and status events to a destination its
  definition names, from the session's durable outbox.
- **Hook destination** — a per-tenant registered endpoint referenced by name.
- **Attachment** — a content type and blob reference carried beside a payload, rendered into a turn
  without the core reading the payload.

## Failure, durability, and operations

- **Idempotency key** — the identifier every side effect carries so it can be redone safely.
- **Quarantine** — setting aside a session whose state will not decode, or an event that keeps failing,
  so the partition carries on.
- **Re-drive** — the next owner continuing a step from the committed state after a crash or move.
- **Backstop** — a transitional store outside the cluster that holds what a new cluster needs to resume
  every session until the durable store is durable on its own.
- **Epoch** — the increasing number a lineage of clusters commits under, so a replaced cluster cannot
  write back.
- **Drain** — the orderly stop of a node or cluster: readiness withdrawn, pending work applied, sessions
  frozen, final records written.
- **Restore** — a new cluster taking over a lineage from the backstop or from backup.
- **Health signal** — a process's own report that it may be sent requests.
- **Deployment control plane** — the system that creates, patches, replaces, and deletes clusters.

## Simulation and conformance

- **Simulator** — a stand-in for an external dependency under deterministic simulation, injecting the
  faults the real dependency exhibits. The **entropy source** supplies inputs and faults; the
  **simulation framework** explores interleavings.
- **Contract test** — one set of expectations run against a simulator and, through a **probe**, against
  the real dependency, so the simulator cannot drift.
- **Scenario** — a simulated run asserting an invariant; a **property test** generates sequences and
  checks a reference model.
- **Citation** — a marker in code or test naming the requirement it satisfies by its identity.
- **Requirement gate** — the check that every requirement is cited by an implementation and a test that
  exercises it. **Execution witness** — proof that the cited test and code ran.

## The collaboration platform

- **Collaboration platform** — where people and agents work together, with boards, documents, chat, and
  a wiki; the first definition source, the first tool server, and the first adapter.
- **Task**, **document**, **question**, **channel** — the collaboration platform's records; every change
  to one publishes to that record's topic.
- **Composer** — the only writer of definition versions in the collaboration platform, composing a card
  from approved document versions.
- **Decision surface** — the one queue in which everything that needs a person waits.
- **Revision** — the collaboration platform's gapless per-tenant counter that orders its event log.

## The workspace service

- **Handle** — the name a session uses for a workspace it holds under a lease.
- **Snapshot** — a content-addressed capture of a workspace's contents after a call that changed them;
  snapshots form a **lineage**, and a **swap** restores one onto any host.
- **Per-call credentials** — credentials served to a call's processes only while the call runs.
- **Workspace type** — a named image and network **placement** a workspace is provisioned with.

## Specifications and the change gate

- **Specification** — a document whose requirements are entities that code and tests cite one by one.
- **Requirement block** — a typed block with a stable id, level, polarity, statement, conditions,
  parameters, relations, and rationale, from which the requirement's text is generated.
- **Pin** — the exported form of a specification version a repository commits and cites against.
- **Trusted run** — the requirement gate run by the pipeline with the base commit's configuration.
- **Failure** — one requirement-level gap the gate reports: unimplemented, untested, not executed,
  stale, orphaned.
- **Exception** — a recorded, reasoned decision not to implement a requirement in one repository.
- **Lifecycle (specification)** — Draft, Preview, Target, Green, Superseded, Abandoned.
