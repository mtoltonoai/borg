# Native Inbound Service Qualification

Status: the exact private technology qualification is selected in the [technology inventory](../technology-inventory.md#native-inbound-service-qualification-bundle). Production transport, listener and protocol decisions remain separate. No qualification success is inferred from selection.

## Recommendation

Qualify the maintained Hyper server for HTTP/1 and HTTP/2, with hyper-util connection and graceful-shutdown support, using the already selected Tokio/Rustls transport inputs. This gives the frontend and governed tool services a realistic native ingress candidate without promoting the small synthetic peer into a production server or maintaining our own HTTP parser.

The narrow request is the server feature extension below and one new supplied package, httpdate1.0.3. This is private acquisition/build and loopback-only synthetic qualification at qualification/inbound-service, not a production listener, protocol, routing, deployment or authentication-format selection. No real credential, provider call or external traffic is included.

## Exact Supplied Inputs

| Input | Qualification scope |
| --- | --- |
| Hyper1.11.1 | Existing package identity; enable server in addition to the existing HTTP/1 and HTTP/2 capabilities. Keep unrelated optional features disabled. |
| hyper-util0.1.21 | Existing package identity; server, server-auto, server-graceful and tokio support. The transitive HTTP/1 and HTTP/2 feature paths are recorded in the resolved profile. No proxy, server framework or service middleware bundle is implied. |
| httpdate1.0.3 | New package, archive SHA256 df3b46402a9d5adb4c86a0cf463f42e19994e3ee891101b1841f30a545cb49a9. Its official manifest declares no normal dependencies. |
| TLS and runtime | Reuse tokio-rustls0.26.6, Rustls0.23.45 with the selected AWS-LC provider, Tokio1.53.2 and the already selected native build inputs. No alternate TLS provider, system-crypto fallback, multithread feature extension or toolchain upgrade is requested. |
| Bodies and types | Reuse the existing exact HTTP/body/bytes dependency identities and their selected profiles. Owned admission and service behavior remains Rust; system Python3.12.3 may supervise the disposable fixture. |

The allowed input set is the [existing 162-package transport inventory](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json), bound by lock SHA256 31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff, plus the exact httpdate package above and the stated server feature changes. This is a closed candidate input set, not a claim that all entries compile or that the new graph has already resolved. After approval, record and verify the exact candidate lock, selected-target features, actual compiler/build inputs and source topology before package execution. Any other supplied identity, version or feature is outside this request. Existing client, core and store locks remain unchanged.

## Why This Candidate

The existing h2 0.4.19 server API and tokio-rustls TlsAcceptor could provide an HTTP/2-only alternative without a new package. That alternative needs more owned stream, flow-control and connection-lifecycle code and supplies no HTTP/1 compatibility. No identified client inventory justifies restricting the eventual frontend to HTTP/2 merely to avoid a technology review. Qualifying both protocols preserves options; it does not adopt a public protocol policy.

An Axum/Tower application layer is not proposed: it adds technology without resolving the below-handler parser behavior identified below. We can first measure the smaller connection layer against the actual Borg boundaries.

## Concrete Qualification And Known Gaps

1. Run bounded simultaneous HTTP/1 connections and HTTP/2 streams with explicit connection, header, body, in-flight and output-buffer bounds. Observe backpressure, slow consumers, reset storms and cleanup. Report actual active work, memory and retained objects; queued identifiers or one-host protocol tests do not establish the [ten-host agent/workspace target](../implementation.md#simultaneous-production-scale).
2. Authenticate and apply the relevant control/tool gates before method execution. Observe handler non-entry on refusal. For uploads and streamed assertions, observe application body reads separately from transport pre-read buffers and test headers/body arriving together. A body wrapper alone is not proof of the [before-body checks](../../../core/the-frontend.md#an-upload-is-checked-before-its-body-is-read) or the [streamed-call rule](../../../contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match).
3. Reset or disconnect a client while a durable operation is unresolved. Hyper may drop its pending service future. The actual application owner must retain and settle the in-flight operation; cancellation or disconnection cannot imply rollback, safe resend or an unrecorded effect. Exercise the existing [atomic receipt/effect obligation](../../../contracts/tool-call.md#a-service-dedupes-by-call-id) through the qualified store/dispatcher interfaces, with no second business implementation.
4. Exercise unknown routes/methods, wrong media/shape, oversized or unparsable bodies and configured caps. Observe the actual wire reply, stable refusal class, request/trace identity and refusal event. Test malformed request lines/headers and oversized headers separately, before any application handler has run.
5. Keep [emergency controls available during a platform drain](../../../core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list). Do not equate shutting down all HTTP connections with the platform's ordered drain. Verify bounded tail buffering, slow-viewer detachment and independent progress of unrelated admitted work.
6. Bind exact source, selected package/build graphs, commands, executables, observations and cleanup. Exercise owned handler/admission logic with Bach and the same expectations against real transport faults. Hyper, TLS and kernel internals are real external dependencies, not claimed Bach-simulated code.

Source reading identifies a real qualification risk: Hyper's HTTP/1 parser can emit bare 400, 431 or 414 responses before invoking the application service. Under a literal reading, those responses do not satisfy Borg's [traceable replies](../../../core/the-frontend.md#replies-are-legible-and-traceable) and [uniform pre-route refusals](../../../core/the-frontend.md#every-refusal-has-one-shape). No malformed-framing exception was found. Whether a protocol framing failure lies outside the API request/reply domain remains unresolved; this packet adopts no exception. Measure the actual wire/trace/audit behavior, identify a supported integration solution or bring the precise source question for review. Do not call a bare parser response conforming or introduce an owned HTTP parser to conceal the gap. HTTP/2 resets and transport pre-read/body boundaries require the same honest distinction.

## Sources And Approval Boundary

[Research packet](/home/dev-user/borg-runtime/artifacts/execution/inbound-service-research-1d8f83v9/root-packet.json), SHA256 282a5c2988ed9b7a65da2a0a5ec721e82e114806bc0b1568752735d7ca24e567, binds 20 exact source requirement tuples, the materialized manifests and inspected primary source. [Official httpdate index](https://raw.githubusercontent.com/rust-lang/crates.io-index/master/ht/tp/httpdate) and [manifest](https://docs.rs/crate/httpdate/1.0.3/source/Cargo.toml) supply the new package metadata. Version-specific rendered API pages for the selected Hyper, hyper-util and h2 versions were not retrievable in this review; their API findings are based on the exact materialized primary source bound in the research packet. Source inspection is not execution evidence.

The recorded selection permits only the exact private qualification inputs and isolated consumption site. Production transport adoption, source-contract interpretations, route/encoding policy, authority issuers, operational ports, infrastructure and ten-host deployment remain separate. Shared requirements, other approval scopes and historical evidence are unchanged.
