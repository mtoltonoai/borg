# Owned Blob Service Over Existing Durability

Proposed direction, 2026-10-12. Matthew asked whether Borg should build its own
object service integrated with PostgreSQL metadata. This question does not adopt
a production backend, durable layout, protocol, cipher or replication design.

## Recommendation

Evaluate a small Borg-owned blob service using the existing owner/pin component
and PostgreSQL durability as a candidate for the intended production architecture.
Compare immutable ciphertext segments in PostgreSQL with bytes on independently
durable files/volumes while PostgreSQL holds Borg metadata. Neither profile is
selected here. Qualification needs to establish a path to the already-required
simultaneous ten-host agent/workspace load, including bulk workspace snapshots
alongside hot session traffic. Smaller tests exercise portions of that intended
architecture; they do not justify an intermediate implementation that cannot
meet the target. Do not build a new distributed storage engine or a general
storage abstraction before these concrete needs justify one.

The [simultaneous production scale requirement](../implementation.md#simultaneous-production-scale)
applies to both profiles. Hardware, workload, duration and performance criteria
remain explicit qualification inputs; no capacity result or new numeric budget
is asserted by this comparison.

The owned interface is narrow: immutable ciphertext publication with retry,
authorized and integrity-checked reads, current-owner reference/pin changes,
deletion/key-destruction coordination, and recovery. It does not need general S3,
public discovery, mutable files or unrelated object-platform features.

## What We Already Own And What Remains

The existing blob component implements owner/pin transition plans and checks
generation, tenant, holder, scope, expiry, lifetime and identity reuse. Its
destruction result is an intent, not proof that a key was destroyed. It has no
real ciphertext store, production hash/format, sealing or key service.

Those Borg-specific rules remain necessary with PostgreSQL, Ceph, SeaweedFS or
Kubo. Owning the service does not mean we must also own device durability,
replication, placement or storage repair.

## Two Concrete Storage Profiles To Compare

| Profile | Benefit | Qualification cost |
| --- | --- | --- |
| PostgreSQL metadata and ciphertext | One transaction/recovery system, reusing WAL and existing replication/backup machinery; fewer independent publication/recovery boundaries. | Measure blob WAL volume, replication, backup/restore time, hot-session contention and deletion/space recovery against the combined target workload. Define bounded chunks and actual key/value accounting. |
| PostgreSQL metadata and durable files/volumes | Keeps bulk byte traffic out of database WAL and metadata backups. | Own crash-safe byte publication, synchronization error handling and paired metadata/byte recovery. The underlying medium still needs a defined survival/failure-domain contract. |

Large-value support in PostgreSQL makes the first profile technically possible;
it does not override Borg's state/value limits. The existing 64 KiB head, 120 KiB
state, 128 KiB store-value expectation, active-key count and frozen-session rules
still need an explicit compliant mapping. Cold blob rows cannot enter hot session
state or scans merely because the same database engine hosts both roles. Calling
extra data a separate namespace is not sufficient proof.
[Declared defaults](../../../record.md#durable-substrate),
[active working set](../../../core/sessions-and-partitions.md#cost-follows-active-sessions),
[PostgreSQL large-value representation](https://www.postgresql.org/docs/16/storage-toast.html).

## Publication And Recovery Acceptance

For separate byte storage, establish owner/bookkeeping information from the first
write, durably publish complete immutable bytes, then commit the usable reference
and required pin under the current owner's fence. A lost metadata-commit response
needs outcome resolution before exposure, retry or reclamation. A crash may leave
unreferenced bytes, but not an acknowledged reference to bytes that were never
durable. Deferred garbage collection does not permit reclaiming referenced data.

Filesystem publication needs actual file and directory durability and error
handling, not just a successful rename. PostgreSQL needs verified WAL/commit and
storage settings. Independently restoring an older byte backup alongside newer
metadata can lose referenced objects; restore therefore needs evidence that its
byte set covers the restored references. A node-local directory by itself does
not survive the cluster replacement required by the shared source.
[Referenced-blob protection](../../../core/encryption-and-blobs.md#reclamation-never-removes-a-referenced-blob),
[stage resource survival](../../../core/durability-and-deployment.md#stage-resources-outlive-every-cluster),
[PostgreSQL reliability](https://www.postgresql.org/docs/16/wal-reliability.html).

The qualification cases are concurrent duplicate publication; a different body
under a claimed digest; crash at each bytes/metadata boundary; unknown commit;
pin-versus-delete races; corruption; denied versus absent backend reads; restored
metadata paired with sufficient/insufficient bytes; and exactly preserved
ciphertext/digest across recovery. Measure latency and storage costs without
turning measurements into unapproved production budgets.

Client sealing, tenant/owner/range binding, live key destruction, bounded cache
and host-plaintext destruction, and transfer resealing remain independent
requirements. The [v1 backup-key retention exception](../implementation.md#backup-key-retention)
allows keys to remain in backups and point-in-time recovery copies; restoring an
older copy can restore access to deleted data. That exception does not exempt
live blob copies or justify a shared backup-erasure conformance claim. SQL
deletion or file removal cannot substitute for required live key destruction.
[Stored blob contract](../../../contracts/stored-form.md#blobs),
[key destruction](../../../core/encryption-and-blobs.md#deletion-is-key-destruction).

## Decision Boundary

The recommended next evaluation is the two concrete profiles above using the
existing owned ownership/pin logic and approved private dependencies. This is
qualification of potential production layouts, not selection of an interim
store or adoption of another object-store product. Exact production layout,
byte backend and survival policy remain review items.
PostgreSQL's existing approval is for private qualification; this document does
not silently broaden that approval or install another technology.
