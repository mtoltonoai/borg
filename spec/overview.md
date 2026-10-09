# Borg — Architecture Overview

> **What this document is.** The target architecture: a description of *what the platform is when it is
> built*, independent of any implementation. It is the intent arbiter: every normative requirement in the
> constitution, the contracts, and the capability specifications traces back to a section here (see
> [traceability](./traceability.md)). When a specification and this document disagree about intent, this
> document is corrected or the specification is, deliberately, never silently.
>
> This document is descriptive, not normative: it carries no RFC-2119 requirements. Section headings are
> stable identifiers cited by name from the traceability map, so they change only deliberately. The
> vocabulary is defined in [glossary.md](./glossary.md); the concrete technologies the current realization
> chose are recorded in [defaults.md](./defaults.md).

---

## 1. The one idea

The platform is a multi-tenant harness for agents: teams onboard, start agents, give them tools, govern
them, and let them work reliably for as long as the work takes. Its core **executes and decides nothing it
can be told**. It runs sessions, delivers events, calls models, runs tools, and persists state; every
behavior beyond safe defaults is configuration that a tenant's own services serve, and every judgment is a
decider the tenant names at one of a fixed set of hook points.

Three consequences shape everything else:

- **Event-driven, never polling.** A session wakes when something addressed to it arrives, and nothing in
  the system asks "anything new?" on a timer. Status, liveness, timers, and the payload of every wake are
  the platform's job, so an agent spends its turns on work.
- **Durable state, disposable memory.** Everything a session is lives in the durable store; everything in a
  node's memory is a cache that can be dropped at any moment. Any node can lose anything at any time, and
  the next owner continues from the committed step.
- **Mechanisms, not approvals, decide what is correct.** Agents produce evidence; checkers decide; people
  decide only what remains ambiguous, and each such decision is a candidate for a mechanism.

## 2. Why the platform exists

The first tenant is a set of agents that ran on an interactive coding assistant driven by keystroke
injection and polled by timers. Measured over five days, most of its spend was waste: wakes that did
nothing, cache writes that rewrote prompts expired during idle gaps, prompts carrying hundreds of thousands
of tokens because compaction fired only near the window, and turns spent on status, heartbeats, and
fetching the event a pointer named. The machinery around it (notifiers, nudges, watchdogs) existed because
agents could not be woken by events, and it was fragile: a lapsed human credential once took most of a
daemon's nodes down for hours.

Buying a managed loop does not reach the levers that matter: no available loop offers a stop veto, inline
deciders on output, exactly-once event delivery, or control over compaction and cache lifetime, and tokens
are nearly all of the cost. So the loop is built, on an inference layer already in hand, and what every
option needs anyway (an adapter for the first tenant's board, stop admission, deciders, a credential
broker) comes first. The goal past the first tenant is the multi-tenant harness at scale: hundreds of
thousands to millions of sessions for hundreds of operators across dozens of projects, with thousands of
people's live viewers beside them.

The platform is not an everything platform. It hosts no programs for its tenants: tools are services
teams build and ship through their own pipelines, agent definitions live in services too, and deciders
are the only tenant-supplied code the platform runs, sandboxed and bounded. Agents can still build almost
anything, because they write services and ship them through the normal pipelines; the platform changes
who does the work, not where code runs or how it ships. Behaviors harden progressively: a workflow starts
as instructions an agent follows and, step by settled step, becomes a tool behind the same interface.

## 3. Sessions, partitions, and the durable store

A session is one continuous transcript for its lifetime, and the platform does not care how long that
is: a concierge may live indefinitely as a conversation, a task session may start fresh and end with its
task. Sessions run in **partitions**: a fixed set, each the single writer of its sessions, fenced by a
generation so that a superseded owner's next commit fails rather than corrupts. A session's partition is a
function of its id alone, so placement never moves data; the space grows by splitting each partition into
two children, and a session only ever moves to one of its partition's children. A generic coordinator
places partitions on nodes from one consistent view of membership, so that a node's arrival or departure
moves only the partitions it wins or loses; the coordinator is off the critical path.

A partition sleeps on the earlier of a watch on its doorbell and its next deadline, and reads the wall
clock again on every wake. It loads a session when there is work, keeps it resident while busy or warm,
and evicts idle sessions lowest priority first. Everything about a session is durable in the store; the
resident form is a cache. The store's per-key cost drives the layout: an active session holds few keys, a
frozen session is an element of its partition's index, the transcript head is bounded, and queues are
trimmed, so the store's working set follows the active load rather than the total number of sessions.

## 4. Events, inboxes, and subscriptions

Order is per session, and there is no order between sessions. Each session has its own inbox; a publisher
appends the event to the inbox and the session id to the partition's doorbell in one transaction; the
partition applies the inbox in order and commits the new state and the cursor together under its fence,
so every event applies exactly once relative to the session's state and a slow session delays only
itself. The partition reads at the version its watch woke for and commits as blind writes that validate
only the fence, because a commit that validated the doorbell starves under publishers.

Subscriptions are the platform's own: publish, subscribe, and unsubscribe over topics whose names and
payloads the core never interprets; fan-out actors deliver in bounded batches so publish latency does not
grow with subscriber count; every delivered event carries its publisher's authenticated principal and
chain, stamped by the platform and never taken from the payload, and names its causal parent. Every
notification has a priority, and the session decides whether it interrupts: a priority above the
session's dynamic threshold cancels the model turn in progress (a running side-effecting tool is never
cancelled), one below waits for the turn boundary. An inbox that never drains is bounded, and past its
cap keeps a dropped-count marker per topic rather than dropping silently.

People's pages subscribe too. A viewer is one live connection from one page that subscribes to the topics
for what it shows, receives small change notices rather than records, misses nothing between loading a
view and subscribing to it, replays nothing, collapses bursts to a reload notice, never slows a publisher,
and sees only what its person may read.

## 5. Model turns, scheduling, and the prompt cache

Every model call goes through the platform's schedulers, so capacity is shared, ordered, and fair. A
session's effective priority is the highest of its base priority, the priority of the work it is
handling, and any priority inherited from a session waiting on it, and that one number orders model
admission, the order a partition processes its dirty sessions, the interrupt threshold, eviction, and the
deciders' compute pool. Throttling becomes queueing: concurrency rises until throttles or latency climb
and backs off multiplicatively. Quota is a cluster resource leased in shares per node and rebalanced by
demand, with reservations per priority class. Capacity sources are an account, a region, and a model
model endpoint reached through a role; the platform's own form the default pool, a tenant may bring its own
through a trusted role that requires the tenant as the external id, and a session's route is sticky so
its cached prefix stays warm.

The prompt cache is the largest cost and the platform manages it per session from the definition:
the cache lifetime, refresh reads across expected short gaps, compaction before a long idle, low-priority
wakes held for a warm window, and cache reads and writes recorded in every usage record. The inference
layer beneath is provider-neutral: one canonical transcript model, provider-bound payloads round-tripped
opaquely, a request that is a pure function of the log and policy, cache breakpoints at segment ends so
client and provider cache boundaries coincide, append-only messages with fixed tools and system prompt within
a prefix epoch, and failures classified from each provider's structured fields.

## 6. Context: transcripts, compaction, and freezing

A session's transcript is compacted in the background, configured per session well below the model's
window: a soft threshold starts a side request that summarizes everything before a cut point and is
swapped in only at a turn boundary, a hard threshold makes compaction a precondition of the next turn,
and a change to the session's charter schedules a debounced compaction that folds the change into the
system prompt so the charter the model reads stays whole. Compaction preserves history: everything before
the cut freezes into the blob store, encrypted and compressed, and the durable log keeps a pointer. The
recent tail and a keep set (plans, open items, decisions, identifiers) stay verbatim. Failed attempts are
rolled back out of the model's view only, stale tool results are trimmed behind resolvable placeholders,
and the full output of every tool call is stored once and paged through by a native tool.

## 7. Tools are services

A tool's spec comes from the server that provides it and its semantics default to the server's
annotations, which a definition can only make more conservative. The definition is the registry: it
lists the servers and tools its sessions may call, dispatch refuses anything not on it, and no provider is
trusted to enforce what a model loaded. A tool call is a durable operation: resolved against the card,
passed through the tool-call hook, recorded before it goes out, run with credentials leased by reference
for the principal and chain, passed through the tool-result hook, and committed as a state transition.
The call id is the dedupe key end to end; recovery retries a call under a declared idempotency guarantee
that covers its unrecorded outcome, even when it has side effects, without a prerequisite outcome check.
A side-effecting call without that guarantee returns "outcome unknown". A superseded owner's dispatch
does not take effect even before its receiver learns of replacement, including outside targets covered
by the tool-call contract; receiver-local generation checks do not exhaust that guarantee. Outbound
calls leave through one egress point that admits only named destinations.

The platform implements as native tools only what acts on its own state: subscriptions, publish and send,
sub-sessions and forks, focus, timers, a small key-value slot, loading a deferred tool, and paging a
stored output. Everything else, the board, workspaces, coordination across agents, discovery, memory,
company systems, the web, is a service on the card, called with the session's identity, and the platform
never knows what it does. Governance, compaction, caching, scheduling, and status reporting are not tools
at all: they are mechanisms the platform runs around the model, so no agent spends turns on them.

## 8. Governance is one mechanism

Authorization, content deciders, approvals, secret scrubbing, stop admission, and interruption are one
mechanism: a fixed set of hook points, each with an ordered pipeline of deciders from the definition, each
decision one of allow, flag, block, defer, or transform. Cheap deterministic deciders run first and a
block short-circuits the rest; a blocked output retries in the loop with a reason that never repeats the
rejected content; a blocked tool call becomes an error result; a blocked control operation is rejected; a
defer parks the call, not the session; a transform redacts or routes. Deciders guard what enters a session
as well as what leaves it, an inbound event never carries a tool call for the core to dispatch, output is
gated per completed block before anything takes effect, and failure defaults to closed.

Confidence comes from mechanism, not from a click. A human approving each action cannot scale to hundreds
of thousands of agents, and approvals that come too often stop being meaningful. Actions are tiered by
risk and most proceed on mechanical evidence (deciders, tests, deterministic simulation, proofs, canaries
with rollback, a role's track record), audited after the fact. Agents produce evidence but never approve;
only a sound checker may widen authority; evidence is the checker's own record, never an agent's account
of it; weakening a property is a widening change; humans approve policies and plans, as gate specs
approved once, rather than instances. Autonomy is earned and withdrawn by temporal policy. A new decider
runs in shadow first, its decisions are events, stateless decisions are cached by content, and every
configuration change, a session's own deciders included, is itself a control operation that passes a
hook, so no path changes config without governance and a session can loosen neither its own governance
nor another's. A tenant's genesis bundle and a fully audited emergency-override path are the only touchpoints
outside the pipeline.

## 9. Identity, delegation, and secrets

The substrate owns the invariants and higher levels own the policy. Every action has a principal, and the
on-behalf-of chain travels with every model call, tool call, outbound message, control operation, and
event. Enforcement happens at the fixed hooks and fails closed. Secrets stay out of model context, the
transcript, state, events, records, metrics, and logs: a tool spec names a credential by reference, a
broker leases a short-lived credential for the principal, chain, and target, and only the executor holds
it. Delegated authority is data: grants replicated into the partition and evaluated locally, visible to
audit and to analysis. Delegation only narrows: a sub-session's grants are references to its parent's,
revoking an ancestor revokes its descendants, no grant exceeds what its grantor holds or the role's
ceiling, and budgets are carved from the parent's. A shared session acts for whoever asked, with the
intersection of its grants and the requester's. Services learn a session's identity from a short-lived
signed assertion bound to each call, never from an argument.

No human sign-on credential runs automation: nodes reach their dependencies with instance roles that
refresh without a person, and each operator's sessions act with credentials issued for that operator's
delegation, attributable, scoped, and revocable per operator, with the operator carried into the external
audit trail. Where a system offers an on-behalf-of exchange the platform acts as the operator; where none
exists it acts as itself and names the operator in its audit and visible text; where company policy
reserves an action for a person, the session defers to one who is not the operator who directed the
work. Every authorization decision and every credential issued is an event; audit holds ids and decisions,
never content, with its own retention.

## 10. Tenancy and confidentiality

The tenant is the trust and data boundary, and the substrate commits now only to what is hard to add
later: every durable record, principal, grant, subscription, and event names its tenant as a field (never
a metric dimension); ids are unique across tenants and encode no location; principal ids name their
identity provider; authority stops at the tenant; nothing derived from a tenant's content (caches,
labels, trained heads, deduplicated blobs) reaches another tenant; each tenant's keys root in a key of its
own, so destroying it makes the tenant's data unreadable everywhere, backups included; retention is
tenant configuration per kind of data; and tests run two tenants and assert nothing crosses while there
is only one real one. The first tenant is bootstrapped with a stage's first cluster rather than
configured, and there is no path for registering tenants until a second one needs it.

Inside a tenant, confidentiality follows access scopes: every inbound event carries the scope its source
allowed, every transcript range, compaction, and observation record preserves all of its inputs' read
restrictions, and a consumer or viewer needs permission to read every input before reading the combined
content. This reader rule does not prescribe a scope representation. Scopes are recorded from the
first event, because a private channel cannot be separated out again once it is compacted into a summary.

## 11. Encryption at rest and the blob store

Sessions are encrypted at rest under an envelope: a root key per tenant in the key service, a branch key
per epoch, and a random data key per session, with every envelope's authenticated data binding it to its
session and range so a copied ciphertext will not decrypt elsewhere. Any node reads a session with at
most one key-service call; plaintext keys live only in a per-node cache whose lifetime is the revocation
window; anything that cannot resolve a key fails closed; and a stored value never chooses the key, which
comes from the node's trusted list. The key service stays off a turn's critical path: keys resolve when
the doorbell wakes a session, the cache refreshes ahead of expiry, waited-on calls are hedged with short
timeouts, and resumes after a failover are paced.

The blob store is owner-scoped: every blob has one owner, is immutable, and is named by the hash of its
stored ciphertext, so the store and caches verify a blob without any key and a name reveals nothing. A
blob carries its wrapped key chain in its header, so the blob store and the key service alone can recover
a session's history. Sharing is an explicit pin with a holder and an expiry; nothing dedups across
tenants; deletion comes from destroying keys and reaches caches and backups; reclaiming bytes is only
cost and runs late. The first version collects nothing but records owners and pins from the first write.

## 12. Definitions come from sources

A definition is the desired configuration of one agent identity, and a tenant serves its definitions
from its own sources under its own change control. A source serves an index at a revision that only
rises, immutable versions by digest, and a change feed that resumes from any revision. One controller
per binding validates each version against the schema and the tenant's ceilings, passes it through the
input deciders and the control-operation hook, and accepts or rejects it with a reason. Sessions pin a
digest and adopt new versions at turn boundaries, by cohort, with a version able to run as an experiment
against its parent. An unreachable source changes nothing; a definition missing from the index is held,
never retired; served retirement or an authorized emergency stop ends sessions. Emergency pause and
stop remain authoritative after source recovery until explicit authorized reconciliation; reconciliation
does not resume a terminated session. A change to the cached prefix is made without
rewriting it, through each provider's cache-preserving path, and accumulated changes fold into the prefix
at the next compaction. A definition names no principal and carries no credential; a keyed definition's
instance is created by its first event and acts for its requester.

## 13. Timers

Timers are a native primitive: durable, guarded, and fired by the session's partition as events to its
own inbox. Schedules are once or recurring; due times are absolute instants; a timer never fires early by
its owner's clock, a late fire does not drift a recurring timer, and a clock step cannot replay one.
Guards (held events, silence, a slot value, one decision hook) make a wake cost nothing when there is no
work. After consecutive empty wakes a recurring timer's interval stretches, minimum intervals and
per-session counts bound what a session may hold, and empty wakes are capped per day. A partition keeps
one deadline heap for its sessions' timers and the platform's own deadlines and sleeps on the earlier of
its doorbell and the heap's head. Clock faults are events, and an owner whose clock disagrees with the
store's commit timestamps holds its fires.

## 14. The frontend

What calls the platform comes through one authenticated API, scoped to a tenant, through which every
call is made by a principal and passes the control-operation hook. Onboarding installs a genesis bundle
and binds sources and hook destinations; governance writes policies, grants, and catalog entries;
emergency controls pause, stop, quarantine, and resume without the source; the data plane publishes,
sends, resolves defers, manages subscriptions on a session's behalf, asks a session a question through a
throwaway fork, and reads state and granted transcript ranges. Session configs and the desired set are
never written through the API: they come from definitions. A tail streams one session's gated activity to
a viewer holding a read grant, relayed from the owner through a bounded buffer so a slow viewer is dropped
and never slows the session. Webhooks let an external system publish; event hooks out deliver a closed
set of lifecycle and status events from a session's durable outbox to named destinations, replacing the
turns agents spent reporting their own status and the watchdogs that polled for liveness. Replies carry
stable error codes and trace context; refusals name the cap they hit; development-only modes refuse to
start outside the development stage.

## 15. Observation, usage, and the improvement loop

Everything is observable: a specific event for everything, with metrics split only by closed sets, and
ids, principals, and tenants carried as fields rather than dimensions. One usage record per model
attempt, tool call, and decider call names the tenant, chain, session and parent, cause, outcome, cache
reads and writes, prompt size, and definition digest, so versions compare on the same stream and budget
deciders, metering, and experiments read the same data. A session publishes an observation record to a
queue when it crosses a threshold, pointing at a transcript range rather than copying it, and a reader
with a grant reads the range under audit. The system improves itself through its own primitives:
observers read the usage stream, propose a change as a new definition version through the source's change
control, run it on a cohort against its parent, and only then roll it out.

## 16. Failure handling, durability, and deployment

The durable state machine is the source of truth, all work is re-drivable from it, every side effect
carries an idempotency key, and every failure class has one defined handling and a defined state the
session ends in: throttles back off and queue, an exceeded window compacts and retries, an invalid request
blocks with its reason, a mid-stream failure discards the partial reply, and a tool retries within its
budget when its declared idempotency guarantee covers an unrecorded outcome, even if it has side effects.
A side-effecting call without that guarantee reports an unknown outcome. Undecodable state is quarantined and never
overwritten, an event that keeps failing is quarantined so it cannot wedge its partition, a moved
partition resumes from committed cursors, and a source that is unreachable changes nothing. Every turn
and tool call has a deadline, and every blocked or quarantined state has a way out an adapter can drive.

The store must survive a full-cluster restart, with backup and restore defined. Until the shared
framework's durable log lands, a backstop outside the cluster holds what a new cluster needs to resume
every session, written at each freeze and at shutdown and conditioned on an epoch so a replaced cluster
cannot write back; an orderly drain loses nothing. A single-node failure recovers committed progress
from surviving cluster state. Total-cluster disaster restoration may lose ordinary acknowledged session
progress since the last durable freeze, while acknowledged tenant governance records remain protected. The
platform deploys through the deployment control plane the team already operates, as a cluster type of
its own that never shares a cluster with other workloads; durable formats are versioned so two versions
run side by side; alarms and dashboards derive from the platform's own events, every alarm has a runbook,
and restores are drilled. A gate builds and tests every commit before any stage can deploy it.

## 17. Deterministic simulation and conformance

Everything runs under the simulation framework, external dependencies included. Each dependency (model
model endpoints, tool servers, definition sources, hook destinations, the key service, the object store, the
token exchanges, the backstop, the clock) has a simulator that injects latency and its tail, throttles,
errors, timeouts, dropped connections, duplicates, reordering, and partial failures; the entropy source
supplies inputs and faults; the framework explores interleavings; and a contract test runs the same
expectations against the simulator and, through a probe, against the real dependency, so the simulator
cannot drift. Time is a dependency too, with a per-node wall clock whose offsets, drift, and steps are
injected. The invariants every scenario checks are exactly once, ordered, fenced, re-drivable, gated,
secret-free, isolated across two tenants, bounded, and accounted. Requirements themselves are judged by a
requirement gate: every normative sentence is cited by an implementation and a test that exercises it,
with execution witnesses, so a stub that reproduces the shape without the behavior does not pass.

## 18. The collaboration platform

The collaboration platform is where people and agents work together: boards, documents, chat, a wiki, questions anyone can
pose on almost anything, and, growing on one typed-block document model, specifications that reconcile
with code, code reviews, notebooks, and dashboards. It runs on many servers behind a load balancer with
no state on any node, scales to the platform's session count with thousands of people's viewers, orders
changes per record rather than across the tenant, takes every caller's identity only from
authentication, and keeps every id people cite. Toward the platform it is a client and an adapter, never
a client of the durable store: it publishes every record change to that record's topic with ids derived
from its revision so delivery is exactly once, manages agents' subscriptions as tasks are assigned and
finished, serves agent cards as the first definition source with lifecycle intent governing ordinary
pauses and authorized emergency pauses retained until explicit authorized reconciliation, answers stop admission from whether an agent has open unblocked work, receives status hooks that
replace agents reporting themselves, embeds the session tail in an agent's profile, and keeps only
person-addressed notifications of its own. Coordination across agents (fan-out, collect, loops with a
cap, re-delegation) lives here as tasks and assignments until working cases show what generalizes.

## 19. The workspace service

Workspaces are not part of the platform: they are a service on an agent's card, called like any other
tool with the session's identity, and the platform does not know what the service does. The service
hands a session disposable workspaces for shell, files, and builds. A workspace's contents are a value:
the service snapshots them, content-addressed, after every call that changed them, so a host can be lost
at any time and the session continues from its last snapshot on another host; snapshots also give fork
and rollback. Leases belong to the service and expire unless used. A session may hold several named
workspaces with placement constraints that can reach one another, each with its own lineage. No standing
credentials exist inside a workspace: a local endpoint serves each call's credentials only while the call
runs, the call's processes are killed when it ends, and the instance metadata path is unreachable.
Workspaces never cross tenants, tools run unprivileged under limits, egress leaves through one point, a
workspace type carries its network placement, a repository carries a domain the session must be cleared
for, and a build cache keyed by content is shared within a tenant and never across.

## 20. Spec-driven design

Requirements become entities that code and tests cite one by one, so whether code still matches its
specification is computed, not judged. A specification version moves from Draft through Preview (the
failures it would add and resolve) and Target (one tracking ticket, the gate's failures as the work list)
to Green, when the trusted reports on every bound repository's main show no failures against it. The gate
is a floor, not an approval: "no new failures compared with the base", run by the pipeline with the base
commit's configuration, can block a change but never admits one alone; a requirement the change touches
also needs execution witnesses plus mutation testing or a proof; an exception on a mandatory requirement
is a widening change; and a version that weakens a requirement defers to an approver who is not its
author. Documents become versioned trees of typed blocks whose requirement blocks carry stable ids, so a
reworded requirement stales exactly the citations it should and a retitled document stales none.

## 21. Shared framework components

Generic mechanisms belong in the shared framework, extracted from working cases with two users from the
start: a partitioned actor set with a coordinator and placement policy; reads pinned at the woken
version; a deadline queue and absolute-time sleep on the environment; a per-node simulated wall clock;
one envelope-encryption implementation; the owner-scoped blob store; a change feed with fenced consumers
and a resumable HTTP form; a live-stream hub for viewers with bounded buffers and resumption; keyed rate
limiting; caller authentication that turns a request into a caller and call guards that dedupe by call id
and fence by lease generation; and, for durability, a write-ahead log, durable snapshot, and durable index
for the store itself.

## 22. What is deliberately left open

The platform is released early to learn from use, but its first conformance claim retains all current
normative obligations. There is no implicit phase deferral for fair sharing, downstream metering and
billing, or tenant-supplied keys and capacity. Foundational usage records and their required consumers
remain in scope. Product choices and additional mechanisms remain open only where they do not defer an
existing normative obligation: tenant registration behind the tenant on every record; cells behind
location-free ids; more identity providers behind provider-qualified principals; stronger isolation
behind the sandbox being the only place tenant code runs;
cross-tenant collaboration behind explicit shares as grants; streaming ingress and request-reply between
sessions behind topics and call ids on sends; signed definitions behind the attested author recorded with
each version; and a reference definition source serving a repository, and serving external customers, as
product decisions once internal use shows the demand.

## 23. What earlier designs taught

The platform has been designed before, and the attempts shape it. A reducer design tried to cover agents,
programs, and inference with one interface and was too generic to build agents on, so the core now has a
few concrete mechanisms and no universal abstraction. An actor per session placed randomly and never moved,
so sessions now run in partitions placed by a coordinator and fenced by generation. A replication bridge
tailed the board's state into the store and synchronized session state back, so the board is now a client
of the API and a definition source. Bodies were remote-procedure definitions inside the harness, so
workspaces are now a service on the card. A blob design addressed content by its plaintext hash and
deduplicated across sessions under one managed key, which forced reference counting over every chunk and
gave up deletion by key destruction, so blobs are now owner-scoped and named by ciphertext. A classifier
was assumed cheap and measured far slower, so fuzzy deciders now run behind deterministic pre-filters and
off the turn's path unless required to block. The polling predecessor showed where the cost is, so nothing
polls. A lapsed human credential took a daemon down, so no human credential runs automation. The
reasoning behind each lesson is recorded in the project's design history.
