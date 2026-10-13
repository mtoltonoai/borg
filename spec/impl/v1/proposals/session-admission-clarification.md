# Session Admission After Partition Reassignment

Status: the recommended meaning is selected in [the v1 implementation specification](../implementation.md#session-admission-after-reassignment). The shared sources and runtime target are unchanged. The proposed shared tuples below remain review history; protocol/format discrimination and migration precede incompatible activation. See [the exact compatibility disposition](../decisions.md#five-decision-compatibility-disposition). No store product or physical layout is selected.

## Decision And Recommendation

Recommend separating atomic partition revocation from bounded admission of each
session by its replacement owner. The assignment commit immediately fences the old
owner; the replacement advances a session's lease before executing or recovering
it. Preserved content remains distinct from current control authority. This avoids
a reassignment transaction that rewrites every session and allows healthy sessions
to recover when another session's content is unreadable.

This is a deliberate timing and stored-header interpretation change, not a claim
that the unchanged shared specification has no possible implementation. The
alternative is an eager same-commit transfer with a proven population bound and
coherent authenticated state selection. A bound alone does not prove lawful
dead-owner transfer or preservation of unreadable content. The prior staged
preparation candidate did not establish those properties.

The decision here would select only this control/content and admission direction.
It would not select PostgreSQL for production, an encoding, generation scheme,
physical record layout, plaintext exemption, key service, transaction accounting
exception or deployment. Existing immutable object identity and transaction bounds
remain applicable.

## Exact Replacement Identities

Each before quotation is an existing shared requirement. The proposed replacements
below are not active requirements. Section headings remain unchanged.

### Ownership Is Fenced By Generation

Source: [sessions-and-partitions.md](../../../core/sessions-and-partitions.md#ownership-is-fenced-by-generation).

Before:

> A session's lease generation MUST advance, in the same commit as the change, on every change of the party that may act for the session: an owner move, a restore, a pause, a quarantine, a stop or end, and a resume.

Proposed replacement:

> A session's lease generation MUST advance, in the same commit as the change, on every restore, pause, quarantine, stop, end, and resume.

Proposed accompanying clauses under the same heading:

> After a partition owner move, the new owner MUST commit each session's admission before executing or recovering that session.

> A session's admission MUST advance its lease generation under the current partition assignment.

> Partition reassignment MUST NOT require updating every session's state.

> The work of admitting one session MUST be bounded independently of the partition's session count.

The existing requirements for the coordinator's own fence, every partition
commit's fence, the single writer, advancement before recovery dispatch/reconcile/
close, and the committed resume acknowledgement remain unchanged. A partition
assignment alone grants no session execution. Admission does not lift pause,
quarantine, stop or other lifecycle restrictions.

### Authenticated Data Binds The Envelope

Source: [stored-form.md](../../../contracts/stored-form.md#authenticated-data-binds-the-envelope).

Before:

> A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation, and its owner identity.

Proposed replacement:

> A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation at sealing, and its owner identity.

Only the lease interpretation changes. The existing term "owner" includes resource
ownership and is not interchangeable with the partition's node/writer; this
proposal does not change owner identity or the tenant/owner/range encryption
binding. The following existing requirement stays literally unchanged:

> The state header MUST be bound to the body as authenticated data.

Proposed accompanying clauses:

> A session's current control authority MUST be recoverable independently of its content envelope.

> A session's current control authority MUST authenticate its association with the tenant, session incarnation, partition assignment, lease generation, lifecycle restrictions, and held content.

> A session MUST NOT be admitted from an unauthenticated or unrecoverable lease high-water.

Proposed descriptive glossary terms: current control authority is the authenticated
logical association just listed; held content is the original content envelope
retained for a quarantined session. These terms prescribe neither an additional
physical record nor a new storage primitive. A historical header is not current
execution authority. No unencrypted-content or key-selection exception is created.

### State That Cannot Be Decoded

Source: [failure-handling.md](../../../core/failure-handling.md#state-that-cannot-be-decoded).

Before:

> A quarantined session's state MUST NOT be overwritten.

Proposed replacement:

> A quarantined session's held content MUST NOT be overwritten or substituted.

Proposed accompanying clause:

> A control transition of a quarantined session MUST preserve its association with the held content.

Resume still retries the original held content and lifts quarantine only after
successful decode. An empty replacement session with a diagnostic copy beside it
does not satisfy that requirement. Healthy sessions still proceed independently.

## Executor Handoff: Proposed Added Clauses

Source location: [The Dispatch Sequence](../../../core/tools-and-dispatch.md#the-dispatch-sequence).

> An executor MUST accept a dispatch handoff only when its committed dispatch record agrees with the current partition assignment and session control authority.

> Acceptance of an executor handoff MUST be serialized with changes to that authority.

Handoff means the governed executor accepts responsibility, not merely a generation
read or a queued local send. Revocation winning before that acceptance prevents the
old owner's release/retry. Work accepted before revocation retains the existing
stable call ID, receipt and unknown-outcome handling; reassignment does not cancel
or undo it. The receiver's highest-observed-generation rule and unfenced reconcile
are unchanged. A replacement first advances the session lease before its recovery
operations. Durable-before-send-or-execution remains unchanged.

These clauses expose a real implementation obligation. A separate generation read
followed by an unprotected send does not satisfy serialization. No lock, gateway,
receiver topology or cross-service transaction mechanism is selected here.

## Required Discriminating Evidence

| Case | Acceptance for this proposed direction |
| --- | --- |
| Dead owner; population exceeds one transaction's budget | Revoke assignment without dead-owner preparation; admit sessions in bounded operations. Count all authority/control/outbox work. |
| One unreadable session | Retain its exact envelope and quarantine; healthy sessions recover without waiting for its decode. |
| Admission races reassignment | Admission commits under one current assignment or fails; an assignment alone never makes a session executable. |
| Crash before/after admission or unknown commit | Resolve the actual durable outcome under current authority before proceeding; do not invent a lease or infer absence from an old snapshot. |
| Dispatch committed; revocation precedes handoff | Old owner neither releases nor retries; replacement uses the existing recovery rules after advancing its lease. |
| Handoff precedes revocation | Preserve the executor's existing result/receipt semantics; do not report cancellation merely because authority changed. |
| Restore, pause, quarantine, stop/end and resume | Preserve same-commit lease advancement, emergency precedence, restore controls and committed resume acknowledgement. |
| Control association or high-water corrupt/unavailable | Fail closed; no invented authority, empty replacement, successful-recovery claim or admission from an unverified header. |

The last case is a remaining boundary, not a demonstrated repair algorithm. These
clauses do not prescribe how to recreate lost control authority. Any stronger
recovery promise requires separately reviewed semantics and evidence.

## Compatibility And Review Accounting

Three existing file/section/sentence identities would retire; the three replacement
sentences and ten accompanying sentences above would be new identities. The
header/body authenticated-data sentence and the other preserved requirements are
not remapped or counted as replacements. No current planning allocation is changed.

Changing lease interpretation in the stored-form contract can be non-additive for
readers that treat the header as current authority. Before applying this proposal,
the existing contract-version and migration discipline needs a concrete disposition:
compatible coexistence/reader rejection and format migration where necessary. This
proposal selects no version number and does not authorize ignoring old clients.

The exact physical representation, trusted control recovery, complete handoff
enforcement, key/count accounting and source-version compatibility still need
implementation evidence. No benchmark, architecture selection or conformance result
is implied by this wording review.

Read-only independent review: /root/activation_scope_review. The prior proposal's
naive rewrite and staged-preparation failures are evidence against those candidates,
not proof that every representation under the current wording is impossible.
