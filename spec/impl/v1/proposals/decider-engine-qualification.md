# Private Decider Engine Qualification

Status: selected for bounded private qualification. The exact reviewed packet is frozen in [the adoption before-state](/home/dev-user/borg-runtime/artifacts/execution/five-decisions-adoption-n0k5tii9/before/proposals/decider-engine-qualification.md). The selection and guards are normative in [the implementation specification](../implementation.md#decider-engine-qualification). The historical recommendation below does not claim acquisition, build, production selection or full sandbox implementation.

## Recommendation

Qualify Wasmi 2.0.0 as the private execution boundary for sandboxed deciders.
It is an interpreter with validation, fuel and stack controls. Compare it with
Wasmtime 49.0.2, which provides compiled execution and epoch interruption but
has a larger runtime/compiler dependency set. The small interpreter profile is
the recommended first qualification; neither engine is a production selection.

The shared specification requires sandboxed deterministic programs, not a
particular program format. This proposal does not select the product's language,
compiler, public format, hosting topology or code generator. Required platform
checks still need their actual input and policy adapters. An engine cannot
replace those checks with supplied replies or an empty protected-check set.

## Exact Requested Scope

The proposed target is x86_64-unknown-linux-gnu. The dependency is Wasmi 2.0.0,
with defaults disabled and only stable, std, validate, portable-dispatch and
extra-checks enabled.

| Package | Version | Requested role |
| --- | --- | --- |
| wasmi | 2.0.0 | New interpreter |
| wasmi_collections | 2.0.0 | New engine support |
| wasmi_core | 2.0.0 | New engine support |
| wasmi_ir | 2.0.0 | New engine support |
| wasmparser | 0.228.0 | New validation dependency |
| spin | 0.9.9 | New synchronization dependency |
| libm | 0.2.16 | New unconditional engine dependency, even with guest floats disabled |
| bitflags | 2.13.2 | Exact already-approved native-bundle identity |

The [dependency packet](/home/dev-user/borg-runtime/artifacts/execution/decider-sandbox-dependency-scope-tlm78chy/dependency-scope.json),
SHA256 `9b7d2adc5e258e5f64647b2e2ad154a92c0581cbbc58e3bd99dbe9dcf341cb60`,
pins registry checksums, features, compiler requirements and license sources.
Cargo produced a 21-record lock and reproduced it offline. The eight active
identities above are a source-derived graph, not yet an archive-backed Cargo
feature result. The thirteen lock-only identities are not requested for
acquisition or execution. There is no broad fetch authorization in this proposal.

The requested approval covers two guarded steps at a new private
qualification/decider-sandbox consumption site:

1. Acquire only the eight checksum-pinned archives above into an isolated cache.
   Inspect packaged manifests, licenses and supplied build scripts. Wasmi,
   wasmparser and libm have build scripts; those are included in the requested
   profile, with the selected compiler explicitly bound. Run offline locked
   metadata checks before any build.
2. Only if the packaged graph, features and build inputs match this exact scope,
   build and run the bounded synthetic qualification below. Any additional
   archive, feature or build input stops the dependent work for a separate
   decision. An unexpected requirement for a lock-only archive is not permission
   to acquire it.

Reuse Rust/Cargo 1.99, explicit system Python 3.12.3 and the separately approved
Bach/Bolero test profiles. No compiler, native guest toolchain, CLI, WASI, WAT,
SIMD, component-model implementation or additional policy engine is requested.
No provider calls, public listeners, host access changes or deployment follow.

## Finite Qualification

Use validated integer-only private programs with an explicit minimal import
allowlist, no start function, no filesystem/network/ambient clock or entropy,
fresh evaluation state and bounded input/output copying. Include a nonconstant
program whose actual decisions change with its input, transformation and Block
reason cases, malformed module/import/output refusal, fuel exhaustion, recursion,
memory growth and cross-evaluation isolation.

Fuel is not elapsed time. The private runner must enforce its actual deadline
and clean up timed-out execution; an asynchronous timeout around uncancellable
synchronous work is insufficient. Memory accounting must cover host copies,
validation, caches and concurrency as well as guest memory. Test bounds are
recorded with the runner and do not become production capacity defaults.

Use Bach for the owned sequencing, decoding and failure behavior with a
substituted engine boundary, plus the same expectations against the real engine.
Do not claim the third-party engine's internals are simulated. Include a
deliberately divergent simulator case to check the parity test itself.

The first private profile is stateless. It does not remove the full state-slot
requirement or change the existing total per-session bound. Valid Block results
and valid gated state updates remain distinct from traps and malformed output.
No production authority, durable result association, complete usage accounting,
full-v1 sandbox support or conformance is inferred from engine tests alone.

## Sources And Remaining Work

The [source-backed qualification design](/home/dev-user/borg-runtime/artifacts/execution/decider-sandbox-prep-tlm78chy/qualification-proposal.md)
binds the actual governance/simulation requirements and the existing source
gaps. The [dependency review input](/home/dev-user/borg-runtime/artifacts/execution/decider-sandbox-dependency-scope-tlm78chy/review-input.md)
separates metadata facts, source inferences and unperformed acquisition/builds.
It also preserves an earlier Python-selector mismatch and its explicit
system-interpreter revalidation instead of relabeling the earlier command.

Primary references are the [Wasmi release](https://github.com/wasmi-labs/wasmi/releases/tag/v2.0.0),
[exact feature manifest](https://raw.githubusercontent.com/wasmi-labs/wasmi/v2.0.0/crates/wasmi/Cargo.toml),
[engine configuration](https://raw.githubusercontent.com/wasmi-labs/wasmi/v2.0.0/crates/wasmi/src/engine/config.rs),
and [Wasmtime comparison release](https://github.com/bytecodealliance/wasmtime/releases/tag/v49.0.2).

The selected scope is only the private acquisition and qualification described above. Production selection, additional consumption sites and any inputs outside the exact profile remain separate. The frozen packet preserves the wording presented for the decision.
