# Qualification And Contract Detail Appendix

Everything below is PROPOSED detail for review, not product/library selection or an authoritative contract amendment. The operator already expressed reuse-before-own-build, constituent-preserving latest-state delivery, and publication-time entitlement directions. Their authoritative recording remains blocked; these notes do not retry it. No engine/codec probe was executed.

## Transactional-store candidate

Recommendation: qualify a PostgreSQL snapshot-bound wake protocol with current Borg bounds preserved. Compare a same-transaction read handle first with exported-snapshot handoff (extra keeper lifetime and cleanup). The requirement is a consistent version represented by the wake, not necessarily the transaction that emitted NOTIFY. A repeatable-read, read-only snapshot verifies durable append progress and is then consumed as that exact snapshot. Export/import is only an alternative; never return a token and substitute a fresh snapshot later. Official [snapshot synchronization](https://www.postgresql.org/docs/18/functions-admin.html#FUNCTIONS-SNAPSHOT-SYNCHRONIZATION), [isolation](https://www.postgresql.org/docs/18/transaction-iso.html), and [transaction setup](https://www.postgresql.org/docs/18/sql-set-transaction.html) establish the relevant mechanics, not Borg conformance.

Proposed watch: commit LISTEN before catch-up; arm a local notification latch before snapshot/check/sleep. Persist inbox, doorbell and monotone append progress atomically with notification. Reconnect re-registers and catches up from durable progress. A lost/expired handle fails explicitly without advancing committed progress. Silent client loss of a hint on a healthy connection is not solved by this sketch. [LISTEN](https://www.postgresql.org/docs/18/sql-listen.html), [NOTIFY](https://www.postgresql.org/docs/18/sql-notify.html).

Proposed writes: short read-committed transactions acquire the actual fence-row lock, validate generation and hold it through state/cursor/trim updates; ownership changes use the same lock. Delta-trim fresh locked queue state rather than overwrite a stale snapshot. Unknown-commit recovery reacquires that fence lock before checking durable identity/absence. A global consistent lock order is essential: deadlocks can abort without a changed fence; repeatable-read writes can also fail on unrelated appends. [Explicit locking](https://www.postgresql.org/docs/18/explicit-locking.html).

Discriminating probes: return S1 then append S2 and still read S1; append before snapshot selection and legitimately return S2; lost notification/reconnect catch-up; expired handle with unchanged committed cursor; append versus fence-only conflict; uncertain original commit versus recovery; concurrent publisher/trim without lost tail; opposite lock-order attempts; current 128 KiB value/120 KiB state, two-active-key and transaction-key bounds; snapshot lifetime/VACUUM pressure and notification-queue failure. Pin exact candidate build and surviving medium before interpreting results. Simulator/real parity and all-node survival remain unproved. Source references include [REL_18_0 snapshot manager](https://github.com/postgres/postgres/blob/REL_18_0/src/backend/utils/time/snapmgr.c) and [process array](https://github.com/postgres/postgres/blob/REL_18_0/src/backend/storage/ipc/procarray.c).

FoundationDB direct-value mapping still has a hard current-bounds mismatch; no implicit key splitting or default reduction is proposed as an accepted fix. SQLite requires an exact patched WAL build and durable synchronization settings; it is not selected by convenience. See the existing engine comparison for sources and limits.

## Event-hook detailed contract card

Established direction: retain each constituent event's required information/identity while applying only latest state. Proposed recommendation: freeze a coalescing cut through a high sequence; transfer it as bounded ordered units. Intermediate units durably retain constituent records without applying intermediate states. Whole-unit acknowledgement follows durable receipt; the final unit acknowledges only after latest-state application and completion receipt commit atomically. The sender advances a contiguous acknowledged prefix. Once attempted, a unit's logical identity/content is immutable across retry/failover; exact duplicate returns its receipt, changed content under the same ID and gaps/partial overlaps do not advance progress. A retry may use a fresh signed transmission timestamp with identical logical content. Signature binds tenant/session/destination, cut/unit, constituent identities/order/information and latest-state binding. Invalid signature, failed persistence or final state application produces no acknowledgement.

Whole-unit ACK is recommended over selective constituent ACK: fewer reconciliation states, at the cost of retransmitting a bounded unit. Tests: idle10 then blocked11 retains both identities/reason but only applies state11; lost final ACK causes no second effect; crash before final commit has no ACK; a fragmented cut10-100 applies no intermediate state91 times; changed reason under the same ID is rejected. These are proposed tests, not executed evidence.

Unresolved authoritative wording: ordered first acceptance versus late duplicate arrival; terminal retry versus the existing bounded-attempt default; receipt/constituent retention; compatibility/version migration. Recommend bounded concurrent retry/backoff rather than silently dropping events after a retry count. Finite capacity, indefinite outage, lossless history and sessions never affected cannot all be guaranteed without an operating envelope or an explicit changed guarantee. Batching does not solve this. Preserve the original [durable delivery, coalescing, session isolation, receiver and evolution sections](../../../../contracts/event-hook.md); all companion semantics remain outside those contracts until reviewed.

## Subscription detailed contract card

Established direction: entitlement at publication; unsubscribe stops later eligibility without retracting earlier entitled work, subject to current security, retention and explicit drops. Proposed recommendation: future-only initial subscription/rejoin with prior entitled debt preserved. Membership changes use fenced publication order, never wall-clock comparison. Rejoin opens a new eligibility interval and excludes the gap, without erasing old debt. Denied operations change nothing.

Example: subscribe at head100; publish101,102; unsubscribe; publish103,104; rejoin at head104; publish105. Owed:101,102,105, not103/104. Late fan-out still owes101/102. Actual retention expiry reports a dropped range; a current read denial still blocks delivery despite historical entitlement. Epoch loss is reported, not compared as plain counters. Rejoin does not change dedupe identity for101. Resynchronization repairs prior entitlement only; gap replay/reexecution is a separate future authority/API choice. This recommendation does not borrow the viewer no-history rule. Compare with explicitly authorized retained-history replay when required by a consumer; it adds scope, budget and dedupe/migration decisions. Consumer inventory and compatibility review precede declaring either change additive. [Original topic requirements](../../../../core/events-and-subscriptions.md), [epochs](../../../../contracts/event-envelope.md), [dedupe](../../../../core/sessions-and-partitions.md), [current authority](../../../../core/identity-delegation-and-secrets.md).

## Codec and every-level dependency review

The independent source-only findings and concrete vectors follow. They are not executed outcomes and do not select a format or approve an exception.

### Independent codec qualification preparation

Descriptive recommendations only. No format, library, language, exception, or runtime design is adopted. No probes, installs, or product code were written. Findings below are source-derived, not executed test results.

## Recommendation

Qualify a strict CBOR profile with retained borrowed byte spans first; keep Protobuf as a constrained alternative and Ciborium's dynamic tree as a comparator. The proposed profile uses definite-length containers, minimal encodings, unique deterministically ordered map keys, integer field tags for evolving records, and an explicit variant discriminator plus tagged payload. Apply the same rules at nested and wrapped-key levels. RFC 8949 provides building blocks, not an existing Borg profile: https://www.rfc-editor.org/rfc/rfc8949.html#section-4.2.1

Retain original input, borrow validated spans for unknown fields, record bounded edits to known fields, and rebuild affected containers while preserving untouched unknown spans. A fallback is a read interpretation; it must not replace an unknown variant's stored discriminator/payload.

## Candidate findings

- minicbor 2.3.0 exposes borrowed input/positions but accepts wider-than-minimal lengths. skip() advances, without retaining edit history; its allocating skip path can allocate for nested indefinite containers. Strict validation, span retention, editing, depth limits and allocation accounting remain owned work. Exact source: https://docs.rs/minicbor/2.3.0/src/minicbor/decode/decoder.rs.html
- Released Ciborium 0.2.2 Value map decoding starts with Vec::new() and pushes entries. Do not attribute later-branch preallocation to this release. Header conversion accepts nonminimal lengths. Dynamic trees preserve generic unknown keyed values but may allocate much more than encoded input. Exact release sources: https://github.com/enarx/ciborium/blob/v0.2.2/ciborium/src/value/de.rs and https://github.com/enarx/ciborium/blob/v0.2.2/ciborium-ll/src/hdr.rs
- prost-reflect 0.16.5 DynamicMessage retains unknown fields but field-number ordering can change evolving oneof meaning. prost 0.14.4 accepts nonminimal varint lengths. These are inspected versions, not a dependency lock; prost-reflect's range does not pin this prost release. Sources: https://docs.rs/prost-reflect/0.16.5/src/prost_reflect/dynamic/fields.rs.html and https://docs.rs/prost/0.14.4/src/prost/encoding/varint.rs.html

Protobuf counterexample: V1 knows oneof field 10; V2 also knows field 20. Input A0 01 02 50 01 means field20=2 followed by field10=1, so field10 wins for V2. Old re-encoding orders fields as 50 01 A0 01 02, making field20 win when V2 reads it. An unrelated ordinary-field edit does not prevent this semantic change. Retaining unknown bytes alone is insufficient. Official semantics: https://protobuf.dev/programming-guides/proto3/#oneof

## Proposed qualification vectors

- K0 declarations: stable/nonreused tags at every evolving level; removed-tag reservation; explicit absence/default behavior; enums with and without safe fallback. Record maps must not become positional arrays. Distinguish absent, present zero and unknown enum numbers in Protobuf.
- K1 edit/rewrite: CBOR A2 01 A2 01 01 18 63 43 AA BB CC 18 64 A1 01 07. Change nested field1 from1 to2; nested unknown99 and outer unknown100 survive. Exercise actual storage conversion. Protobuf includes both oneof orderings, nested/repeated messages and map values. Generated-message and JSON conversion are separate potentially lossy routes.
- K2 strict input: CBOR canonical41 AA versus overlong58 01 AA; reject58 00, B8 01 01 00, truncatedA1, and58 02 AA. Protobuf canonical0A 01 78 versus0A 81 00 78; include0A 80 00. Test boundary widths, huge lengths with short input, malformed suffixes, tiny collections, nesting and actual allocations. Reject rather than normalize/truncate.
- K3 failure: malformed or unsupported no-fallback variants leave stored bytes untouched. Never persist a partial/default decoded object. A safe fallback preserves the unknown representation through unrelated edits.
- K4 compatibility: old/new writers/readers; new writer -> old reader edit -> new reader; adjacent rollout; removed variants; independent envelope/body versions. Compare meaning/content, not only successful decode. Preserve exact fixture/schema/features/toolchain identities.

Protobuf LEN ambiguity: in 1A 04 0A 81 00 78, an old reader cannot know whether unknown field3 contains opaque bytes or a nested message with a nonminimal length. Recursively treating unknown bytes as messages is invalid. https://protobuf.dev/programming-guides/encoding/

Two consequential requirement meanings remain open: allocation accounting (each request, peak live storage, representation, or all working memory against remaining input) and added-field fallback's reader/schema/record relationship. Report input storage, decoder state, indexes, edits, scratch and output separately. Do not silently replace the bound with an amplification ratio or equate fallback to proto3 zero.

## Third-party consumption and simulation

- Retained Duvet0.4.3/Duvet Cap/helpers are planning/conformance tooling, not Bach product-runtime evidence. Classification is not an automatic exception.
- Future owned validator/span editor/adapter must run actual owned logic under Bach with Bolero inputs and approved coverage obligations.
- Candidate codec/transitive libraries: inventory direct and every indirect consuming level; execute actual library code, not only adapter fixtures.
- Driver/transport/TLS/runtime: controlled I/O does not cover ambient threads/clocks/randomness or replaced implementations; record these gaps.
- Store engine: simulator/real-probe parity differs from simulation of real engine internals. A substantive exception needs Matthew's direct approval.
- Crypto/compression: actual implementations and controlled dependencies/entropy, not interface substitutes.
- External model/tool/key/identity services: identify exact consumed surface, dependency simulations, real probes and unverified internals separately.
- Archived Rust prototype is historical; its old manifest does not define maintained future dependencies.

Any exception must identify exact component/build, consumption levels, excluded behavior, remaining tests and implications. Exceptions cannot waive mandatory stored-format correctness.

## Reviewed local source identities

stored-form.md: dd1a4eef9d0de5fb00cd5c458f66b5159dde0b4720c5b67c7dae71c71564523c
implementation.md: 675bf4b8d0f55a181af0e4d32e481b663ea459d77db8dd7f3e333311a4a94926
engine-codec-qualification.md: dfe8f852820e7f82ccb28f4b9547b7b5fd9497928c74857a807243884015937a
