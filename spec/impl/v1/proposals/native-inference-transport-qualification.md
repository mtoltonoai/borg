# Native Inference Transport Qualification

Status: selected for private qualification and recorded in the [implementation specification](../implementation.md#externally-supplied-technologies) and [decision record](../decisions.md#native-inference-transport-qualification). The research receipts below precede acquisition; execution results are recorded separately. This selection grants no production adoption or provider-call budget.

## Decision

Qualify the maintained Reqwest HTTP client with Rustls, HTTP/1 and HTTP/2, and bounded JSON handling so the model transport can execute in Rust. This addresses the [runtime language requirement](../implementation.md#runtime-language) without carrying the Python demonstration adapter into the production runtime. Python can remain qualification supervision. Provider HTTP and credentials belong in the inference-service adapter, not Borg's core; this library choice does not move the credential boundary.

The recommendation is the exact available profile below. The alternative is assembling Hyper directly, which uses much of the same transport stack while leaving more connection, proxy and error handling to Borg. This is a maintenance tradeoff, not a measured size, performance or production-capacity result.

## Selected Scope

| Input | Qualification profile |
| --- | --- |
| Reqwest | 0.13.5, default features disabled, `rustls,http2` |
| serde_json | 1.0.151, default features disabled, `std,raw_value` |
| Tokio | Existing approved 1.53.2, defaults disabled, `rt,net,time,sync,io-util` |
| External package set | 162 exact identities and registry checksums in the linked locked-package records |
| TLS provider | Resolved Rustls 0.23.45, aws-lc-rs 1.18.1 and aws-lc-sys 0.45.0; platform verifier 0.7.1 |
| Build-tool profile | Existing GCC/G++ 13.3.0, GNU Binutils 2.42, glibc 2.39 development/runtime files and Linux 6.8 userspace headers; CMake 3.28.3, GNU Make 4.3 and pkgconf 1.8.1 if the inspected native build requires them. Exact installed package revisions and observed tools are recorded below; no system-package install or upgrade. |
| Proposed consumption site | `qualification/inference/native`, an isolated Rust transport qualification crate; no replacement of the existing native-runtime lock |

The selected action is acquisition of the exact package set, verification of downloaded checksums and actual target/build features, followed by bounded local HTTP/TLS and parser qualification. It includes the external TLS provider's native build tools; the Rust-language rule does not require rewriting external dependencies in Rust. It selects no Borg content-encryption algorithm, production TLS policy, pool topology, provider capacity, frontend framework or deployment.

The packet qualifies HTTP/1 and HTTP/2 up front. HTTP/2 acceptance requires actual negotiation and simultaneous streams on one connection, not merely an enabled feature; an HTTP/1 result cannot satisfy that check. Reusable clients and bounded active admission are required by the existing scale work; an idle-pool limit is not an active-request limit. This qualification does not establish the simultaneous ten-host target or select an endpoint fallback policy.

Existing Bach/Bolero qualification scope remains separate. This lock does not silently authorize additional packages or features when the native adapter is composed with other crates. Actual resolution outside the recorded sets stops for review before consuming the difference.

## Exact Evidence And Availability

- [Metadata-only H1/H2 resolution and limitations](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/README.md).
- [Exact Cargo lock](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/Cargo.lock), SHA256 `31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff`.
- [All package identities, checksums and source metadata](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json).
- [Source-derived Linux feature projection](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/source-derived-host-features.json).
- [Resolution receipt](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/receipt.json), SHA256 `7a9fc0af2f30bedee344fc0d5d1cdb20c41a04ac739c64fd6c9a4da50a543429`.
- [Installed build-tool identities](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/root-transport-build-tools.json).
- [Installed package and compiler-subprocess observations](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/native-http-review-build-d0q9kyni/installed-toolchain-observation.json).
- [Independent H2 delta review](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/native-http2-delta-review-7gobcgok/receipt.json), SHA256 `12b57a5e81d9aa757a6538144f524aa5b1edf4e18240fb74276c2ce5c98520c7`.

Online resolution and independent offline regeneration produced identical locks. All 162 expected checksums match the configured registry metadata. No source archive was downloaded, and root-only Cargo metadata is not proof of the complete materialized build graph. The source-derived Linux projection contains 98 packages in 99 inferred target/build contexts. That analysis does not determine actual proc-macro host-unit placement or build-script effects; it is not an executed Cargo unit graph. Other locked targets are not claimed as Linux build inputs.

Compared with the approved native 83-package bundle, 45 identities are identical and 117 are new. The 38 old-only identities are absent because this standalone candidate does not include the PostgreSQL client root; it does not remove or replace those approved dependencies. Actual combined dependency resolution requires its own exact comparison. The H2 profile adds seven identities to the preserved 155-package H1 profile without changing any of those earlier versions/checksums.

The build-tool observations are not an exhaustive compiler/sysroot manifest. After authorized archive acquisition, inspect the exact build scripts and bind the actual compiler subprocesses, assembler/linker, headers, startup objects and runtime libraries used from the recorded installed bundle. Do not silently select fallback downloads, FIPS, bindgen, Go, external system-crypto libraries or another toolchain. A material input or feature outside this proposed scope stops for a concrete comparison before use.

The initial serde_json 1.0.154 proposal could not resolve through the unchanged configured registry, which currently supplies versions through 1.0.151. The failed attempt is preserved. This packet explicitly proposes 1.0.151 rather than silently changing the original request or bypassing registry configuration.

Version 1.0.151 has a known JSON `Value` versus `f32` infinity comparison defect corrected in 1.0.152. The adapter cannot use that comparison path to validate numeric bounds. Qualification includes the published extreme-number counterexamples, exact integer/count validation, malformed UTF-8 rejection and bounded raw-value preservation. No claim is made that the later fix exists in this version. Prefer the fixed release when it becomes available through the existing registry; this proposal does not authorize an unrecorded upgrade. [Maintainer's 1.0.152 changes](https://github.com/serde-rs/json/releases/tag/v1.0.152), [comparison correction](https://github.com/serde-rs/json/pull/1347/files).

## Qualification Before Integration

The owned request, stream and outcome logic remains individually testable under Bach with injected transport. Real HTTP/TLS expectations are checked against the actual dependency. Testing a substitute does not establish behavior of Reqwest, Rustls, the certificate verifier or AWS-LC internals.

The concrete qualification cases are:

- Bounded incremental response reads: absent or false length, oversized chunks/headers/errors, deep JSON and slow consumers. No collect-then-check substitute for enforcing the body bound.
- JSON and streaming-event framing: split UTF-8, split event/data lines, multiline data, comments, partial final frames, tool argument deltas, opaque provider-state preservation and explicit completion/incomplete endings. Socket EOF alone is not a successful model result.
- Cancellation and unknown outcomes: cancel before send, during connection, after headers and between frames; observe local teardown without claiming provider computation or billing stopped. Another admitted call remains independent.
- Retry and origin rules: explicitly disable hidden request retries and redirects; distinguish unsent, uncertain and completed outcomes. A client idempotency header is not proof of server-side deduplication.
- TLS and secrecy: valid and invalid names/roots, expiration, proxy refusal and changed-origin attempts using synthetic fixtures. Keys stay out of payloads, logs, errors, model context and durable records. Test fake-secret echoes without reading real credentials.
- H2 multiplexing and recovery: observe ALPN and two real streams on one connection, obey peer stream limits, test flow control and cancellation isolation, and distinguish refused from possibly processed streams during reset, GOAWAY and connection loss. Proxy CONNECT and origin ALPN are distinct observations.
- Resource and lifecycle checks: reuse the client, enforce active admission separately from idle pooling, observe bounded buffers/connections and clean shutdown. Smaller fixtures qualify the intended implementation; they are not the full scale acceptance test.

Reqwest exposes explicit [retry disabling](https://docs.rs/reqwest/0.13.5/reqwest/retry/fn.never.html) and [timeouts, redirect, certificate and connection controls](https://docs.rs/reqwest/0.13.5/reqwest/struct.ClientBuilder.html). These APIs do not establish that Borg has configured or qualified them correctly. AWS-LC's [platform/build guidance](https://aws.github.io/aws-lc-rs/platform_support.html) informs the build profile; actual build-script tools and features are checked after authorized acquisition and before execution.

No real provider request is included in this proposed batch. A later live test uses a separate bounded authorization tied to the actual adapter and request inputs; the previous exhausted allowance is not reset or reused. No production dependency adoption, access change, deployment, original gate-baseline change or runtime target repin is implied.
