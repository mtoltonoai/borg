# Durable Store Mapping: Qualification Proposal

Status: proposed implementation direction, not an adopted production layout.
Prepared on 2026-10-12 from the current shared specification, two existing
transactional integrations and independent source reviews. The existing approval
of PostgreSQL 16.15 is for development and qualification only.

The [concrete source-clarification companion](session-admission-clarification.md)
sets out three proposed replacement identities and the additional admission/handoff
obligations. It preserves the header/body authentication sentence and distinguishes
resource owner identity from the partition's node/writer. Both documents remain
proposals; neither changes the current source or runtime behavior.

## Recommendation And Smallest Decision

Independent review rejects the staged transfer below as a sufficient current
solution: it has no lawful dead-owner preparation path. An eager population cap
also fails to settle how an unreadable authenticated state advances its lease
without overwriting protected bytes. Neither candidate is ready for adoption.

Recommend assignment revocation followed by individual session admission, with
separately recoverable current control authority and preserved unreadable content.
The concrete proposed boundary is below. This requires an explicit source-semantics
decision; it is not an interpretation that already satisfies the same-commit
owner-move and authenticated-header requirements. No source clarification, physical
layout, generation encoding or new dependency is selected by this proposal.

## Proposed Revocation And Session Admission Boundary

1. The coordinator atomically changes the partition assignment and generation under
   its existing coordinator fence. Old partition writers immediately lose their
   ability to commit. The replacement becomes the sole partition owner. The
   coordinator does not gain permission to rewrite session content.
2. Before executing or recovering a session, the replacement commits a bounded
   admission transaction binding that session to the assignment and advancing its
   session lease beyond its recoverable high-water. A changed assignment rejects
   admission. A transaction's work does not grow with the partition population.
3. Current control authority and the content's original seal binding are distinct.
   A preserved envelope is never used as proof of current execution authority.
   Undecodable content stays byte-for-byte held as the session's content, not a
   disposable diagnostic copy. Resume retries that exact content; it cannot create
   an empty replacement session to bypass the failure.
4. The trusted session identity, lease high-water and held-content association must
   survive restart without decoding failed content. If those facts or a safe
   authenticated control binding are unavailable, the session remains
   non-executable. Healthy peers may recover independently. No generation or key
   choice is inferred from corrupt or unauthenticated headers.
5. A new tool dispatch is admitted only by committing its existing dispatch record
   under the current partition fence and session control authority. The governed
   executor consumes only that committed, identity-bound admission. It does not
   accept a caller's assertion that admission occurred. Admission after revocation
   fails at this transaction boundary. Executor handoff/release also checks current
   authority at a boundary serialized against revocation; an old dispatch record
   alone does not authorize a superseded owner's new release or retry.
6. Work already handed to an independently governed executor before revocation may
   finish under its existing semantics. It retains its call ID, receipt and
   unknown-outcome handling. A superseded owner cannot use a historical dispatch
   record to initiate or retry a send. Assignment revocation does not cancel, undo
   or establish the result of handed-off work. Recovery dispatch, reconcile and
   close by the replacement still require its committed advanced session lease.
   The receiver's existing highest-observed-generation rule and unfenced reconcile
   remain unchanged.
7. Every control record, held-content reference, assignment/fence, receipt and
   outbox access counts toward the declared transaction budget. Physical rows,
   index work, encoded bytes and retained history are also measured. No new
   unencrypted-content exemption or logical-key accounting exemption is proposed.

The proposed admission seam in steps 5 and 6 is a concrete enforcement obligation,
not proof that a current adapter implements it. A receiver that has not observed a
new lease cannot infer a partition reassignment from its own highest-observed
lease. The proposal therefore does not claim immediate cancellation or universal
receiver-side revocation. Its discriminators are the ordering of durable dispatch
admission, executor handoff and the assignment commit, with stable identity
preserved through unknown results and recovery. The existing invariant that no
commit or dispatch from a superseded owner takes effect remains in force; this
proposal does not reinterpret it as allowing a stale owner's delayed first send.

## Proposed Source Clarification Scope

| Existing source identity | Proposed clarification; not saved to the shared source |
| --- | --- |
| sessions-and-partitions.md, Ownership Is Fenced By Generation: "A session's lease generation MUST advance, in the same commit as the change, on every change of the party that may act for the session: an owner move, a restore, a pause, a quarantine, a stop or end, and a resume." | Separate atomic partition revocation from bounded per-session admission for owner moves. Retain same-commit advancement for the other listed transitions and advance before every recovery operation. |
| stored-form.md, Authenticated Data Binds The Envelope: "A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation, and its owner identity." | Distinguish the content envelope's original lease/owner seal binding from independently recoverable current session control authority. Both remain authenticated to the correct session; a historical header cannot authorize execution. |
| stored-form.md, Authenticated Data Binds The Envelope: "The state header MUST be bound to the body as authenticated data." | Keep this sentence literally unchanged; the proposed current control binding does not replace original header/body authentication. |
| failure-handling.md, State That Cannot Be Decoded: "A quarantined session's state MUST NOT be overwritten." | Permit fenced control updates while forbidding alteration or substitution of the held undecodable content. Preserve exact-content retry on resume. |

This is a proposed change to the shared semantic boundary and potentially the
stored-format contract, not just a SQL implementation detail. Concrete control
encoding, sealing/key authority, bound accounting and any non-additive format
version/migration remain explicit acceptance work. The existing original source
and implementation target are unchanged. The rejected staged alternative below
remains review history, not instructions to implement it.

The smallest operator decision is whether to adopt this revocation/admission
direction instead of requiring every affected authenticated session lease to
change atomically with the partition assignment. It avoids a transfer transaction
proportional to all sessions and permits isolated recovery of healthy sessions.
The tradeoff is a separate admission step and two distinct, carefully bound kinds
of state. A future implementation must prove that distinction rather than merely
rename a stale header.

Required qualification adds these counterexamples to the existing first-probe
list: dead owner with more sessions than the transaction budget; one unreadable
session among healthy peers; crash before and after admission; corrupted control
binding without invented authority; admission racing revocation in both orders;
committed dispatch with a lost acknowledgement; revocation between dispatch-record
commit and executor handoff followed by both old-owner release and replacement
recovery; and a remote receiver that has not yet observed the newer lease. No test may count a refusal as successful
recovery, omit unhealthy sessions from accounting, or infer cancellation from a
lease change.

## Why A Choice Is Needed

The original [lease rule](../../../core/sessions-and-partitions.md#ownership-is-fenced-by-generation)
requires a session lease advance in the same commit as an owner change. The
[stored header](../../../contracts/stored-form.md#authenticated-data-binds-the-envelope)
carries that lease and is authenticated with the body. The
[simulation invariant](../../../core/simulation-and-conformance.md#the-invariants-every-scenario-checks)
checks every transaction against its key bound.

If each current session header is independently rewritten and counted as a distinct
store key during one partition move, N sessions require at least N plus the
fence/publication overhead in keys.
An unbounded N cannot fit any fixed transaction bound. This disproves that naive
layout, not the shared requirements. Neither the physical row layout nor the
generation representation is prescribed there.

[Declared defaults](../../../record.md#durable-substrate) call 100 transaction
keys advisory and store-derived, and describe two active-session objects: the
inbox queue and state/config/cursor together. A frozen session occupies an element
of the shared partition index, not its own key. Increasing the bound alone does
not solve an unbounded population; hash splitting alone does not prove every
resulting partition fits.

## Proposed Addressed Objects

| Object | Proposed representation and accounting |
| --- | --- |
| Session state | One addressed state object with tagged, sealed control versions and append-only transcript-head parts. Subordinate rows are not independently exposed store keys. |
| Session inbox | One addressed queue object with ordered entries and append/trim operations. Queue entries are bounded individually; the queue is not silently treated as one unbounded scalar value. |
| Partition | One authoritative owner/generation record used by both coordinator and session writers, plus separately counted doorbell/index objects. |
| Receipts and effects | Explicit service-owned fences, receipts and outbox objects, counted in every transaction that accesses them. They are not hidden inside a session-key exemption. |
| Frozen sessions | Index elements and blob references under the existing frozen-session rules; no new per-frozen-session key. |

The adapter's public addressing and enumeration describe a proposed logical
key accounting interpretation, not authority to redefine the shared requirement.
Merely calling arbitrary SQL rows one logical key is insufficient.
Count all distinct objects read, condition-checked or written, including authority,
service, partition and outbox objects. Independently report touched physical rows,
indexes, bytes, retained versions and write-ahead-log growth.

The proposed adapter retains the 64 KiB transcript head, 120 KiB whole state and
128 KiB scalar-value defaults. Head fragments collectively count toward the head
and state bounds. Tagged fields, unknown fields and encryption overhead need actual
size accounting; the apparent 8 KiB difference is not a proven envelope allowance.
Retained preparation versions also need explicit storage and cleanup accounting.
No limit is relaxed or exception adopted by this document.

## Proposed Ownership Publication

1. The old owner remains the sole owner while a fenced preparation records the
   target owner/generation and builds future authenticated header/body versions
   in bounded transactions.
2. While preparing, mutations keep active and prepared versions consistent.
   Creations, deletions, freezing and unfreezing participate; they cannot escape
   the preparation frontier.
3. Publication is refused until durable preparation evidence accounts for every
   affected session and every version selected by the publication is valid for
   its new binding. Unselected or undecodable original bytes remain preserved
   according to the existing corruption and compatibility rules.
4. One conditional transaction publishes the new partition owner/generation and
   active-version selector. Readers obtain ownership and selected state from one
   snapshot. Old writers fail their fence; the new owner cannot issue recovery
   effects before the publication is committed.
5. Failed or superseded preparers cannot publish. Unknown publication outcome is
   resolved under the same authoritative fence before redo or cleanup.

These are proposed acceptance semantics, not existing releases. The precedent
that [compaction prepares then switches atomically](../../../core/sessions-and-partitions.md#cost-follows-active-sessions)
does not prove this ownership protocol. Corrupt or unavailable session content,
frozen headers, cancellation, racing controls and bounded preparation progress are
explicit failure cases; none may be dismissed to claim the move works.

There is a material unresolved counterexample: if the owner dies before or during
preparation, the source still requires its partitions to be reassigned, while only
the current partition owner may write session state. A new owner or coordinator
cannot simply complete preparation as a non-owner, and refusing publication
forever is not recovery. The candidate needs a lawful authority/publication path
for this case before it can be recommended. See [single-writer ownership](../../../core/sessions-and-partitions.md#the-partition-owner-is-the-single-writer)
and the reassignment requirements in that file. A corrupt session also cannot
indefinitely block healthy sessions in the partition; preserving bytes and refusing
one operation is not by itself evidence of the required isolation/progress.

## Alternative And Tradeoff

An eager single-transaction transfer preserves the same-commit timing, but needs a
proven population/write budget including receipts, authority and events. A cap
alone does not resolve corrupt or unsealable state: independent header decoding
does not make an authenticated header independently mutable, and quarantined
state cannot be overwritten. The index's declared space for undecodable state
does not define a separate authoritative lease representation. Normal freezing
also cannot be assumed to apply to a corrupt session with unknown open work.

The staged hypothesis avoids a population cap only if its preparation and
publication semantics are lawful. Those semantics remain unproven after owner
loss. It is retained below for review history and counterexamples, not as the
recommended mechanism to implement.

For the next proposed model, reassignment would immediately revoke old partition
write authority but would not itself permit the replacement to execute a session.
Each healthy session would advance its lease atomically with its own admission;
unreadable bytes would remain preserved and non-executable without preventing
healthy sessions from recovering. This deliberately changes the timing currently
specified for owner moves. It also needs an independently recoverable trusted
lease high-water and an explicit rule preventing an old preserved header from
being treated as current authority. Exact storage location and key accounting
remain unselected.

Effect handling cannot be hand-waved: the existing tool receiver checks its
highest observed session lease, not an automatically observed partition move.
Already dispatched calls, stable receipts and unfenced reconciliation keep their
original semantics. A new proposal must distinguish those cases from a new
effect admission and prove the actual boundary that rejects stale authority;
merely declaring that it rejects old owners is not an implementation or proof.

A composite lease is not a drop-in third implementation: whether an authenticated
header may carry only a seal-time lease instead of the current authoritative lease
needs a source clarification first. This proposal does not make that amendment.

## Transaction Discipline And First Probe

A proposed total lock order is stage epoch, coordinator when needed, membership
when needed, sorted partition fences, authority/control records, then sorted data
objects. Account for uniqueness and foreign-key locks as well as explicit locks.
Normal session writes do not acquire the coordinator. Apply append/trim deltas
to current queues rather than overwriting a stale snapshot. Preserve the
[fence-only conflict rule](../../../core/sessions-and-partitions.md#a-commit-conflicts-only-on-the-fence).
An unresolved deadlock or unrelated-append rejection is a failure, not a permitted
new conflict class.

The first experiment uses more sessions than one transaction's key budget and
concurrent state updates/appends during preparation. Observable cases are:

- Accept the configured key limit, reject one above it before mutation, and report
  physical work separately; exercise 99/100/101 at the current advisory value.
- Reject oversized state without truncation or partial commit; measure full
  encoded/sealed sizes only after those representations are selected.
- Kill a preparer, lose a publication response, and restart. Observe one coherent
  old or new owner/header/body view, never a mix or premature recovery effect.
- Race new/deleted/frozen/corrupt sessions, emergency controls and membership
  changes with preparation. Refuse an incomplete publication.
- Kill the current owner before and during preparation. Demonstrate lawful
  reassignment without non-owner session writes or indefinite loss of healthy
  session service; otherwise reject the candidate protocol.
- Race freeze/deletion with publication. Observe the required single-commit
  removal of active, prepared and subordinate state, incarnation/tombstone rules,
  and destruction/reclamation boundaries without recovering destroyed key data.
- Preserve append-only head behavior, receipt identities and decision/outbox
  associations. Demonstrate that mirrored control/header updates do not rewrite
  prior transcript bytes. Then prove safe reclamation of unselected preparation
  versions while retaining undecodable original bytes where required.

Transactional tests with opaque fixture bytes establish only the exercised
publication transitions. Real sealing, authenticated header recovery, exact encoded bounds and
simulator/real-store parity remain necessary before a maintained production adapter
can be accepted. No production release follows from experimental success alone.

## Scope

No shared requirement, canonical capability, milestone, approval state, runtime
target or copied specification snapshot is changed. Existing bounded component
and private-engine integration work continues independently. This proposal does
not add a new store engine or a speculative universal storage abstraction.
