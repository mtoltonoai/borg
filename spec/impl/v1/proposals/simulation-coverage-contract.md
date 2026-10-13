# Phased Durable Delivery And Simulation Coverage Proposal

Status: UNAPPROVED milestone, decisions, scope and measurement details. The
[phased process](../implementation.md#phased-delivery-and-coverage) and
[seven-stage structure](../implementation-phases.md) are approved; their
approval does not adopt the details below.
Runtime work remains paused. This proposal selects no store, adapter, version,
format, cryptography, runtime, exception, installation or deployment.

The previous semantic-obligation proposal is preserved
[verbatim as unapproved history](archive/simulation-coverage-contract-semantic-draft.md).
It is not the main deliverable or a prerequisite framework to build first.

## Earliest Durable Milestone: One Persistent Event-To-State Flow

Proposed first outcome: publish one synthetic tenant event, process it through
the session and notification hook, commit its state/cursor/decision event, kill
the process, reopen real persistent storage in a fresh process, and observe the
same committed result without applying a duplicate publish twice.

This crosses intake/inbox, owner fencing, session processing, notification
governance, stored-form sealing and an actual persistent-store adapter. It
deliberately avoids making topic join boundaries, tool retry/accounting policy,
model scheduling or transcript compaction decisions just to demonstrate the
first durable flow. Existing experiments provide tests to reuse; they do not
implicitly approve durable schemas, authority fixtures or production interfaces.

Acceptance proposed for this first milestone:

1. Persist the inbox and doorbell atomically on the selected real substrate.
   Seal tenant content before the store write under a reviewed stored form;
   synthetic payloads do not justify adopting today's plaintext fixture codec.
2. Load the committed input under its owner fence. A bounded notification hook
   produces its declared result. Commit state, cursor and required decision/event
   effects atomically. A refusal or failed commit leaves no partial effects.
3. End the owning process without relying on a graceful flush; also restart the
   store process if the selected adapter has one. A fresh process recovers from
   the persistent artifact alone, with no surviving SimStore, parent-memory
   checkpoint, or replay of setup actions supplying the asserted state.
4. After acknowledged commit, recovery preserves the committed state and cursor.
   For an applied commit whose reply was lost, recovery establishes the outcome
   from stored evidence; retrying the same event causes no second application.
   Before-commit failure leaves the prior state. An uncertain result is not
   classified as definite non-commit without evidence.
5. A replacement owner advances the required generations before recovery work.
   A superseded owner regaining store access cannot commit. If the test replaces
   a cluster, apply the original conditional epoch and restore-order rules too.
6. Run the applicable transaction, fence, ambiguity and recovery expectations
   against the simulator and the actual adapter/probe. Keep process-crash,
   storage-loss and full-cluster evidence separately identified. Verify tenant
   separation with two synthetic tenants in the applicable tests.
7. Retain exact requirement/code/test links, actual Duvet results, process exits,
   persistent-artifact identity, input/tool/binary digests and reproduction
   commands. Record measured line/branch coverage when available; unavailable
   Bach/branch measurement is an explicit gap, never an invented percentage.
   Essential correctness/safety checks above are acceptance requirements even
   while numeric coverage is incomplete.

The first milestone proves only the declared and exercised persistence boundary.
A process restart with storage retained does NOT prove survival of losing its
host, its disk or every store node. The selected substrate still needs a credible
path to the original self-durable guarantee; those untested dimensions remain
missing requirements, not exceptions. The store's own log/snapshot/index and
backup/restore are foundational requirements, not optional deployment polish.

## Delivery Order And Phase Acceptance

| Stage | Proposed deliverable and gate |
|---|---|
| Before coding this slice | Approve the minimal decisions below and the exact flow's state/commit boundaries. Qualify the proposed adapter contract and stored form. Keep other unresolved audit items outside this slice; correct any original-mandated behavior it uses. |
| First durable milestone | The one persistent event-to-state flow above with restart, dedupe, fence and ambiguity checks. It is progress within the staged implementation, not completion of every component or a complete Borg agent. |
| Broader capability integration (detail proposed) | Working end-to-end integration for every applicable component, including direct model/tool execution, authority, events/timers, blobs/transcripts, usage/observation and the applicable recovery/operational paths. Each has a concrete flow, persistence boundary and essential safety/correctness evidence. Track missing components explicitly; the unapproved 25-capability graph is not silently adopted as the authoritative list. |
| Subsequent phases | Broaden required behaviors, failure classes, recovery boundaries and generated scenarios; close measured line/branch and Duvet gaps. Include genuine full-cluster durability, backup/restore/drain and real-dependency parity at their applicable gates. No intermediate percentage is proposed. |
| Final coverage target | Separately reach 100% executable lines AND 100% instrumented branches of owned logic run under Bach, within directly approved scope/exceptions. Retain original behavior, probe and trusted-promotion gates; code coverage is not full conformance. |

Integrate a small working flow before perfecting every isolated component.
Parallel work can resume only when authorized, prerequisites and ownership are
settled, and the slice's design choices have direct approval. No worker or runtime
implementation is started by this proposal.

## Minimal Decisions Before The First Durable Flow

All recommendations here are UNAPPROVED.

| Decision | Concrete alternatives | Recommendation and boundary |
|---|---|---|
| Persistent substrate and adapter | Adapt an existing transactional engine with its own recovery mechanism; or build/own the required log, snapshot, index and recovery mechanism. | Qualify an existing engine first to reduce new durability work. No product has been selected or qualified. A named candidate still needs proof of atomic multi-key effects, chosen read validation, fences, version/watch semantics, ambiguity handling and committed-state recovery; a plain file dump of SimStore is insufficient. Return a candidate-specific adapter proposal before coding, not an implicit production-store choice. |
| Operator/test survival boundary | An isolated persistent test directory on this host, retained across process death; or an explicitly authorized persistent service/resource with a broader failure boundary. | Begin qualification with the local process-restart boundary, using only synthetic data and a directly approved path/lifecycle. Require a documented path to all-node survival and later proof; local-disk persistence alone is not that proof. No resource, credentials or other-host access is authorized here. |
| Durable records and sealing | Define the minimum conforming tagged records/envelope for this session flow now; or restrict the preceding experiment to a generic storage probe that writes no session content. | Review the minimal conforming records and sealing/key-lifecycle design before the durable session flow. A generic probe can qualify storage sooner but is not the first end-to-end milestone. Library-backed encoding/crypto choices and test-key handling remain explicit decisions; fixture codecs cannot become durable formats by default. |
| Concrete integration boundary | Route this flow's state/cursor/event effects through one reviewed atomic adapter transaction; or retain separate component stores and stop at independent adapter probes until atomic composition is designed. | Review the smallest concrete shared-commit seam for this flow. Separate stores cannot be called an end-to-end atomic session transition without a proved protocol. This recommends no universal storage/runtime abstraction and approves no current key layout or permanent API. |

The first two choices establish what persistence can actually be tested. The
third is necessary before real session records; the fourth makes the
cross-component atomicity reviewable. Topic membership, full tool-dispatch
semantics, model providers and other audit choices need review when their slices
enter the flow, not as blockers to this narrower first slice. Selecting direct
execution has not selected a runtime or dependency.

## Proposed Measurement Scope And Gate

Target, per approved component and build/feature/target configuration:

- `lines.covered == lines.count` for LLVM's mapped executable lines, including
  uncalled owned functions present in the coverage map.
- `branches.covered == branches.count` for instrumented branch outcomes.
  Branchless uncalled code can evade the branch metric, so both checks are
  necessary. A genuine zero-branch component reports branch coverage as not
  applicable, not 100%; an absent/unsupported branch report is unknown.
- Publish integer covered/total/uncovered counts and missing locations for both,
  per component and file. Do not gate on rounded displayed percentages or allow
  one component's hits to compensate for another's omissions.

Review explicit owned source files/ranges, build configurations and exclusions.
Inventory source absent from the compiled mapping; unbuilt modules, stripped or
uninstantiated code and conditional configurations are not silently covered.
Third-party internals, test-harness code and non-runtime generated/build code
are proposed scope exclusions, NOT adopted exceptions. Owned error/recovery code
stays in scope. Any deliberate exclusion identifies its exact source/boundary,
rationale and direct approval; compiler gaps are reported as measurement limits,
not approved exclusions. Inline test code requires explicit treatment.

Collect a fresh, isolated profile set from an allowlist of reviewed Bach test
entrypoints only. Do not merge ordinary unit tests, real probes or old profiles.
A test name/filter alone does not prove execution under Bach: owned logic invoked
by setup or teardown outside Bach contaminates that claim. Validate the entrypoint
and invocation boundary before accepting a simulation-only result. If it cannot
be isolated, report the gap rather than proposing new instrumentation implicitly.
Reject zero-test selections, ignored/failed tests, timeouts, truncated profiles,
source mismatches and incomplete coverage mappings as successful gate runs.

The following is a command template for the future approved toolchain and Bach
harness, NOT executed and NOT runnable against today's missing dependencies.
The variables mean a selected nightly, a fresh isolated target/profile directory,
an existing reviewed Bach-only test target/case, and an output file. No version
or test target is chosen here:

```sh
CARGO_LLVM_COV_TARGET_DIR="$BORG_COV_RUN/build" \
cargo "+$BORG_COV_TOOLCHAIN" llvm-cov --branch \
  --no-default-ignore-filename-regex --no-cfg-coverage --no-cfg-coverage-nightly \
  --locked --offline --package borg-sim --test "$BORG_BACH_TEST_TARGET" \
  --json --output-path "$BORG_COV_RUN/coverage.json" -- "$BORG_BACH_CASE" --exact
```

Use the exported JSON file summaries to aggregate each approved component's
explicit source set and compare the integer line and branch counts above.
Missing files/branch fields fail validation, not default to zero. Validate map
completeness and profile provenance before this arithmetic. Preserve full JSON
for missing locations and the exact invocation/executable hashes. The small
per-component JSON gate is proposed, not implemented; current
`development_check.py` hashes inputs and collects per-test LLVM lines but has no
such branch gate. Current official CLI documents line thresholds, but no
`--fail-under-branches`; this proposal does not invent that flag or substitute
region coverage for branches.

## Short Feasibility Notes

Read-only checks on 2026-10-11:

- Active rustc is stable 1.99.0, commit b940084d7, LLVM 23.1.1. Toolchains are
  stable and 1.97.1, with no nightly. Matching llvm-cov/llvm-profdata are installed
  (23.1.1-rust-1.99.0-stable). `rustc -Z help` rejects nightly-only flags.
- `cargo llvm-cov --version` reports no such command. No installation or
  toolchain change was made. Bach/Bolero are selected but absent from Borg's
  manifests/source; existing tests are not Bach measurements.
- [cargo-llvm-cov](https://github.com/taiki-e/cargo-llvm-cov) supports the proposed
  collection/export workflow; branch mode is unstable/nightly.
  Its [official CLI](https://raw.githubusercontent.com/taiki-e/cargo-llvm-cov/main/docs/cargo-llvm-cov.txt)
  documents the flags above. Its
  [report CLI](https://raw.githubusercontent.com/taiki-e/cargo-llvm-cov/main/docs/cargo-llvm-cov-report.txt)
  has line/function/region thresholds, not a branch threshold.
- [Rust's upstream limitations](https://github.com/rust-lang/rust/issues/124118)
  report instrumentation for if/while (including if-let/let-chains), let-else,
  short-circuit boolean branches and match guards. Individual match arms and
  or-patterns, `?`, await and macro-introduced branches remain reported gaps.
  The final operand of a standalone boolean chain is not itself a branch;
  [condition instrumentation](https://doc.rust-lang.org/nightly/unstable-book/compiler-flags/coverage-options.html)
  is a separate option, not selected here. Validate behavior of the eventual
  pinned nightly; these upstream claims are not a local version-specific test.
- [Rust coverage documentation](https://doc.rust-lang.org/rustc/instrument-coverage.html)
  defines executable-line mapping and warns about optimized-out functions and
  compatible LLVM versions. Compiler-mapped branches do not represent every
  semantic outcome, schedule, fault combination or path.
- Existing raw checkpoint
  [run-_pijyq0s](../../../../../borg-runtime/artifacts/conformance/run-_pijyq0s/duvet-metrics.json)
  records Duvet 0.4.3: 134/1,817 pairs; 1,683 missing implementation and test links
  in overlapping columns. Its separate local evidence executed 105 cited tests.
  Current dispatch.rs differs from that checkpoint. None of those numbers is
  Bach line or branch coverage; no new runtime test or Duvet development run was
  performed for this proposal.

## Original Requirements Remain The Acceptance Basis

- [P5](../../../../constitution.md#p5-durable-state-disposable-memory), lines 110-114:
  session state belongs in durable storage; disposable node memory is rebuilt.
- [Durable Store Durability](../../../core/shared-framework.md#durable-store-durability),
  lines 156-160, and [self-durable](../../../decisions/durability/self-durable.md),
  lines 15-17, 23-29 and 39: real log/snapshot/index, all-node survival without
  committed loss, backup/restore and release drills.
- [Session commits and fences](../../../core/sessions-and-partitions.md#ownership-is-fenced-by-generation),
  lines 204-214 and 259-264, and
  [failure recovery](../../../core/failure-handling.md#a-store-conflict-or-outage),
  lines 126-130 and 166-170: atomic state/cursor changes, generation fencing,
  store failures and recovery from committed cursors.
- [Notification decision atomicity](../../../core/governance.md#every-decision-is-an-event):
  hook decisions/events and their transition commit under the partition fence.
- [Stored form](../../../contracts/stored-form.md), lines 23, 50-60, 106-108 and
  114-134: client sealing, authenticated bindings and compatible tagged records
  apply before calling persisted session data conforming.
- [Durability and deployment](../../../core/durability-and-deployment.md), lines
  42, 52-58 and 88-106: lasting resources, conditional cluster epochs, ordered
  restoration of authority/emergency controls and preserved IDs/dedupe.
- [Simulation and conformance](../../../core/simulation-and-conformance.md),
  lines 29-37, 43-99 and 123-166: real-probe parity, required fault/recovery
  scenarios, generated model checks, discriminating tests, Duvet identities and
  execution evidence. Line/branch targets supplement these.
- [Evidence record](../../../contracts/evidence-record.md), lines 21-26 and 32-62:
  local writable artifacts cannot establish trusted promotion evidence.

No intermediate percentage, exclusion, vendor, version or architecture is
approved by this proposal. The next concrete review is the first durable flow
and its four decisions, not a bespoke transition-metrics framework.
