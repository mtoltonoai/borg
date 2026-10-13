# Existing Engine And Codec Qualification Proposal

Status: descriptive research and proposed probes only, 2026-10-11. No product,
version, format, adapter, layout, exception or declaration is adopted. No install,
probe, benchmark or runtime test was run. An authoritative-spec write was rejected
by automatic approval review; this document does not retry or record that change.
The approved specifications and completed planning artifacts remain unchanged.

## Recommendation And Mandatory Gates

There is **no conforming winner yet**. Compare PostgreSQL while preserving the
current Borg bounds. Keep FoundationDB only as a conditional exploration: its
100,000-byte native value limit fails the current direct-value mapping. Do not
start by silently lowering bounds or splitting records. For codecs, compare
Protobuf with prost-reflect DynamicMessage against preservation-aware CBOR
representations; no stock decoder establishes all required guarantees.

Mandatory requirements are distinct from implementation cost or convenience:

- [Self-Durable, The Store Survives The Loss Of Every Node, lines 15-17](../../../../decisions/durability/self-durable.md:15): no committed loss after every node is lost, using the store's own write-ahead log, snapshot and index. Equivalent durability without a WAL does not satisfy this text.
- [Self-Durable, A Crash Loses No Committed State, line 39](../../../../decisions/durability/self-durable.md:39), plus [backup/restore and release drills, lines 23-29](../../../../decisions/durability/self-durable.md:23).
- [Shared Framework, Reads At The Woken Version, lines 33-34](../../../../core/shared-framework.md:33) and [Sessions, A Partition Reads At The Woken Version, lines 246-249](../../../../core/sessions-and-partitions.md:246): doorbell and inboxes are read together at the watch's wake version. A notification followed by an arbitrary fresh snapshot is not sufficient.
- [Sessions, A Commit Conflicts Only On The Fence, line 253](../../../../core/sessions-and-partitions.md:253): unrelated appends/helper reads cannot introduce conflict rejections. [Atomic application and unknown commits, lines 259-270](../../../../core/sessions-and-partitions.md:259) require atomic state/cursors and fenced reread before redo.
- [Stored Form, Every Durable Format Is Self-Describing, lines 114-144](../../../../contracts/stored-form.md:114): stable tags at every evolving level, preserved unknown fields, declared fallback or safe rejection, compatible retained records, rejection of excessive values and non-minimal length encodings. [Decoder bounds, lines 150-156](../../../../contracts/stored-form.md:150) also forbid overwriting undecodable bytes and allocation beyond what remaining input can hold.
- [Simulation, Every Dependency Has A Verified Simulator, lines 29-37](../../../../core/simulation-and-conformance.md:29): the same expectations run against the simulator and real dependency; a selected transaction can fail on demand. Documentation is not parity or execution evidence.

The numeric baseline is the original [record, Durable substrate, lines 69-89](../../../../record.md:69): 128 KiB values, state below 120 KiB, two active-session keys and configured transaction-key bounds. These are declared defaults/assumptions, not immutable universal constants, but changing them requires explicit review; this proposal preserves them. The record's [self-durable outcome, line 30](../../../../record.md:30) says committed storage outlives nodes. Losing all compute nodes, retaining their durable medium, and destroying every durable copy are different fault models. Replication or an asynchronous backup proves none of those boundaries automatically.

## Transactional Engine Shortlist

| Candidate | Primary-source facts | Qualification judgment and owned complexity |
|---|---|---|
| PostgreSQL | Atomic transactions, MVCC, WAL recovery, snapshots and transactional NOTIFY. The documented field limit is 1 GB, so the current Borg value sizes are not rejected by that hard limit. | Conditional baseline candidate, not accepted. Large values can use TOAST, so the declared no-cost-step expectation still needs measurement. NOTIFY itself supplies no snapshot token, but the Borg contract does not require the notification emitter's exact commit. A candidate can establish and return a verified snapshot-bound wake handle, then consume that same snapshot. Keeper lifetime and fence-only writes remain unproved; see the current qualification appendix. |
| FoundationDB | Multi-key transactions, explicit read versions, snapshot reads, selective conflict ranges, ordered keys and watches. Native values are limited to 100,000 bytes. Watches report readiness/error, not a trigger-version value. | **Direct native-value mapping is eliminated under current bounds.** The 120 KiB state and 128 KiB value assumptions exceed the limit; the roughly 115 KB undecodable index element also needs attention. Chunking, changed physical/logical key accounting or reduced defaults needs separate review before qualification could proceed. Attractive APIs do not offset this failure. |

PostgreSQL evidence: [limits](https://www.postgresql.org/docs/current/limits.html),
[TOAST](https://www.postgresql.org/docs/current/storage-toast.html),
[isolation](https://www.postgresql.org/docs/current/transaction-iso.html),
[NOTIFY](https://www.postgresql.org/docs/current/sql-notify.html),
[LISTEN initialization race](https://www.postgresql.org/docs/current/sql-listen.html),
[exported snapshots](https://www.postgresql.org/docs/current/functions-admin.html#FUNCTIONS-SNAPSHOT-SYNCHRONIZATION).
The current documentation identifies PostgreSQL 18; no release is selected here.

FoundationDB evidence: [hard limits and transaction lifetime](https://apple.github.io/foundationdb/known-limitations.html),
[C API watch/read-version/conflict behavior](https://apple.github.io/foundationdb/api-c.html),
[transaction-log design](https://github.com/apple/foundationdb/blob/main/design/tlog-spilling.md.html),
[backup/DR limitations](https://apple.github.io/foundationdb/backups.html).
The published docs identify 8.0.0; that is an inspected documentation version,
not selection of a binary. A watch can miss a value that changes and changes back;
a monotonic-doorbell proposal would still need review and the exact-version probe.

Do not substitute generic SERIALIZABLE for Borg's conflict contract: SQL isolation
can abort for other concurrent changes, and ordinary FoundationDB reads add
conflicts. Neither an opaque notification nor an exported/current snapshot alone
proves the required wake-version relation. Both engines' actual internals running
under Bach are unverified. An interface simulator does not test the real engine.

Durability settings are acceptance constraints. PostgreSQL `fsync=off` or
`synchronous_commit=off` cannot establish the promised durability; `remote_write`
alone does not establish survival of a standby OS crash. The selected storage and
failover boundary must preserve acknowledged writes. See [WAL settings](https://www.postgresql.org/docs/current/runtime-config-wal.html)
and [storage reliability](https://www.postgresql.org/docs/current/wal-reliability.html).
FoundationDB's asynchronous backup/DR can lag; it is not a zero-loss replacement
for surviving committed storage.

**Not advanced as turnkey alternatives:** RocksDB offers transactions, snapshots
and WAL, but adding a distributed store boundary and version-bearing notification
protocol is significant owned construction. Its asynchronous write options do not
prove power-loss durability ([transactions](https://github.com/facebook/rocksdb/wiki/Transactions),
[operations](https://github.com/facebook/rocksdb/wiki/Basic-Operations)).
WAL-less copy-on-write engines fail the literal WAL requirement unless the
original requirement is separately changed; no such change is proposed here.

SQLite remains an embedded probe substrate only, not a selected store. WAL is
same-host and does not work over a network filesystem. Its WAL-reset corruption
bug was fixed in 3.51.3; 3.52.0 was withdrawn, and 3.53.0 includes the fix. A future
trial requires an exact patched source/build, not whichever binary is installed.
WAL `synchronous=NORMAL` can lose committed transactions after system/power failure;
the documented stronger setting is necessary for that durability claim but does
not prove all-node survival. Sources: [WAL sections 1 and 11](https://sqlite.org/wal.html),
[release history](https://www.sqlite.org/news.html),
[synchronous behavior](https://sqlite.org/pragma.html#pragma_synchronous).

## Proposed Store Qualification Plan

Stop at a failed mandatory gate; a later benchmark cannot compensate. These are
proposed cases, with no results asserted and no numeric stress budget selected.

| Gate | Concrete common simulator/real-engine expectation and failure cases |
|---|---|
| S0: Static fit | Pin engine source/build, binding, configuration and storage semantics. Exercise current maximum state/value/index sizes and key counts. Reject unsupported bounds or an undeclared split/layout. FDB's direct mapping already fails the documented value limit. PostgreSQL hard-size fit leaves physical cost and key accounting unproved. |
| S1: Versioned wake | Register before/after append and during registration; commit append at v1, delay consumption, then commit v2 and trims. The returned wake identity must support one consistent doorbell/inbox read at its defined version, including the triggering append. Reject substitution of latest/stale reads. Cover empty-key first append, change-back, cancellation/replacement, reconnect, notification loss and snapshot expiry without periodic polling or missed work. |
| S2: Fence and atomicity | Race a publisher append with state/cursor/trim commits under unchanged fence: no torn effects, lost append, premature trim or unrelated conflict rejection. Include reads hidden inside helpers. Move ownership, then restore the stale writer's access: reject its commit. Compare actual receipt/effect and inbox/doorbell atomic groups to independent expected state. |
| S3: Chosen transaction ambiguity | Identify the transaction actually failed. Inject rejection before apply, loss of connection/reply after apply, and an outcome that cannot initially be distinguished. Only proven abort is reported as no commit. Unknown outcomes are resolved by fenced reread before redo, retaining logical operation identity and one effect. Audit automatic driver retries rather than assuming they preserve this rule. |
| S4: Persistence and recovery | Kill the process at commit/log/checkpoint boundaries; every acknowledged commit survives restart. Distinguish process crash from OS/power failure. Separately replace all nodes while the declared durable medium survives; restore using the store's own backup and measure the committed frontier. An older asynchronous backup cannot be called zero-loss. Identify WAL, snapshot and index artifacts and their actual recovery roles. |
| S5: Dependency failures | Activate delayed/dropped/reordered requests, throttling, disk-full, permission failure, corruption and expired snapshot behavior. Distinguish retryable failure, unknown commit and safe refusal. Check actual peer authentication, not a synthetic verified-caller flag. The simulator and engine probe use the same assertions but independently implemented outcomes. |
| S6: Sealing boundary | Write only client-sealed tenant content. Inspect store values/logs/backups for a distinctive synthetic plaintext marker and permitted metadata leakage. Wrong tenant/owner/range binding fails. Destroy an owning key and attempt recovery from live storage, cache and backup after the configured window; retained wrapped keys must not reopen it. This requires approved real key/envelope components, not a database encryption-at-rest checkbox. |

A small DA/S2 restart proof is useful but does not finish DV/S6 or DR/S6. Exact
real-engine simulation status, production adapters, surviving storage, real-probe
access and all component-level third-party boundaries remain unapproved/unverified.

## Tagged Codec Shortlist

| Candidate | Preservation path | Missing guarantees and owned work |
|---|---|---|
| Protobuf + prost-reflect DynamicMessage | Documented unknown-field retention and nested dynamic messages; inspect/edit/rewrite the preservation-aware representation. | Not plain generated prost. Transcoding to a generated type can drop unknowns; future oneof members can look unset. Descriptor/variant policy, strict length rejection and allocation control are required work, not stock guarantees. |
| CBOR + Ciborium Value/maps | Dynamic maps can retain unknown keyed values and nested structures. | Borg field identities need an explicit stable-key/tag profile at every evolving level; CBOR semantic tags alone do not provide that schema. Strict input rules, unknown variants, duplicate handling and bounded decoding need review. Ordinary typed Serde conversion is not a preservation path. |

Inspected documentation: [prost-reflect DynamicMessage 0.16.5](https://docs.rs/prost-reflect/0.16.5/prost_reflect/struct.DynamicMessage.html),
[nested messages](https://docs.rs/prost-reflect/latest/src/prost_reflect/dynamic/message.rs.html),
[Ciborium Value](https://docs.rs/ciborium/latest/ciborium/value/enum.Value.html)
(released documentation 0.2.2), [Serde unknown-field behavior](https://serde.rs/container-attrs.html),
and [RFC 8949](https://www.rfc-editor.org/rfc/rfc8949.html).
These versions are evidence references, not approvals or installed dependencies.

**Targeted CBOR alternative: retain minicbor for qualification.** Its 2.3.0
Decoder borrows input and byte/string slices; input and position access could
support retained raw unknown spans with less dynamic-tree allocation. This is a
building block, not automatic preservation: skip only advances, while retaining
unknown spans through unrelated nested edits and rebuilding enclosing lengths
is owned work. Alloc-enabled skip uses a vector stack for some nested indefinite
containers; the nonallocating path rejects those shapes. Derived/typed decoding
may allocate independently, so no_std alone proves nothing about the full path.
The inspected length-reading path also lacks minimal-width rejection. Retain it
within the existing CBOR branch for K0-K4, including strict lengths, nesting,
raw-span lifetime and semantic edit/rewrite checks; select no version or profile.
Source: [official minicbor 2.3.0 decoder implementation](https://docs.rs/minicbor/2.3.0/src/minicbor/decode/decoder.rs.html).
No minicbor test or probe was executed.

Neither deterministic output nor an unchanged round trip proves conformance:

- Prost's varint decoder accepts terminated overlong representations by source
  inspection. For a bytes field, `0a 81 00 aa` encodes length one non-minimally;
  Borg requires rejection, not normalization to `0a 01 aa`. No probe was run.
  See [prost 0.14.4 decoder](https://docs.rs/prost/0.14.4/src/prost/encoding/varint.rs.html)
  and [length readers](https://github.com/tokio-rs/prost/blob/master/prost/src/encoding.rs).
  [Protobuf explicitly distinguishes deterministic from canonical serialization](https://protobuf.dev/programming-guides/serialization-not-canonical/).
- An old reader cannot necessarily distinguish unset oneof from a future unknown
  member. Test an unrelated-field edit and both orderings of known/unknown union
  alternatives through the actual representation; preservation of an unknown byte
  bag alone does not prove preserved meaning. See [oneof compatibility](https://protobuf.dev/programming-guides/proto3/#backwards-compatibility-issues).
- Ciborium accepts wide length representations through its [header decoder](https://github.com/enarx/ciborium/blob/main/ciborium-ll/src/hdr.rs).
  [CanonicalValue](https://github.com/enarx/ciborium/blob/main/ciborium/src/value/canonical.rs)
  is not strict input validation. Its [dynamic map visitor](https://github.com/enarx/ciborium/blob/main/ciborium/src/value/de.rs)
  on repository `main` reserves from declared small map counts before reading
  entries; [size hints](https://github.com/enarx/ciborium/blob/main/ciborium/src/de/mod.rs)
  propagate declared counts. This is a main-source observation, not a finding
  about released 0.2.2, whose Value map visitor starts with `Vec::new()`. A
  truncated `a1` map remains a proposed allocation case for the exact candidate
  build, not an executed result or demonstrated 0.2.2 behavior.
- Dynamic tree/map/vector overhead can exceed encoded size even for valid input.
  A message-size cap or prevalidation alone does not prove stored-form line 156.
  The exact decoder allocation path must be measured and constrained without
  silently relaxing the original requirement. If this needs a different bounded
  representation, that is additional owned design requiring review.

Additional strict validation must reject non-minimal lengths **before** the
permissive allocation/decoding path. It must cover known nested messages and
container lengths while preserving unknown opaque payloads without guessing their
schema. Its parser, allocation accounting, compatibility and fuzzing cost belongs
in the comparison; it is not a free adapter.

**Comparators/eliminations:** rust-protobuf has explicit [UnknownFields](https://docs.rs/protobuf/3.7.2/protobuf/struct.UnknownFields.html),
but its [project status](https://github.com/stepancheg/rust-protobuf) says it is
approaching end of life; do not prefer it for a new permanent format without a
maintenance decision. Plain prost generated structs [skip unknown fields](https://github.com/tokio-rs/prost/blob/master/prost-derive/src/lib.rs).
Ordinary FlatBuffers generated object unpack/edit/repack reconstructs known
fields and is eliminated for this path; this is an inference from the
[generated Rust object code](https://github.com/google/flatbuffers/blob/master/tests/monster_test/my_game/example/monster_generated.rs),
not a claim about every possible raw-buffer strategy.

## Proposed Codec And Sealing Corpus

| Gate | Required discrimination |
|---|---|
| K0: Evolving structure | Validate nonreused field/variant tags at outer, nested, variant-payload and wrapped-key levels. A tagged outer envelope around an untagged evolving body fails. Check declared missing-field defaults and enum fallback/no-fallback behavior. |
| K1: Real edit/rewrite | New writer stores unknown fields at every level; old reader edits an unrelated known nested field, persists through the actual representation/conversion path; new reader verifies unknown values and original union meaning. Include ordered conflicting known/future oneof alternatives, unknown enum values, dynamic-to-generated conversion and partial record updates. Identity decode/reencode alone cannot pass. |
| K2: Strict lengths and allocation | Reject overlong zero/one and boundary-width lengths, truncated payloads, declared lengths larger than remaining input, nesting beyond the approved bound and small-count allocation amplification. No depth budget is selected here. Instrument actual allocations, including representation overhead; do not merely check reserialized equality. Test exact bound and bound+1 without truncation. Fuzz the strict validator and actual decoder together. |
| K3: Preserve on failure | Unknown unsafe/no-fallback variant and corrupt sealed record fail closed while original stored bytes remain untouched. Distinguish absent, valid and corrupt; reject plaintext where a sealed value is due. A fallback must not erase unknown data during an unrelated edit. |
| K4: Retained compatibility | Keep recorded old encodings, test old/new adjacent forms during rollout, unknown tags surviving nested rewrites and continued readability under prior envelope/key generations. The envelope format identity stays independent of body schema. Relevant original tests: [simulation lines 86-88](../../../../core/simulation-and-conformance.md:86). |
| K5: Client sealing and destruction | Seal before store writes; verify authenticated tenant/owner/range/session/event binding, deterministic immutable-blob mode and fenced re-sealing. Check live/cache/backup key destruction. Codec preservation cannot authorize retention of usable destroyed key material. Sources: [Stored Form lines 23-40](../../../../contracts/stored-form.md:23), [binding lines 50-66](../../../../contracts/stored-form.md:50), [client sealing line 106](../../../../contracts/stored-form.md:106), [Encryption And Blobs, Deletion Is Key Destruction](../../../../core/encryption-and-blobs.md:120). |

The real codec implementation should be exercised inside the proposed owned
component simulation; a fake codec round trip is not library qualification.
External key/store simulators still need common real probes. Bach integration and
third-party internals/exclusions are not established by these source readings.
The current Borg manifests declare no external Rust dependencies; neither codec
nor either engine is integrated by this research.

## Smallest Next Review Scope

1. With current bounds unchanged, review a concrete PostgreSQL wake-version and
   fence-only protocol on paper. If it cannot satisfy both, stop before building
   an adapter. This is feasibility work, not selection of PostgreSQL.
2. Decide whether to authorize a **separate FDB value-limit exploration**. It must
   compare a reviewed representation/key-accounting change or an explicitly
   reviewed default change against retaining the current baseline and excluding
   FDB. No splitting or reduced bound is implicit in the shortlist.
3. Review the strict codec profile and allocation boundary for the two candidates,
   then, only under new explicit execution authorization, run the same small
   K0-K4 corpus using exact pinned builds. A source-inferred failure remains an
   unexecuted prediction until measured. Sealing/key choices are separate.
4. Any later probe authorization must state exact versions/builds, isolated local
   resources, allowed faults, cleanup and surviving-media boundary. It must not
   select a production deployment or treat a trial result as adoption.

Storage and codec researchers worked independently; a third read-only reviewer
challenged literal WAL, value bounds, watch versions, conflict rules, length
rejection, allocation and union preservation. The resulting recommendation is
conditional feasibility review, not a product winner. See the separately staged
[contract directions](contract-direction-clarifications.md) and
[proposed approval-record wording](requested-direction-approval-record-proposal.md).
The 1,817/1,817 report is historical. The [current preparation receipt](../../../../../../borg-runtime/artifacts/planning/review-prep-l409c9h1/completion.md) records actual current-source 1,846-item native coverage and review provenance. Neither report approves this research packet.

[Current qualification appendix](qualification-appendix.md) adds the bounded snapshot/fence candidate and strict codec vectors. Existing-engine and existing-codec qualification are established operator directions; authoritative recording remains blocked. Product, library, profile and probe-environment choices are still proposed, not adopted.
