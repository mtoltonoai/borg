# DC QUIC Patterns For Borg: Proposal For Review

**Status: unapproved research proposal.** Matthew requested this source review.
That authorizes the research, not adoption of a pattern, dependency, or text in
the implementation specification. Nothing in this document has been added to
`spec/implementation.md`. No DC QUIC architectural pattern has been integrated
into the current Borg simulator. Approve exact additions separately.

Source: the local `s2n-quic` fork at commit
`dd6f2e82cf5636b7fa2d001b92c33452a88d3373`. Its checkout remains clean. This review
read source and instructions; it did not run the fork's tests or benchmarks.
References below point to the inspected checkout and are tied to that commit.

## Findings That Affect The Proposal

The simulator is **Bach**, package `bach`, rather than Bock. The fork's
[testing instructions](../../../../../s2n-quic/CLAUDE.md) prefer it for deterministic
discrete-event testing and explain that it runs cooperative tasks on one thread.
That can reproduce races across suspension points, but it cannot exercise every
atomic-memory interleaving of actual threads.

DC QUIC uses substitutable runtime boundaries in important paths; it is not
wholly independent of Tokio. Its [manifest](../../../../../s2n-quic/dc/s2n-quic-dc/Cargo.toml)
enables Tokio by default at line 14, always includes Tokio synchronization at
line 75, and enables Bach's `tokio-compat` at line 51. The
[runtime interface](../../../../../s2n-quic/dc/s2n-quic-dc/src/runtime.rs) is at line 25;
its Bach implementation is at line 257 and its Tokio implementation at line 384.
The proposed lesson is to isolate the capabilities components use so they can
run under simulation, not to claim that the reference has no Tokio dependency.

The present Borg milestone is a standard-library-only test substrate. Whether
to integrate Bach, Bolero, Loom, a production runtime adapter, a metrics crate,
or a chunked-byte implementation remains open. The reference repository is
valuable even if none of those crates becomes a direct dependency.

## Simulation And Runtime Boundaries

### Small Interfaces Around External Capabilities

DC's `Runtime` binds a clock and a worker-local spawner, and its `Spawner`
interface accepts futures without putting a concrete scheduler in each component:
[runtime.rs:25](../../../../../s2n-quic/dc/s2n-quic-dc/src/runtime.rs:25) and
[runtime.rs:49](../../../../../s2n-quic/dc/s2n-quic-dc/src/runtime.rs:49).
The endpoint accepts generic receive/send sockets and a runtime at
[endpoint.rs:429](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint.rs:429).
The simulator supplies its runtime at
[sim.rs:439](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/testing/sim.rs:439)
and calls the shared setup at
[sim.rs:487](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/testing/sim.rs:487).

**Proposal:** exercise actual Borg component logic through replaceable store,
clock, transport, and provider interfaces. Extract each shared interface from
working real and simulated cases, rather than copy DC's full worker abstraction.
This follows [Borg's concrete-primitives rule](../../../../constitution.md#i-minimal-and-concrete)
and [dependency simulation](../../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator).
Tradeoff: more explicit interfaces and generic types require care, but keep test
behavior from becoming a separate implementation of the platform.

### Separate Scheduler Time From Node Wall Clocks

DC's [precision clock and timer traits](../../../../../s2n-quic/dc/s2n-quic-dc/src/time/precision.rs:6)
provide timer operations; its
[Bach clock](../../../../../s2n-quic/dc/s2n-quic-dc/src/time/bach.rs:23) and
[Tokio clock](../../../../../s2n-quic/dc/s2n-quic-dc/src/time/tokio.rs:17)
adapt time to the execution environment.

**Proposal:** preserve Borg's explicit per-node wall clocks when evaluating Bach.
The inspected traits alone do not establish injectable node-specific offsets,
drift, and steps required by
[Borg's time dependency](../../../core/simulation-and-conformance.md#time-is-a-dependency).
An absolute scheduler deadline and a timer due by an owner's wall clock are
different concepts. Test no-early-fire, held fires on clock faults, recovery, and
missed occurrences separately. The current queue/sleep code covers only the
simulation substrate, not those durable timer guarantees.

Do not copy DC's busy-poll policy: its
[busy-poll receiver](../../../../../s2n-quic/dc/s2n-quic-dc/src/socket/recv/socket.rs:152)
expects repeated polling, whereas
[Borg's event-driven rule](../../../../constitution.md#ii-event-driven-never-polling)
requires notification where the dependency can provide one.

### Structured Faults, Shrinking, And Independent Models

DC derives generated run-length packet-loss inputs at
[deterministic.rs:45](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/deterministic.rs:45),
injects them through monitors at
[line 147](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/deterministic.rs:147),
and combines Bolero with bounded simulated liveness and a shrink-time budget at
[line 430](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/deterministic.rs:430).
Its generated delay/duplication case begins at
[line 486](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/deterministic.rs:486).
A separate operation/model test runs 10,000 cases and checks invariants after
each operation at
[state/tests.rs:277](../../../../../s2n-quic/dc/s2n-quic-dc/src/path/secret/map/state/tests.rs:277).

**Proposal:** generate typed Borg actions: publish, duplicate, crash, ownership
move, commit failure, lost response, configuration change, and clock fault.
Run them against the implementation and an independent small reference model;
retain minimized failures as regression fixtures. Record seed and action sequence.
This supports [Borg's generated properties](../../../core/simulation-and-conformance.md#properties-are-generated-and-checked-against-a-model).
Tradeoff: generators need valid preconditions and explicit recovery assumptions
for liveness. A deterministic seed alone neither shrinks a failure nor proves the
model is independent. Current Borg queue tests provide repeatable seeds and a
vector model, but do not yet provide Bolero shrinking or full-platform scenarios.

### Controllable Dependency Completion And Failure

The filesystem [backend interface](../../../../../s2n-quic/dc/s2n-quic-dc/src/fs/backend.rs:57)
has a [Bach processing closure](../../../../../s2n-quic/dc/s2n-quic-dc/src/fs/backend/bach.rs:119)
and [simulated completion scheduling](../../../../../s2n-quic/dc/s2n-quic-dc/src/fs/backend/bach.rs:181).
A [failure test](../../../../../s2n-quic/dc/s2n-quic-dc/src/fs/tests.rs:57)
supplies a failing processor.

**Proposal:** represent latency, tails, refusal, timeout, cancellation, duplicate,
reordering, and partial outcomes explicitly at each Borg dependency boundary.
Preserve the current distinction between precommit failure and postcommit lost
acknowledgement. The same expectations still need to run through a real dependency
probe, as [Borg requires](../../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator).
Replacing I/O with callbacks alone does not establish simulator fidelity.

### Diagnostics And Separate Concurrency Tests

DC's [simulation runner](../../../../../s2n-quic/dc/s2n-quic-dc/src/testing.rs:281)
uses [simulated timestamps](../../../../../s2n-quic/dc/s2n-quic-dc/src/testing.rs:263)
and [trace comparison](../../../../../s2n-quic/dc/s2n-quic-dc/src/testing.rs:314).
Its [handshake test](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/bach_handshake.rs:4)
explicitly identifies itself as a feasibility spike, not the DC PSK path; it
[disables unstable TLS trace snapshots](../../../../../s2n-quic/dc/s2n-quic-dc/src/endpoint/tests/bach_handshake.rs:51)
while preserving semantic round-trip checks.

**Proposal:** combine deterministic, content-safe traces with semantic invariant
assertions. Keep snapshots focused on stable behavior. If Borg adds shared-memory
concurrency, separately test it with a mechanism such as the fork's
[Loom credit model](../../../../../s2n-quic/dc/s2n-quic-dc/src/credit/pool/loom.rs:121).
The metrics crate also uses a
[real-thread page-conservation test](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/rseq.rs:1161)
for a hazard single-threaded Bach cannot reproduce. Neither deterministic
simulation nor thread stress alone replaces the other.

## Events And Metrics

DC declares [typed events](../../../../../s2n-quic/dc/s2n-quic-dc/events/endpoint.rs:25),
generates a [Subscriber interface](../../../../../s2n-quic/dc/s2n-quic-dc/src/event/generated.rs:2655),
and supports [subscriber composition](../../../../../s2n-quic/dc/s2n-quic-dc/src/event/generated.rs:3570).
Its metrics layer
[registers and retains recorders](../../../../../s2n-quic/dc/s2n-quic-dc/src/event/generated/metrics/aggregate.rs:843)
and uses an [ordered registry](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/registry.rs:34).

**Proposal:** one typed event schema, with optional metric/trace subscribers and
pre-registered metric handles. Keep tenant/session IDs on event fields, never
metric dimensions, following [Borg observability](../../../../constitution.md#iii-everything-is-observable).
Tradeoffs: generated schemas reduce drift but add tooling; borrowed event fields
are cheap for synchronous observers but asynchronous sinks require deliberate
ownership. The fork's unrestricted dimension support is broader than Borg allows.

The metrics [Backend interface](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/backend.rs:484)
uses native values and borrowed histograms. Its
[tuple composition](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/backend.rs:554)
feeds several exporters from one
[destructive report pass](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/registry.rs:688).

**Proposal:** fan out a single observed metrics snapshot; do not independently
drain counters per exporter. Separate recording, aggregation, export, and their
scheduling so simulation can drive them. Keep buffers bounded and make export
failures observable. In the reference, callbacks run under the registry lock:
they must not re-enter it or perform slow I/O. Its
[absorb operation](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/registry.rs:714)
compacts buffered pages without draining counters, showing why aggregation and
export cadence are separate. Start with a simple deterministic recorder; adopt
the per-CPU/assembly techniques only if profiling justifies their complexity.

Metrics runtime instrumentation is itself
[Tokio-specific](../../../../../s2n-quic/dc/s2n-quic-dc-metrics/src/runtime.rs:22).
That is a reason to isolate the adapter, not evidence of a runtime-free crate.

## Buffer Ownership And Zero-Copy

DC's [ByteVec storage bridge](../../../../../s2n-quic/dc/s2n-quic-dc/src/byte_vec/mod.rs:332)
makes the ownership distinction explicit: `put_bytes` transfers an owned chunk;
`put_slice` copies a borrowed slice. Its
[chunk reader](../../../../../s2n-quic/dc/s2n-quic-dc/src/byte_vec/mod.rs:356)
returns an owned slice of the head, and the
[builder freezes segments](../../../../../s2n-quic/dc/s2n-quic-dc/src/byte_vec/builder.rs:245).
The heavy buffer implementation lives in the external `etude-bytevec` crate, as
[the manifest records](../../../../../s2n-quic/dc/s2n-quic-dc/Cargo.toml:57).

**Proposal:** preserve immutable owned chunks across suitable transcript, tool
output, and transport boundaries, copying only where ownership or transformation
requires it. Do not call this universally zero-copy: encryption, compression,
serialization, borrowed inputs, and kernel I/O may still copy or allocate.
For Borg, shared ownership also needs tenant isolation and plaintext lifetime
controls from [encryption and blobs](../../../core/encryption-and-blobs.md).

The reference [explicitly weighs compaction against zero-copy](../../../../../s2n-quic/dc/s2n-quic-dc/src/stream/reader.rs:807):
many small chunks can retain too much allocation overhead. Its
[Owner/Handle accounting interface](../../../../../s2n-quic/dc/s2n-quic-dc/src/byte_vec/tagged.rs:70)
and [bounded destination test](../../../../../s2n-quic/dc/s2n-quic-dc/src/byte_vec/tests.rs:60)
are examples to study, not a certification of accounting correctness.

**Proposal:** distinguish logical bytes from physical retained allocation; test
clone, split, append, cancellation, and drop sequences for conservation and
bounds before adopting a tagged-buffer helper. Benchmark deliberately copying
or compacting long-lived small chunks instead of assuming sharing always wins.

## Decisions To Review Before Adoption

| Decision | Proposed next step | Remains unapproved |
|---|---|---|
| Bach integration or existing local harness | Make a small adapter experiment with the same component expectations | Library adoption and permanent runtime interfaces |
| Bolero-generated actions and shrinking | Compare a structured session action generator with the existing seeded queue test | Dependency and generator/test policy |
| Production executor | Design the minimum real adapter alongside its simulator | Tokio or another runtime, worker topology |
| Atomic concurrency verification | Add a concurrency model when the first actual concurrent primitive exists | Loom dependency and proof obligations |
| Event/metrics scheme | Draft one closed event family and derived metrics as a review example | Schema generation and exporter architecture |
| Shared chunk buffers | Measure representative transcript/output copies and retention | ByteVec/etude dependency and ownership format |

No recommendation here changes an operator decision. Direct execution concerns
who runs the model loop; it does not choose
Tokio, Bach, a store, an event generator, or a buffer library.
