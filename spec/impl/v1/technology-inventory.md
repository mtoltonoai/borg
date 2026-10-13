# V1 Technology Inventory

This descriptive inventory records supplied technologies and consumption sites selected in the main specification, including the exact immutable-byte and C2 implementation targets. It does not independently add runtime behavior or expand any selected purpose, package, feature, deployment or qualification claim. Review metadata binds these bytes separately from the three normative declaration roles.

## Consumption Boundaries

| Technology | Consumption sites | Real implementation / simulation boundary |
|---|---|---|
| Rust/Cargo/standard library | Owned contract and integration crates; qualification crates | Build and language runtime; not claimed to run internally under Bach. |
| Python/standard library | Duvet Cap and qualification/integration runners | Actual harness/tooling execution; not component simulation. |
| LLVM/nightly | qualification/coverage | Actual compiler coverage instrumentation; dependencies are outside owned-component coverage. |
| Duvet0.4.3 | Native planning and runtime traceability runners | Actual installed binary; not a simulator or behavior judge. |
| Tokio1.53.2/tokio-postgres0.7.18 and pinned bundle | qualification/native-runtime; qualification/native-runtime/parity through its parent; integration/blob-point-store and its parity consumer through the parent; integration/collaboration-store; integration/event-delivery-view; integration/workspace-control-store; integration/dispatch-usage; integration/governed-reference-turn; integration/coordinator-store/native; integration/configuration-store/native; integration/authority-store; integration/session-store; integration/blob-native-receiver; qualification/assertion-signature/caller | Actual private native-runtime/store adapters using the same approved bundle; Bach tests owned logic through injected dependencies, not these libraries' internal runtime, timers or protocol implementation. Real-adapter parity remains to be qualified. |
| Reqwest0.13.5, Rustls/AWS-LC and serde_json1.0.151 bundle | qualification/inference/native, its isolated real-test manifest and qualification/inference/native/live-adapter | Actual private HTTP/1 and HTTP/2 client, TLS/provider/verifier and JSON parser. The separate Bach consumer compiles the shared owned collection algorithm without this transport dependency surface. Remaining owned adapter logic still needs Bach qualification; third-party internals are not claimed to run under Bach. |
| Bundled s2n-bignum, jitterentropy and Fiat sources | Inside the exact selected aws-lc-sys0.45.0 archive, consumed by the native inference TLS provider build | Supplied assembly and C internals; not owned Borg runtime or Bach-simulated behavior. These are recorded nested contents of the selected archive, not separate package/version selections or a Borg content cipher. |
| serde_json1.0.151, std only with defaults disabled | integration/collaboration-store | Primitive JSON escaping/encoding into the owned bounded counting sink and qualifier parsing, using the same approved package. No transport edge, production codec choice or claim that library internals run under Bach. |
| serde_json1.0.151, std/raw_value with defaults disabled | integration/governed-reference-turn through qualification/inference/native with parent defaults disabled | Reuse of the selected bounded inference mapping/types without the Reqwest/HTTP/TLS edge; real library parsing is not itself Bach simulation. |
| Installed native compiler/build bundle | qualification/inference/native dependency and real-test builds | GCC/G++13.3.0, Binutils2.42, glibc2.39 development/runtime files and Linux6.8 userspace headers; CMake3.28.3, Make4.3 and pkgconf1.8.1 if required by the inspected build. Exact package revisions and observed tools are recorded below; actual compiler/sysroot inputs are bound before build. |
| PostgreSQL16.15/libpq160015 | qualification/store, qualification/store-parity, integration/durable-session, integration/coordinator-store, integration/coordinator-store/native, integration/authority-store, integration/collaboration-store, integration/lifecycle-store, integration/configuration-store, integration/configuration-store/native, integration/transcript-store, integration/admission-store, integration/live-view-store, integration/observation-store, integration/timer-store, integration/turn-recovery, integration/dispatch-usage, integration/blob-store-metadata, integration/blob-point-store, integration/subscription-store, integration/event-delivery-view, integration/governed-reference-turn, integration/workspace-control-store, integration/session-store, integration/blob-native-receiver, integration/drain-store, integration/credential-dispatch, qualification/assertion-signature/caller | Real engine probes and assigned caller integrations; Bach executes owned models/components, not PostgreSQL internals. A recorded consumer site is not evidence that its tests have completed. |
| minicbor2.3.0 | qualification/stored-form | Actual borrowed decoder for qualification and the exact C2 target; vendor internals and source selection are not Bach or compatibility proof. |
| OpenAI Responses/gpt-6.1-sol | qualification/inference, integration/durable-session external provider adapter and qualification/inference/native/live-adapter under a finite call scope | Actual bounded service calls; provider internals are not simulated. |
| OpenBao2.7.1 release bundle | qualification/key-service server/CLI and API harness; built-in Transit, token/policy/audit, Raft storage and synthetic seal/key configuration | Selected real-service qualification. Vendor internals are not Bach simulation; no simulator-parity, custody or erasure exception is implied. |
| aws-lc-rs1.18.1/aws-lc-sys0.45.0 and OpenBao2.7.1 | qualification/assertion-signature Rust verifier and isolated signing-service fixture; qualification/assertion-signature/caller through the primitive | Direct private application-signature qualification, separately scoped from TLS and symmetric-key fixtures. Actual cryptographic/service internals are not Bach simulation. No production assertion format or parity claim follows. |
| aes-gcm0.11.1, aes-siv0.8.0, hkdf0.13.0, sha2 0.11.0, zeroize1.9.1 and pinned dependency inputs | contracts/envelope; integration/sealed-store; existing integration/durable-session consumer for the private envelope experiment | Real cryptographic libraries; owned envelope logic uses injected entropy and Bach cases. This does not establish simulation testing of vendor internals or a production cipher/format choice. |
| Existing minicbor2.3.0, OpenBao2.7.1, PostgreSQL16.15/libpq and selected native/Bach profiles | The same private envelope qualification and integration sites | Existing borrowed-codec, synthetic key-service and real-store fixtures are reused. Every actual combined dependency/feature graph remains separately verified; no new engine, SDK, service profile or simulation exception follows. |
| gVisor release20261005.0 official x86_64 distribution | qualification/workspace-execution, acquisition and static inventory only | No real sandbox, sidecar, shim or workload execution. Neither simulator parity nor an executable isolation/resource profile is established by archive inspection. |
| Hyper1.11.1, hyper-util0.1.21 server features and httpdate1.0.3 with the existing native transport bundle | qualification/inbound-service | Real private HTTP/1 and HTTP/2 server qualification. Owned request/admission behavior needs Bach and real-probe evidence; HTTP/TLS/kernel internals are not claimed to run under Bach. |

## Envelope Qualification Bundle

The selected roots have default features disabled: aes-gcm0.11.1 with aes/zeroize,
aes-siv0.8.0 with zeroize, hkdf0.13.0 without extra features, sha2 0.11.0 with
zeroize, and zeroize1.9.1 with alloc. The [pinned dependency metadata](/home/dev-user/borg-runtime/qualification/key-service/artifacts/next-envelope-proposal-20261012/candidate-dependencies.json),
SHA256 `01697f2e75ce20c1d5bfe73cd77b4a398fbb0115fd491eddbed9029fc32f40e9`,
records the 28 conservative supplied identities, archive checksums and requested
feature closure. It is not an executed Cargo resolution or compiled count.

The [envelope qualification packet](proposals/envelope-integration-qualification.md)
defines synthetic qualification at contracts/envelope and integration/sealed-store,
followed by the existing integration/durable-session consumer. The existing
Rust/Cargo, Bach/Bolero, Python3.12.3, minicbor, native-store and OpenBao fixture
profiles are reused. Actual package identities, features and build inputs are
checked against the recorded bundles before execution; no unexpected input is
silently added. Entropy remains explicitly injected rather than obtained from an
ambient library RNG.

This selects the technologies for the reviewed private experiment, not a
production cipher, hierarchy, durable encoding, custody arrangement or migration.
The later AWS-LC envelope-reuse comparison is not a replacement selection. No
backup-erasure requirement beyond the existing v1 exception is reintroduced.

## Workspace Sandbox Static Inventory

The selected input is the official `gvisor-x86_64.tar.bz2` distribution for
release20261005.0, source revision
`97e8e896904cb364319cd9758241d3f0bd27a6f4`, archive SHA256
`bd4eb2ca0ee74f0fbbb5f5273ba011e79c56a29fbb047393b88b981cd3230ede`.
The [workspace inspection packet](proposals/workspace-execution-qualification.md)
records the official release/checksum URLs and source-level feasibility limits.

The entire archive may be acquired and statically inventoried at
qualification/workspace-execution. Inventory includes exact member identities,
paths, modes, linked inputs and nested supplied components. Bundle membership
does not select each contained program for execution. In particular, runsc,
gvisor-bin sidecars, the containerd shim and the metric server remain unexecuted
under this scope. No container daemon, sandbox workload, new image, privilege,
host/network configuration or production deployment is selected. Native workspace
BUILD remains the existing product choice; its actual isolation and simultaneous
capacity proofs are still required.

## Native Inbound Service Qualification Bundle

The selected extension is Hyper1.11.1 server with the existing HTTP/1 and HTTP/2
features, hyper-util0.1.21 server/server-auto/server-graceful/tokio, and
httpdate1.0.3, archive SHA256
`df3b46402a9d5adb4c86a0cf463f42e19994e3ee891101b1841f30a545cb49a9`.
The available supplied set is the [existing transport package inventory](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json),
bound by [transport lock](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/Cargo.lock)
SHA256 `31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff`,
plus that exact httpdate identity and the stated server feature changes. The
containing 162-package inventory is not a claim that all entries compile. Actual
normal/development/build profiles are resolved and checked before execution.

The consumer is qualification/inbound-service. Private acquisition/build and
loopback-only synthetic qualification reuse Tokio1.53.2, tokio-rustls0.26.6,
Rustls0.23.45 with the selected AWS-LC provider, existing HTTP/body/bytes identities
and the existing native build tools. No Axum/Tower, proxy, alternate TLS provider,
system-crypto fallback, multithread feature extension or toolchain upgrade is
selected. System Python3.12.3 may supervise the disposable fixture.

The [inbound packet](proposals/native-inbound-service-qualification.md) retains
the actual parser/refusal, before-body admission, cancellation/settlement,
backpressure, drain and traceability probes. Bare protocol-parser responses are
not declared conforming by technology selection. Public protocol/routing policy,
authentication format, production listener, infrastructure and any source-contract
exception remain distinct. No real credential, provider call or external service
traffic is included in the qualification.

## Assertion-Signature Qualification Bundle

The selected consumer is qualification/assertion-signature. It reuses aws-lc-rs1.18.1 with aws-lc-sys0.45.0 and the materialized aws-lc-sys/prebuilt-nasm feature profile, plus the [existing native build bundle](#native-inference-transport-qualification-bundle). The containing [transport lock](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/Cargo.lock), SHA256 `31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff`, and its [exact package records](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json) bound the available supplied inputs. The actual verifier subset, features and build inputs are resolved and checked before execution; the containing inventory is not a compiled-package count or selection of additional features. No dependency upgrade, alternate provider, FIPS/bindgen route or system-crypto substitution is included.

The isolated service reuses the [OpenBao2.7.1 official release bundle](#openbao-qualification-bundle), archive SHA256 `0e2f1ce10d124e03112b50dd2fbec6b78003783253bc3a91587938f39d1e2243` and dependency SBOM SHA256 `96e64e2d8d81158c7e385fbb2d52cc31c5736b3782c2a583027e0804ed977c31`. Built-in Transit holds disposable non-derived plain Ed25519 private keys. The fixture's Rust verifier consumes only raw 32-byte public keys, 64-byte signatures and bounded exact message bytes; it rejects other representations and prehash modes. Service response decoding/version prefixes are measured adapter behavior, not a Borg token encoding. Existing system Python3.12.3 may supervise the fixture; owned verification and integration logic remains Rust.

The [qualification cases](proposals/assertion-signature-qualification.md#concrete-qualification-cases) cover service-independent local verification, altered/truncated/overlong inputs, wrong representations and key versions, disposable signing-only authority, service failure and cleanup. The selected use adds no production role, real tenant key, plugin, external custody service or persistent service configuration. No real credential or provider request is involved. Actual cryptographic and service code are not claimed to execute under Bach; owned framing/caller logic and simulator/real parity require their own later evidence. Production signature scheme, token/field encoding, publication/rotation integration and deployment remain unselected. Other pending technology and contract proposals are unaffected.

qualification/assertion-signature/caller composes the same private signature primitive, owned verification-key cache, actual admitted request projection and existing native blob receiver. Its [consumer graph](/home/dev-user/borg-runtime/qualification/assertion-signature/caller/artifacts/design-start-6oba8oh_/consumer-graph.json), SHA256 `a5d2d3838d95bb60b393cdc7b807210fff97c7724503eb708f339884a673ff6e`, records seven normal and nine development owned paths. Normal external consumption comes through the already selected AWS-LC, native-store and JSON profiles. Development uses the pinned Bach revision and existing Tokio/tokio-postgres features; no additional direct Bolero edge or profile is selected by this packet. The actual normal/development union and features are verified before package execution; no resolved or compiled count is asserted here.

This is a declared-statement private fixture: raw received signed bytes are verified using the actual selected cached public key, then bound to the actual admitted request and its complete receiver target, assignment, owner, pin and explicitly supplied principal mapping. The resulting private proof retains that exact request for receiver use; no unrelated command or grant set is accepted from signed bytes. The receiver still reads current grants through its existing native authority path and commits its receipt and blob effect in the same transaction. A request mismatch establishes zero receiver grant queries/effects; it does not erase earlier core checks. The [source outline](/home/dev-user/borg-runtime/qualification/assertion-signature/artifacts/caller-join-outline-hy34901o/handoff-v2.json) remains a historical design reference whose producer pins are rebound to current reviewed handoffs before execution. This records existing-profile consumption, not a general assertion encoding, issuer entitlement, production grant publisher, token lifetime, cryptographic exception or readiness result. Actual crypto, service and database internals remain outside Bach simulation; owned caller logic and real-probe parity need their own evidence.

## Historical Use Outside The Selection

On 2026-10-12, one recorded model-recovery test-formatting operation used the installed Rust1.97.1 bundle's rustfmt, SHA256 `30de9e1efcd8f8fe7750e00d0c45ff8f4c480608ef1be5baf9ab6f1b4556e8f8`, with that bundle's library path. That use was outside the selected Rust1.99 development profile and was not approved. The path was stopped, the user was notified, and the original command, loader failure, successful invocation and source diff remain recorded. This entry is disclosure of historical use, not a technology selection or retrospective approval.

The selected Rust1.99 bundle's formatter, SHA256 `a3ab6c445b26eda8d7bcfd5cc3f4b068dd7136af96822fd21378fc6fe8c2f2a3`, subsequently passed an isolated `--check --edition 2021` on the exact retained test source without changing it or the live source. [The scope and verification record](/home/dev-user/borg-runtime/artifacts/execution/formatter-scope-review-mddzvj_k/receipt.json), SHA256 `ee99ce22b6d5af163dd3480b51dc878c7d6360b99a665b088bdba85faf3a0b62`, states the checked inputs and limits; formatting success is not a runtime test result. Existing selected versions, allowed uses and approval requirements are unchanged.

## OpenBao Qualification Bundle

The selected input is the official `openbao_2.7.1_linux_amd64.tar.gz`, published SHA256 `0e2f1ce10d124e03112b50dd2fbec6b78003783253bc3a91587938f39d1e2243`. Its published dependency SBOM is SHA256 `96e64e2d8d81158c7e385fbb2d52cc31c5736b3782c2a583027e0804ed977c31`. These bind the packaged server/CLI and linked dependency bundle, not independent adoption of every upstream optional component. [Official artifact identities](https://github.com/openbao/openbao/releases/expanded_assets/v2.7.1).

The fixture explicitly uses bundled Raft storage, built-in Transit, token/ACL/audit functionality, Shamir sealing and aes256-gcm96 synthetic keys. No external plugin or HSM/KMS is selected. The existing system Python3.12.3 standard-library harness consumes the HTTP API. The acquired binary, SBOM, actual linked dependencies and effective configuration are recorded and checked before server launch; a mismatch or new external runtime dependency is not silently accepted. No artifact has been executed merely by recording this selection.

Ordinary recoverable service snapshots are not a Borg owner-erasure mechanism. Wrong-context checks demonstrate only the selected primitive's binding; Borg's tenant/owner/range envelope, trusted key selection, cached plaintext destruction and infrastructure-deployer separation need their actual implementation and evidence. The reviewed experiment includes deliberate recovery of pre-deletion synthetic snapshots so failures remain visible.

## Native Runtime Qualification Bundle

The selected private bundle is Tokio1.53.2 with default features disabled and
rt/net/time/sync/io-util, plus tokio-postgres0.7.18 with default features disabled
and runtime. Its [83 exact external package identities and checksums](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/dependencies.md)
are bound by the [generated lock](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/resolution/Cargo.lock),
SHA256 06e270111b7bdb78a654fce69dbf173b6577c3941906081c25557a666527431b.
The [resolved metadata](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/resolved-dependencies.json)
records every identity and its comparison with the existing test lock. The old
65-entry test inventory below retains its own scope; it is not relabeled as the
native-runtime closure.

The initial consumption site is qualification/native-runtime, with synthetic data and the
existing private Unix-socket PostgreSQL16.15 fixture. The root experiment requests
a current-thread builder with I/O and time drivers, without selecting production
threading. No ORM, pool, TLS/HTTP adapter, live provider call or frontend framework
is added. Protocol authentication hash dependencies do not select a Borg content
cipher or a production password policy.

The same exact root features and pinned 83-package bundle are also consumed by
integration/blob-point-store, integration/collaboration-store and
integration/event-delivery-view for bounded native caller qualification. Their
[consumer packet](/home/dev-user/borg-runtime/artifacts/execution/native-consumer-batch-9tm4470g/packet.json)
records the owned component dependencies and existing Bach/Bolero development
dependencies where used. Business logic and effect SQL execute in Rust; system
Python remains private fixture supervision. Consumers reuse the Runtime-owned
connection/query/watch lifecycle. Their synthetic table mappings do not select a
production schema, store topology, pool policy, wire format or cryptographic
profile. Normal and development feature closures are checked against their
respective approved scopes before execution.

The same native bundle and existing Bach/Bolero test closure are consumed by
integration/workspace-control-store for the bounded PublishSnapshotHead and
BeginRollback metadata commands in its
[interface packet](/home/dev-user/borg-runtime/integration/workspace-control-store/interface-packet.json).
It uses the existing workspace-control evaluator and native connection lifecycle,
with point facts, conditional commits and exact unknown-commit settlement. The
private synthetic mapping does not select a production format or create a new
storage primitive. Actual authority, lease/activity, sealed snapshot, host and
domain producers remain prerequisites; a persisted plan is not proof of a host
promise, rollback completion or full workspace readiness. Python only supervises
the existing private PostgreSQL fixture.

integration/dispatch-usage also consumes the same exact native bundle and owned
native-runtime library for its bounded Rust dispatcher, receipt and usage
transaction qualification. The
[native consumer packet](/home/dev-user/borg-runtime/integration/dispatch-usage/artifacts/producer-scope-578qy_53/native-consumer-proposal.json)
records the exact normal and existing Bach/Bolero development edges. This reuses
the selected connection lifecycle and owned transition logic; it does not select
a production transaction schema, receiver/service topology, codec or HTTP/TLS
transport. Real authority, executor, sealing and current-fact producers retain
their separate obligations. The unresolved stale-active-close semantics are not
selected by this registration. The private PostgreSQL fixture remains under
tooling supervision; owned runtime sequencing and effects are Rust.

integration/collaboration-store composes its bounded revision source and handler
with the existing Rust consumer in integration/event-delivery-view. The
[owned-edge packet](/home/dev-user/borg-runtime/integration/collaboration-store/artifacts/composed-consumer-q7hcslt7/start.json)
and [offline closure check](/home/dev-user/borg-runtime/integration/collaboration-store/artifacts/composed-consumer-q7hcslt7/closure.json)
record unchanged selected external identities/features, the four additional owned
packages and the borrowed integration/subscription-store/src/codec.rs input. The
source snapshot ends before the distinct read-committed consumer transaction.
This qualifies durable local effect intents, not external dispatch, initial cursor
policy, scope issuance or cross-service atomicity.

The same consumer directly imports the existing contracts/events and contracts/subscriptions type APIs for settled collaboration effects passed to the event append interface. These owned packages were already transitive inputs through integration/event-delivery-view. The [direct-edge packet](/home/dev-user/borg-runtime/integration/collaboration-store/artifacts/publication-joined-gf_rbkq2/registration-packet.json), SHA256 `88709f7d185cd9c665a4cced17ca9a409267fdbf59fbeb651753cc9083b1e258`, records no intended external identity or feature delta; actual resolution is checked before execution. This registration does not select new publisher authority, record scopes, membership semantics or deployment.

integration/governed-reference-turn consumes the native bundle for its addressed
Rust model/reply/X/usage transaction producer and existing governed dispatcher.
Its [consumer packet](/home/dev-user/borg-runtime/integration/governed-reference-turn/artifacts/native-durable-model/consumer-edges-fklfdo_0/packet.json)
records the owned native-runtime, inference types, dispatch, credential and registry
edges. The inference dependency has defaults disabled: it contributes the already
selected serde_json1.0.151 std/raw_value profile, not Reqwest, HTTP/2 or TLS. The
combined normal graph is checked against the native bundle plus that selected JSON
closure; the pinned Bach/Bolero development graph is checked separately before
execution. This records permitted reuse, not a claim that an unresolved combined
graph has passed. Local package additions have new actual lock hashes; the original
approved bundle records remain immutable. Actual authority, configuration/rendering,
sealed-format and committed-reply producers retain their separate obligations.
No provider call, production schema or source-contract clarification is adopted.

integration/coordinator-store/native consumes the same selected Tokio/tokio-postgres features and pinned Bach/Bolero development profile for a private conditional coordinator-claim and current-coordinator adapter. Its [consumer packet](/home/dev-user/borg-runtime/integration/coordinator-store/native/artifacts/design-start-4admis2y/consumer-packet.json), SHA256 `34af32e933283f2ca0002b9a40e20c5e07c11a48883c6337ddc9cf597d857cd9`, records the owned coordinator and native-runtime dependencies. The static normal inventory retains the existing 83 external identities, and the staged development lock retains the existing 128 external identities with three owned packages; these are not resolved or compiled feature counts. Actual normal and development graphs are checked before package execution. This consumption site selects no production election, membership, placement, peer-authentication, schema or identifier policy, and does not alter the canonical Session ownership rows.

integration/authority-store consumes the same selected Tokio/tokio-postgres features and pinned Bach/Bolero development profile for a bounded private current-grant reader and mutation adapter. Its [consumer packet](/home/dev-user/borg-runtime/integration/authority-store/consumer-packet.json), SHA256 `0fedcbdc4ad198967a93a495e51404c3920e43b84ecafba05fc4f92e65d5d35a`, declares the owned authority and canonical Session dependencies with default features disabled. The actual manifest, normal and development closure are checked against the existing native and simulation profiles before package execution; no resolved or compiled count is asserted here. Verified identity and issuer facts, local condition evaluation, expiry-domain mapping, retained-scope authentication and same-transaction currentness remain explicit producer prerequisites. This records consumption, not a production grant schema, storage layout, barrier capacity, retention policy or new grant of authority.

integration/configuration-store/native consumes the same selected Tokio/tokio-postgres features and pinned Bach/Bolero development profile for bounded private configuration reads, submission, pending state, evaluated refusal and operation settlement. Its [consumer packet](/home/dev-user/borg-runtime/integration/configuration-store/native/artifacts/source-start-gqeybuee/consumer-packet.json), SHA256 `420c7f68d761c9cd67f29ee14007db19179dc9d76bea10025f585160b38e370f`, records the existing configuration, canonical Session, lifecycle, governance, model and native-runtime components, with the authority component used only in development. The staged lock preserves the existing 128 external development records; the normal profile remains the selected native bundle. These are static declarations, not resolved or compiled feature counts. Actual normal and development graphs are verified before package execution. Full turn adoption still requires the canonical complete lifecycle producer, retained configuration plans, actual model-boundary and captured-prefix integration, and current authenticated configuration/grant facts. No duplicated Session authority, whole-image configuration store, production format or new policy is selected.

integration/session-store consumes the same native bundle and existing Bach/Bolero development profile for the canonical Session control, inbox, dedupe and doorbell boundary. Its [consumer packet](/home/dev-user/borg-runtime/integration/session-store/artifacts/source-start-xiks178l/dependency-packet.json), SHA256 `3fca6395bfb122dbead3b00bc23c05ca85710e0fa026656ffec41dad9c3a8766`, records the owned session, events, subscriptions, coordinator, lifecycle and native-runtime edges. The manifest selects the same Tokio/tokio-postgres features and pinned test-framework revisions. Its actual combined lock and target features are checked before execution; no resolved package count is asserted here. The private qualification projection is not a production physical mapping or evidence of bootstrap, ownership-transition or authority producers. No HTTP, TLS, JSON or content-crypto edge is selected for its ordinary profile.

integration/blob-native-receiver consumes the selected native bundle, existing serde1.0.229 and serde_json1.0.151 std profiles with defaults disabled, and the existing Bach/Bolero development closure. Its [consumer packet](/home/dev-user/borg-runtime/integration/blob-point-store/artifacts/admission-join-stage-f_nqzf9a/registration/packet.json), SHA256 `0e66af689572f98203fa50a008d0da9bcb459c99e00ba376e7df38e37a7e6d18`, records the typed synthetic tool decoder, awaited blob adapter and existing model/registry/dispatcher qualification edges. The staged normal inventory is native83 plus three already-selected JSON identities (86); the staged test inventory is parity128 plus those same identities (131). These are static inventories, not compiled feature counts. The actual graph is checked against the selected profiles before execution; JSON raw_value comes only through the existing governed-model test dependency. The blob-point-store parent additionally consumes the owned receipt component for same-transaction receiver settlement without changing its external profile. This records consumption, not a production tool schema, identifier/digest format, cipher, storage layout, cross-service atomicity or new authority grant.

All 83 package checksums were checked against official registry metadata before
selection. That metadata-only resolution and its offline reproduction did not
acquire or execute the packages. Acquisition, the actual target/build/dev feature
graph and qualification executions have separate evidence. Any additional identity
or feature remains outside the selected bundle and follows the existing approval
rule. The existing owned and Bach/Bolero components retain their own approved
dependency scopes. No production conformance, simulation exception or milestone
readiness follows from this selection.

## Native Inference Transport Qualification Bundle

The selected private profile is Reqwest0.13.5 with default features disabled and
rustls/http2, serde_json1.0.151 with default features disabled and std/raw_value,
and the existing Tokio1.53.2 rt/net/time/sync/io-util profile. Its
[162 exact external identities and registry checksums](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json)
are bound by the [transport lock](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/Cargo.lock),
SHA256 31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff.
This is a separate qualification graph and does not replace the existing native
83-package or Bach/Bolero dependency graphs.

The TLS profile includes Rustls0.23.45, platform-verifier0.7.1, aws-lc-rs1.18.1
and aws-lc-sys0.45.0. HTTP/2 adds h2 0.4.19 and its recorded dependencies. The
[installed package revisions and tool observations](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/native-http-review-build-d0q9kyni/installed-toolchain-observation.json)
define the existing native build bundle: GCC/G++13.3.0 and associated compiler
files, Binutils2.42, glibc2.39 development/runtime files, Linux6.8 userspace
headers, CMake3.28.3, Make4.3 and conditional pkgconf1.8.1. No system-package
installation, toolchain upgrade, FIPS/bindgen/Go route, external system-crypto
fallback or registry/configuration change is selected.

The consumption site is qualification/inference/native. The scope is exact
package acquisition and checksum verification, materialized target/build feature
inspection, native input/invocation discovery and guarded private builds, followed
by local synthetic HTTP/TLS/JSON qualification. Provider credentials remain in the
inference-service adapter, never the core. No real credential or provider request
is part of this scope. The [qualification packet](proposals/native-inference-transport-qualification.md)
records multiplexing, cancellation, origin, body/stream/depth limits, opaque-state
preservation and secret-handling cases. Enabled HTTP/2 features are not evidence
of negotiation, shared streams, bounded buffers or the ten-host scale target.

The external test consumer qualification/inference/native/rendered-post reuses the selected native-store and HTTP/TLS profiles to qualify the actual rendered request through the durable model/dispatcher path with a local synthetic peer. Its [registration packet](/home/dev-user/borg-runtime/qualification/inference/native/artifacts/rendered-post-join-plan/registration-2k9bv8qj/registration.json), SHA256 `f66b532e7005386efe7a761773f0fc9d830457fc09b26e687ed38d882748ff1d`, binds the exact 200-identity union and the reproduced lock `7817f24a68bff14299845d7b0594c52c64c28843fecd9c86aee5b6fe0e9597bc`. The resolved Linux normal/build projection has 135 external and 21 owned packages, including five proc-macro and 14 custom-build packages; these are metadata classifications, not compiled counts. No external identity or feature is added beyond the existing selected profiles.

The owned governed-reference-turn feature joined-fixture-support is empty and default-off. It exposes the shared finite qualification fixture to this external consumer and to its explicitly feature-gated original test; it is not a production API or a second implementation of the model/dispatch flow. The final owned feature, source and test routing are bound and checked before execution. Cargo unifies HTTP/TLS features in the combined external test graph; the ordinary core's separate default-off graph remains HTTP/TLS-free and is checked independently. Actual rendered bytes, durable-before-request ordering, reply recovery and governed effect replay need execution evidence. No real provider request, credential, production header, token format, deployment topology or new retry policy is selected by this registration.

qualification/inference/native/live-adapter is a separate private native transport consumer of the same selected HTTP/TLS, Tokio and JSON profiles. The [two-consumer packet](/home/dev-user/borg-runtime/qualification/inference/native/artifacts/native-live-scope/consumer-registration-5du0oz57/packet.json), SHA256 `67672d4b39ae2c0263a05789411ba1f168acf8ea5bc20195b648736591d7fc96`, binds six owned manifests and exact reproduced feature graphs. Its normal and local-peer development graphs retain the same 162 external lock identities and 98 active external packages with two owned packages. The default-off empty local-peer feature enables the explicit test target; its five direct h2/http/bytes/rustls/tokio-rustls development edges reuse already selected profiles without a feature delta. These are resolved graph counts, not compilation or test results.

The inference library's owned responses feature separates its model-response semantics from transport. The adapter selects only transport and excludes the model/session/core and PostgreSQL dependencies. Existing core, parity, real transport tests and rendered-POST consumers explicitly select responses to retain their existing algorithms. The governed-reference-turn private native_live_model target uses its default-off live-model-qualification feature, which enables the existing joined fixture. Its graph retains 114 external lock identities, 61 normal external and 20 owned packages; the development tree has 88 external packages and metadata has 90. Those projections remain distinct. The core's direct serde_json std/raw_value edge reuses its existing transitive package/features; its normal graph remains HTTP/TLS-free. The new isolated locks retain their own hashes and do not replace historical selected-profile locks.

Only the separate adapter receives environment:OPENAI_API_KEY when a concrete provider-call scope is released. The fixed selected endpoint/model are unchanged. The existing public SSL_CERT_FILE and HTTPS_PROXY inputs are captured and checked explicitly, without a direct-network fallback or TLS verification bypass. The core receives bounded status/body data through private qualification IPC and retains its actual rendered request and owned interpretation; it receives no provider credential. This registration permits the recorded private local qualification and selects no production process topology, assertion format or additional provider requests. A fresh finite source-bound execution receipt follows successful local checks before any live call; the consumed historical experiment allowance is not reset.


The metadata-only lock has an identical offline reproduction and expected registry
checksums. The inferred 98-package/99-context Linux projection is not a complete
Cargo unit graph: proc-macro placement, build-script effects, source archives and
actual compiler/sysroot inputs were not materialized by that research. Those
inputs are verified for the selected scope before their build; unexpected
dependencies, features or tools are not silently consumed.

The managed registry did not supply the initially considered JSON1.0.154 release.
The selected1.0.151 has the known finite-Value versus f32-infinity comparison
defect. Bounds/count validation uses exact checked paths rather than that
comparison, with the known counterexamples retained in qualification. This is not
an assertion that the library defect is fixed. It selects neither Borg's durable
codec nor an application encryption profile. The separately requested envelope
qualification remains outside this selection.

integration/collaboration-store also consumes the same serde_json1.0.151 identity
with only std enabled and defaults disabled for its private synthetic event
qualification. Its [exact reuse packet](/home/dev-user/borg-runtime/artifacts/execution/collaboration-json-reuse-1yjwfspe/packet.json)
keeps typed event projection, truncation and the bounded counting sink in owned
Rust logic. This adds no Reqwest/TLS edge, direct serde/serde_core dependency,
derive, raw_value, preserve_order or float_roundtrip feature. Resolved package and
feature closure is checked against the existing approved sets before execution.
The known numeric-comparison defect remains; that path is not used for bounds,
authorization or identity checks. This private JSON profile is not adoption of a
production durable or wire format.

## Native Adapter Parity Consumers

qualification/native-runtime/parity and qualification/inference/native/parity
are separate test consumers of their respective parent libraries, using the
existing pinned Bach and Bolero development dependencies and Bolero generator
patches. The [watch consumer packet](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/parity-consumers-peer-nv1dyfds/packet.json)
records its original path and feature edges. The
[inference test-routing packet](/home/dev-user/borg-runtime/qualification/inference/native/artifacts/split-routing-mokc7htb/packet.json)
supersedes that earlier packet's inference peer placement. Each parent exposes
an empty-dependency owned parity-tests feature for qualification support around
the same algorithm; default consumers do not receive that support. Both parent
locks are preserved.

The inference site's real-test surface uses the already selected Tokio1.53.2,
h2 0.4.19, http1.5.0, bytes1.12.1, Rustls0.23.45 and tokio-rustls0.26.6 directly
as development dependencies for its local HTTP/2 and TLS peer. Its authored
transport-tests.toml is evaluated through an isolated artifact manifest against
the same owned library source. Defaults are disabled and explicit features stay
within the recorded transport profile. Test targets are explicit; the parent
does not automatically discover this separately declared transport test. This
selects no additional identity, feature, server product or provider request.

The Bach inference consumer enables parity-tests and responses with parent defaults off,
compiling the same Body and Transfer source without Reqwest/H2. This keeps the
framework's exact indexmap2.11.2 dependency separate from indexmap2.14.2 in the
unchanged transport lock. Neither version is downgraded or substituted. The
request builder, status/chunk driver and URL/DNS/TLS binding remain native-only
in this increment; their owned simulation obligations are still open. This
partial test surface is not a simulation exception or full adapter qualification.

The owned watch, driver and bounded inference-control algorithms are shared
between native callers and injected Bach inputs. This is not simulation of
PostgreSQL, Tokio, Reqwest or TLS-provider internals. Real probes and Bach cases
have matching explicit expectations, with native-only observations kept separate.
Combined normal/build/development resolution is checked against the existing
approved bundles before use; registration does not turn an unreviewed dependency
or feature difference into an approval. Test-peer/native execution is not counted
as Bach coverage merely because it lives in the parity crate.

## Cargo Consumers

The following Cargo consumer map records each use of the selected test framework and its dependency closure. Path dependencies named borg-* are owned components, not externally supplied technologies.

| Consumer | Supplied direct dependencies |
|---|---|
| contracts/admission-limits | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/authority | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/blobs | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/child-sessions | bach (dev-dependencies), bolero (dev-dependencies), and existing owned authority/lifecycle/transcript/blob controls; no new key, signature or durable-format technology. |
| contracts/change-assessment | Standard-library change/evidence admission logic with the existing approved Bach/Bolero development-test closure. No new checker, signing, storage or deployment technology selected. |
| contracts/collaboration | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/compaction | bach (dev-dependencies), bolero (dev-dependencies); scheduled source-only packet, recorded before first dependency execution. |
| contracts/configuration | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/coordinator | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/credentials | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/deadlines | bach (dependencies), bolero (dev-dependencies) |
| contracts/decision-hook-requests | bach (dev-dependencies), bolero (dev-dependencies), and existing owned governance/deadline components; no HTTP/serialization/policy engine selected. |
| contracts/drain | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/events | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/feeds | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/frontend | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/governance | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/key-access | bach (dev-dependencies), bolero (dev-dependencies), and owned authority/deadline components for trusted selection and expiry/revocation control; no key material, cryptographic library or provider adapter selected. |
| contracts/lifecycle | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/live-view | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/model-turn | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/model-failures | Existing owned model-turn, deadlines and session components with the approved Bach/Bolero development-test closure. No new provider, network or retry-policy technology selected. |
| contracts/model-scheduler | Existing approved Bach/Bolero development-test closure and owned model-turn/deadline/session components; standard-library production admission logic. No new runtime, network, quota service or policy technology selected. |
| contracts/node-readiness | Standard-library readiness logic and the existing owned deadlines component with the approved Bach/Bolero development-test closure. No host-control, orchestration, network or deployment technology selected. |
| contracts/observation | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/receipts | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/record-rollout | Standard-library reader/variant write and rollback admission logic with the existing approved Bach/Bolero development-test closure. No codec, deployment engine or new external technology selected. |
| contracts/receipts/boundary-review | Owned receipt retention boundary examples using the existing receipts path and approved Bach/Bolero development-test closure; standard library only outside tests. No storage, receipt-state or stale-generation close decision selected. |
| contracts/session | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/subscriptions | bach (dev-dependencies), bolero (dev-dependencies); scheduled source-only packet, recorded before first dependency execution. |
| contracts/timers | bach (dependencies), bolero (dev-dependencies) |
| contracts/tool-registry | bach (dev-dependencies), bolero (dev-dependencies), and existing owned control components; actual spec hashing/schema/provider rendering remain separate dependencies. |
| contracts/transcript | bach (dev-dependencies), bolero (dev-dependencies) |
| contracts/workspace-control | Standard-library bounded workspace/lineage/promise control with the existing approved Bach/Bolero development-test closure. No sandbox, host substrate, durable format, storage engine, cryptographic library or deployment technology selected. |
| integration/admission-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/blob-store-metadata | Scheduled owned-path metadata integration with the approved PostgreSQL/libpq/system-Python harness; no byte backend, new Cargo technology or key-erasure mechanism selected. |
| integration/blob-native-receiver | Existing native bundle, selected serde/serde_json std profiles, owned blob/dispatcher/receipt/credential/registry components and pinned Bach/Bolero tests. Static86 normal and131 test external identities are checked by actual resolution before use; no new external identity or feature is selected. |
| integration/blob-point-store | Exact approved Tokio/tokio-postgres native bundle and owned blobs, authority, session and native-runtime library. No new external development dependency; Python only supervises the existing private PostgreSQL fixture. |
| integration/blob-point-store/parity | Existing pinned Bach/Bolero development closure and generator patches; owned point-store dependency with defaults disabled and its empty-dependency parity-test-api feature. The ordinary parent retains its native83 profile. The [static registration packet](/home/dev-user/borg-runtime/integration/blob-point-store/artifacts/orchestration-parity-start-dz9i9crh/registration-packet.json) binds 128 external lock records equal to the existing native-runtime/parity inventory and six owned records; actual resolution and compiled features are checked before execution. Same owned admission/fact/commit/recovery logic with modeled external transaction completions; no PostgreSQL-internal simulation or new production storage choice is claimed. |
| integration/collaboration-store | Exact approved Tokio/tokio-postgres native bundle, owned collaboration/feed/native-runtime libraries, existing Bach/Bolero development-test closure, and the already approved serde_json1.0.151 identity with std only and defaults disabled for the private event qualifier. Runtime behavior and effect SQL are Rust; no additional production storage or codec choice. |
| integration/configuration-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/authority-store | Existing native Tokio/tokio-postgres features, owned authority and canonical Session components, and pinned Bach/Bolero development dependencies. Actual closure verification precedes use; no new external profile or production authority/identity choice. |
| integration/configuration-store/native | Existing native Tokio/tokio-postgres features, owned CFG/S/L/H/M/native-runtime components, and pinned Bach/Bolero development profile with development-only AUTH. Actual closure verification and canonical lifecycle/adoption producers remain required; no new external profile or production format. |
| integration/coordinator-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/coordinator-store/native | Existing native Tokio/tokio-postgres features, owned coordinator and native-runtime components, plus pinned Bach/Bolero development dependencies. Static 83 normal and 128 development external identities require actual feature verification before use; no new external profile or production leadership policy is selected. |
| integration/deadline-turn | bach (dependencies), bolero (dev-dependencies) |
| integration/credential-dispatch | Historical Python-assisted private qualification used the already selected Rust/Cargo, system Python, PostgreSQL16.15/libpq profiles. [Retained receipt](/home/dev-user/borg-runtime/integration/credential-dispatch/artifacts/run-c2_io8r3/receipt.json), SHA256 `2e0f3a1ef7c9a60ab98747dcfefab960810e3fa35fe1c9e6fffac96b9cc11976`. This site correction adds no technology selection or current Rust/runtime conformance claim. |
| integration/drain-store | Historical Python-assisted private qualification used the already selected Rust/Cargo, system Python, PostgreSQL16.15/libpq profiles. [Retained receipt](/home/dev-user/borg-runtime/integration/drain-store/artifacts/run-acwcfaus/receipt.json), SHA256 `a843cba837bfdfc6a602c021f648aa7c88f975539fb1141d422d78ef41eaa321`. This site correction adds no technology selection or current Rust/runtime conformance claim. |
| integration/dispatch-usage | Owned-path Rust dispatcher/receipt/usage composition, including owned tool-registry and native-runtime paths, the exact approved Tokio1.53.2 rt/net/time/sync/io-util and tokio-postgres0.7.18 runtime profiles with defaults disabled, and the existing Bach/Bolero development-test closure. Normal and development closures are verified against the approved bundles before use. No HTTP/TLS edge or production layout is selected. The earlier Python harness and three preserved binaries retain their historical qualification scope. |
| integration/durable-session | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/event-delivery-view | Exact approved Tokio/tokio-postgres native bundle and owned authority/event/subscription/feed/coordinator/native-runtime libraries for bounded Rust consumer and publication callers; the [coordinator edge](/home/dev-user/borg-runtime/integration/event-delivery-view/artifacts/native-append-parent-lw70n4xl/owned-edge.json) adds no external package or feature. Existing Bach/Bolero test closure and private PostgreSQL/libpq/system-Python supervision remain. No join/rejoin cursor, event-hook format or production storage choice selected. |
| integration/governed-reference-turn | Existing native Tokio/tokio-postgres bundle, owned native-runtime/inference-types/dispatcher/credentials/registry edges and selected serde_json1.0.151 std/raw_value closure; inference defaults disabled, no Reqwest/HTTP/TLS in this core profile. Existing Bach/Bolero development closure and PostgreSQL/system-Python supervision retain separate scope. Exact merged external identities/features are verified before execution. No provider call, production mapping or cryptographic selection follows. |
| integration/lifecycle-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/live-view-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/observation-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/subscription-store | Scheduled owned-path integration with the approved PostgreSQL/libpq/system-Python harness and existing Bach/Bolero test closure; no initial/rejoin cursor or production sharding choice selected. |
| integration/timer-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/transcript-store | None in its Cargo manifest; native/harness dependencies are recorded above. |
| integration/turn-recovery | Scheduled owned-path integration using the approved PostgreSQL/libpq and system-Python harness; recorded before execution, with no new direct third-party Cargo dependency selected. |
| integration/session-store | Existing native Tokio/tokio-postgres features, owned S/E/ED/CO/L and native-runtime components, plus pinned Bach/Bolero development dependencies and generator patches. Actual normal/test resolution is required before execution; no HTTP/TLS/JSON edge or production mapping follows. |
| integration/workspace-control-store | Exact approved native Tokio/tokio-postgres bundle, owned workspace-control/native-runtime libraries and existing Bach/Bolero development-test closure. Bounded metadata command qualification only; no new host, isolation, key, snapshot or durable-format selection. |
| qualification/simulation | bach (dev-dependencies), bolero (dev-dependencies) |
| qualification/store-parity | bach (dependencies), bolero (dependencies) |
| qualification/stored-form | minicbor (dependencies) |
| qualification/inference/native/live-adapter | Existing selected Tokio/Reqwest/Rustls/AWS-LC/JSON profile with transport-only parent routing; default-off local-peer test target reuses five existing native peer dependencies without added external features. Normal adapter has no model/session/core or PostgreSQL dependency; the separately qualified core target remains HTTP/TLS-free. Exact packet and projection counts are recorded above. |
| qualification/assertion-signature/caller | Existing signature primitive/AWS-LC, native-store and JSON profiles through owned normal paths; pinned Bach and existing Tokio/tokio-postgres development features. Declared-call/current-receiver composition only, with actual union checked before execution and no general token/issuer authority selection. |
| qualification/native-runtime | Tokio1.53.2 and tokio-postgres0.7.18 with the exact root features and 83-package bundle above; existing owned components and already approved Bach/Bolero test closure retain their separate scopes. |
| qualification/native-runtime/parity | Parent native-runtime path dependency with its empty-dependency parity-tests feature; existing pinned Bach/Bolero development dependencies and generator patches. No direct Tokio dependency or new external scope. |
| qualification/inference/native | Reqwest0.13.5 rustls/http2, serde_json1.0.151 std/raw_value and Tokio1.53.2 rt/net/time/sync/io-util, defaults disabled, with the exact 162-package and installed native build bundles above. The isolated real-test manifest adds only the six already-selected local-peer development edges above. Private local qualification; no provider calls or implicit additional dependencies. |
| qualification/inference/native/parity | Parent inference/native path dependency with defaults off and explicit parity-tests/responses; existing pinned Bach/Bolero development dependencies and generator patches. Same owned Body/Transfer source, no Reqwest/H2 or local-peer dependency surface. No provider calls or new external scope. |
| qualification/store-parity/blob-session | Existing owned blob, session and authority components plus the approved Bach/Bolero test closure and PostgreSQL/libpq/system-Python harness. Reuses the existing store-parity corpus for bounded transaction outcomes; no new store, durability model, encoding or crypto choice. |

## Pinned Rust Dependency Identities

These65 observed lock entries include optional and target-specific packages. Their presence does not select an optional fuzzing engine or establish that every package was built. The selected test closure and recorded features remain the scope.

| Package | Version | Source |
|---|---|---|
| anyhow | 1.0.104 | registry+https://github.com/rust-lang/crates.io-index |
| arr_macro | 0.2.1 | registry+https://github.com/rust-lang/crates.io-index |
| arr_macro_impl | 0.2.1 | registry+https://github.com/rust-lang/crates.io-index |
| atomic-waker | 1.1.2 | registry+https://github.com/rust-lang/crates.io-index |
| bach | 0.1.2 | git+https://github.com/camshaft/bach?rev=8bddd1958ff66460ea7d0e060bf07b999262aadb#8bddd1958ff66460ea7d0e060bf07b999262aadb |
| bolero | 0.13.4 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-afl | 0.13.0 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-engine | 0.13.4 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-generator | 0.13.5 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-generator-derive | 0.13.4 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-honggfuzz | 0.13.0 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-kani | 0.13.0 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bolero-libfuzzer | 0.13.0 | git+https://github.com/camshaft/bolero?rev=71e5de0eceb043f3eda2f62ccd02d2be2b398c44#71e5de0eceb043f3eda2f62ccd02d2be2b398c44 |
| bytes | 1.12.1 | registry+https://github.com/rust-lang/crates.io-index |
| cc | 1.6.0 | registry+https://github.com/rust-lang/crates.io-index |
| cfg-if | 1.0.5 | registry+https://github.com/rust-lang/crates.io-index |
| either | 1.18.0 | registry+https://github.com/rust-lang/crates.io-index |
| equivalent | 1.0.2 | registry+https://github.com/rust-lang/crates.io-index |
| event-listener | 5.4.2 | registry+https://github.com/rust-lang/crates.io-index |
| event-listener-strategy | 0.5.4 | registry+https://github.com/rust-lang/crates.io-index |
| find-msvc-tools | 0.1.14 | registry+https://github.com/rust-lang/crates.io-index |
| futures-core | 0.3.34 | registry+https://github.com/rust-lang/crates.io-index |
| getrandom | 0.3.4 | registry+https://github.com/rust-lang/crates.io-index |
| hashbrown | 0.15.5 | registry+https://github.com/rust-lang/crates.io-index |
| indexmap | 2.11.2 | registry+https://github.com/rust-lang/crates.io-index |
| intrusive-collections | 0.10.3 | registry+https://github.com/rust-lang/crates.io-index |
| lazy_static | 1.5.1 | registry+https://github.com/rust-lang/crates.io-index |
| libc | 0.2.190 | registry+https://github.com/rust-lang/crates.io-index |
| memchr | 2.8.3 | registry+https://github.com/rust-lang/crates.io-index |
| minicbor | 2.3.0 | registry+https://github.com/rust-lang/crates.io-index |
| pin-project-lite | 0.2.17 | registry+https://github.com/rust-lang/crates.io-index |
| ppv-lite86 | 0.2.21 | registry+https://github.com/rust-lang/crates.io-index |
| pretty-hex | 0.4.2 | registry+https://github.com/rust-lang/crates.io-index |
| proc-macro-crate | 2.0.2 | registry+https://github.com/rust-lang/crates.io-index |
| proc-macro-hack | 0.5.20+deprecated | registry+https://github.com/rust-lang/crates.io-index |
| proc-macro-nested | 0.1.7 | registry+https://github.com/rust-lang/crates.io-index |
| proc-macro2 | 1.0.107 | registry+https://github.com/rust-lang/crates.io-index |
| quote | 1.0.47 | registry+https://github.com/rust-lang/crates.io-index |
| r-efi | 5.3.0 | registry+https://github.com/rust-lang/crates.io-index |
| rand | 0.10.3 | registry+https://github.com/rust-lang/crates.io-index |
| rand | 0.9.5 | registry+https://github.com/rust-lang/crates.io-index |
| rand_chacha | 0.9.0 | registry+https://github.com/rust-lang/crates.io-index |
| rand_core | 0.10.1 | registry+https://github.com/rust-lang/crates.io-index |
| rand_core | 0.9.5 | registry+https://github.com/rust-lang/crates.io-index |
| rand_xoshiro | 0.7.0 | registry+https://github.com/rust-lang/crates.io-index |
| rand_xoshiro | 0.8.1 | registry+https://github.com/rust-lang/crates.io-index |
| serde | 1.0.229 | registry+https://github.com/rust-lang/crates.io-index |
| serde_core | 1.0.229 | registry+https://github.com/rust-lang/crates.io-index |
| serde_derive | 1.0.229 | registry+https://github.com/rust-lang/crates.io-index |
| shlex | 2.0.1 | registry+https://github.com/rust-lang/crates.io-index |
| siphasher | 1.0.4 | registry+https://github.com/rust-lang/crates.io-index |
| slotmap | 1.1.1 | registry+https://github.com/rust-lang/crates.io-index |
| syn | 1.0.109 | registry+https://github.com/rust-lang/crates.io-index |
| syn | 2.0.119 | registry+https://github.com/rust-lang/crates.io-index |
| syn | 3.0.6 | registry+https://github.com/rust-lang/crates.io-index |
| tokio | 1.53.2 | registry+https://github.com/rust-lang/crates.io-index |
| toml_datetime | 0.6.3 | registry+https://github.com/rust-lang/crates.io-index |
| toml_edit | 0.20.2 | registry+https://github.com/rust-lang/crates.io-index |
| unicode-ident | 1.0.26 | registry+https://github.com/rust-lang/crates.io-index |
| version_check | 0.9.5 | registry+https://github.com/rust-lang/crates.io-index |
| wasip2 | 1.0.4+wasi-0.2.12 | registry+https://github.com/rust-lang/crates.io-index |
| winnow | 0.5.40 | registry+https://github.com/rust-lang/crates.io-index |
| wit-bindgen | 0.57.1 | registry+https://github.com/rust-lang/crates.io-index |
| zerocopy | 0.8.59 | registry+https://github.com/rust-lang/crates.io-index |
| zerocopy-derive | 0.8.59 | registry+https://github.com/rust-lang/crates.io-index |

## Evidence Inputs

The generated source/lock inventory is retained in runtime artifacts/execution/technology-approval-4gb0dczb/technology-inventory.json, SHA256 614ed7748a38bf8c0017e4d20198eb91be7830f8cf689b7cc8351675673edd6c. It records checksums and each consuming lock. The independently collected native ELF closure is retained in artifacts/execution/technology-stack-adoption-sucsjol2/native-dependency-approval-inputs.json, SHA256 175ec58727bef0dbd35bf5fb4c95694d958c14c41e18d4fae419eefa9d0df82e; it records45 inspected files and their hashes. These are evidence artifacts, not another authored requirements catalog.

This inventory is specific to the examined v1 development paths, not a complete inventory of unrelated host software. It grants no simulation exception or production conformance claim. Future additional technologies still follow the main specification's direct-approval requirement.

## Decider Engine Qualification Bundle

Selected scope: bounded private qualification of Wasmi2.0.0 at runtime `qualification/decider-sandbox`, target `x86_64-unknown-linux-gnu`. Root defaults are disabled; features are `stable`, `std`, `validate`, `portable-dispatch`, and `extra-checks`. The exact reviewed packet is frozen under [the five-decision adoption before-state](/home/dev-user/borg-runtime/artifacts/execution/five-decisions-adoption-n0k5tii9/before/proposals/decider-engine-qualification.md), SHA256 `bbd6c9baf518d54d22e9a6526684d14388f2e0c73cfe4219673b0834db247a70`.

| Package | Version | Registry archive SHA256 | Requested active features |
|---|---|---|---|
| bitflags | 2.13.2 | `3ded4057c258ba199e2d26386d3af3780957ecaee6c4ef4041c6b4b8b97c0b06` | (none) |
| libm | 0.2.16 | `b6d2cec3eae94f9f509c767b45932f1ada8350c4bdb85af2fcab4a3c14807981` | (none) |
| spin | 0.9.9 | `3763264f6b73151db08c50ff20d7d8a0b8796e021cdea7ceedad07b80155fa0e` | mutex, rwlock, spin_mutex, std |
| wasmi | 2.0.0 | `78693fcdd618e0fc34af59c6b8efa9ac5d58c68df940beff4bedddb6acfe7c27` | extra-checks, portable-dispatch, stable, std, validate |
| wasmi_collections | 2.0.0 | `8a8be2aa467cf2d29e96ff759472c36eeb44a3c81c67fc9cb76c9a24c519c557` | std |
| wasmi_core | 2.0.0 | `69372d5fda3ea3d1e0aa6603c7888110e0187e88ea17cd8fc2e2df0a0e1f37fa` | std |
| wasmi_ir | 2.0.0 | `8f17b774caa13c618c7244f1ee51fe23c5e7b8538a471fa46d9949779758aed6` | std |
| wasmparser | 0.228.0 | `4abf1132c1fdf747d56bbc1bb52152400c70f336870f968b85e89ea422198ae3` | features, std, validate |

All eight active archives are checked before use. The thirteen additional exact archives below are selected only for acquisition, static inspection and offline metadata, never their compilation or execution. The active normal/build graph remains the original eight and its exact feature/build profile; any graph delta stops dependent execution. The [dependency packet](/home/dev-user/borg-runtime/artifacts/execution/decider-sandbox-dependency-scope-tlm78chy/dependency-scope.json), SHA256 `9b7d2adc5e258e5f64647b2e2ad154a92c0581cbbc58e3bd99dbe9dcf341cb60`, supplies the per-package features, licenses, source-derived graph and initial build-script inventory. Its earlier unapproved status describes the preparation time; this selection does not convert that graph into archive-backed proof. Packaged manifests/features and Wasmi/wasmi_ir/wasmparser/libm build scripts are inspected before an offline locked build; mismatch stops dependent execution. The approved Rust/Cargo1.99 and explicit system Python3.12.3 profiles remain, with no guest compiler or new ambient host input.

The selected wasmi_ir2.0.0 generator is exactly `build.rs`, SHA256 `3f433746f416fb1cec6957102b480de050119a1f96b8ecf5cfb9062ae02a27d2`, and the fourteen bundled helpers listed in [the packaged-input packet](/home/dev-user/borg-runtime/qualification/decider-sandbox/artifacts/packaged-scope-delta-bqpm1368/packet.json), SHA256 `c3374f7e943c82d5dce98263a0edd01003f2fba288a016382fda71f7f3387371`. Its archive checksum remains `8f17b774caa13c618c7244f1ee51fe23c5e7b8538a471fa46d9949779758aed6`. The generator emits only `op.rs`, `op_code.rs`, `encode.rs` and `decode.rs` into its isolated build output directory under the selected compiler and feature profile. This adds no external package, build dependency, feature, guest compiler or public ABI. The packet's historical unapproved banner is superseded only for this generator closure. The separately selected metadata-only alternative below includes equivalent1.0.2 and the other twelve exact identities solely for static inspection and offline metadata. It does not extend generator or engine execution to those packages. Generator-only evidence does not establish the engine's Cargo graph or satisfy its packaged-input gate.

Consumption and simulation boundary: the real interpreter is an external dependency behind the owned governance decider adapter; its internals are not claimed to execute under Bach. Bach exercises owned sequencing, bounded decoding and failure handling with a substitute; the same expectations run against the actual engine, including a deliberately divergent simulator case. The actual H path is Pipeline::evaluate, ordered Check, DeciderResults::answer and closed Reply handling. Protected platform checks, live authority and durable decision/result/state-slot/usage association remain outside this engine selection. No adapter or production guest ABI is established by this record.

The finite private profile uses validated integer-only guests with strict minimal imports, no start function, no filesystem/network/ambient time/entropy, fresh evaluation state, bounded copying and engine fuel/stack/memory limits. It exercises input-dependent verdicts, transformations, Block reasons, malformed modules/imports/output, exhaustion/recursion/memory growth and cross-evaluation isolation. Actual supervisor deadline/cleanup and total host/validation/cache/concurrency accounting remain required; fuel alone proves neither elapsed time nor total memory. The stateless first experiment does not waive the full per-session state-slot obligation. Acquisition/build/tests and production qualification have separate receipts; this selection alone proves none of them.

## Wasmi Metadata-Only Archives

The selected [request](/home/dev-user/borg-runtime/artifacts/execution/wasmi-metadata-request-6qatryg4/request.json), SHA256 `da9350b9545eb87c0e4f7e92e7daf4bf93854a81637f6aea454f7252df3f5fb0`, identifies exactly these thirteen archives. Reuse of verified local bytes or acquisition is allowed; extraction safety and archive checksum checks precede inspection. Offline metadata resolves the packaged graph without executing package code. A lock entry, proc-macro or generator in this table receives no compile or execution permission. The engine graph and build gates remain those in Decider Engine Qualification.

| Package | Version | Archive SHA256 |
|---|---|---|
| equivalent | 1.0.2 | `877a4ace8713b0bcf2a4e7eec82529c029f1d0619886d18145fea96c3ffe5c0f` |
| foldhash | 0.1.5 | `d9c4f5dac5e15c24eb999c26181a6ca40b39fe946cbe4c263c7209467bc83af2` |
| hashbrown | 0.15.5 | `9229cfe53dfd69f0609a49f65461bd93001ea1ef889cd5529dd176593f5338a1` |
| hashbrown | 0.17.1 | `ed5909b6e89a2db4456e54cd5f673791d7eca6732202bbf2a9cc504fe2f9b84a` |
| indexmap | 2.14.2 | `cc4e190f5d26ca7051642629da2c52fc03bde85a03197c99408dcd291734c855` |
| proc-macro2 | 1.0.107 | `985e7ec9bb745e6ce6535b544d84d6cd6f7ad8bd711c398938ae983b91a766d9` |
| quote | 1.0.47 | `1fbf4db142a473a8d80c26bbf18454ed458bf8d26c8219c331daecfdbd079001` |
| serde | 1.0.229 | `4148590afebada386688f18773da617792bf2ef03ffc1e4cbd2b1d45b023e0ba` |
| serde_core | 1.0.229 | `67dca2c9c51e58a4791a4b1ed58308b39c64224d349a935ab5039aa360942a48` |
| serde_derive | 1.0.229 | `e7a5d71263a5a7d47b41f6b3f06ba276f10cc18b0931f1799f710578e2309348` |
| string-interner | 0.19.0 | `23de088478b31c349c9ba67816fa55d9355232d63c3afea8bf513e31f0f1d2c0` |
| syn | 3.0.6 | `8593e8e72159ed2257d083c7a454a85cbf854f37a0966d8d483aff8c8a3ebcee` |
| unicode-ident | 1.0.26 | `d245f478577f809a851594d02313b640fb437e0bb33866753cff937863096954` |

## Immutable Bytes And Protected Record Target

PostgreSQL16.15 Ubuntu16.15-0ubuntu0.24.04.1 is selected for production encrypted immutable bytes and their metadata in one transaction, using the existing engine inputs. The owned service/adapter and its caller composition remain subject to actual atomicity, recovery, fencing, Bach/real parity and simultaneous-scale qualification. No new distribution, installed service or production deployment is selected. Existing libpq and private test uses retain their scopes.

C2-service-held selects the exact descriptor430e04637ff13cd8c47a2272d7f1630e86820e532ca3867a74f197d37e514a2e and normal/build map74e5742840d4bcd5f1798ba487269653df96c82a74ccc193c9b366924feaf75b linked from [the normative target](implementation.md#protected-session-record-target). That section supplies the binding selected meaning. The map's73 external package identities/checksums/features, with the existing compiler profile and OpenBao2.7.1 bundle/SBOM, are the exact supplied boundary. The private envelope/native qualification bundles above remain historical scopes; this separate selection adds their exact C2 production-target use, not arbitrary use of every lock/dev identity. Vendor internals do not become Bach-simulated by this selection. Owned codec/key/lookup/transaction orchestration requires the same expectations against injected and actual dependencies, including deliberately divergent substitutes.

The selected OpenBao profile is service-held root/epoch and owner-removal protection using derived Transit aes256-gcm96. No service endpoint, credentials, topology, new plugin or production grant is selected. Actual committed resource/key/attempt issuers, old/new reader cutover, exact binary identity transport, final-use/cache/currentness and irreversible live-key deletion remain implementation and qualification obligations. A node ResolveOwner request can require two actual backend decrypts; retain that accounting distinction and the shared one-call claim limitation. The existing backup-key retention exception remains in force. No old private codec, byte or key-service test is relabeled as C2 qualification.
