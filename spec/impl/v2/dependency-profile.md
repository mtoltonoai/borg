# V2 Dependency Build Profile

The v2 technology families are selected in [the decision record](decisions.md). This file pins the qualification profile selected for the authorized v2 build, as recorded in decisions.md. Selection is not an installed stack, resolved lockfile or successful qualification result. Existing v1 locks were intentionally deleted and are not reused as v2 proof. Planning input hashes bind this document.

## Direct Build Inputs

| Input | Proposed version/profile | Purpose and remaining verification |
|---|---|---|
| Rust/Cargo | Installed 1.99.0; rustc commit b940084d7eb6a299eb4bfeb8e34901bc051e7ac4; Cargo commit 5f94df4789f005f9a352888e8355ffc645b7ed0e; x86_64-unknown-linux-gnu | Reuse observed toolchain; freeze actual executable hashes before qualification. |
| SQLite | 3.54.0 official amalgamation, private static library | Current-engine candidate; local WAL with synchronous=FULL, foreign keys, bounded transactions and explicit busy/unknown-outcome handling. Not the old system library. |
| rusqlite | =0.40.2, defaults off, modern_sqlite/limits/hooks | Released client; one owned blocking database queue, no ORM or async database wrapper. |
| libsqlite3-sys | =0.38.2, no bundled engine or buildtime_bindgen | Explicit static linkage to selected SQLite; verify the older pregenerated bindings against the newer engine's actual used APIs. |
| Tokio | =1.53.2, defaults off; rt-multi-thread/net/time/sync/io-util/process/signal/macros | Bounded async/network/process work. Process/signal/macros/multithread are explicit feature proposals beyond old v1 profiles. |
| Hyper | =1.11.1, defaults off; server/http1 | Private controller API with an existing HTTP parser. |
| hyper-util | =0.1.21, defaults off; tokio/server/server-graceful/http1 | Async adaptation and shutdown; no new routing framework initially. |
| http-body-util; bytes | =0.1.3 defaults off; =1.12.1 | Bounded HTTP body handling; application limits remain explicit. |
| Reqwest | =0.13.5, defaults off; rustls/http2 | Native provider HTTPS. Bounded chunk consumption; no added cookies/compression/HTTP3/blocking client. |
| AWS-LC Rust API | =1.18.1, defaults off; aws-lc-sys; existing sys0.45.0 closure | Reuse the already locked TLS primitive for bounded document-content SHA-256 identities. The direct API edge is expected to preserve the selected package, feature and build-script union; offline metadata equality is required before consuming the manifest change. No application keys, signatures, encryption or new security subsystem. |
| Serde | =1.0.229, defaults off; std/derive | Fixed typed application records. |
| serde_json | =1.0.151, defaults off; std/raw_value/float_roundtrip | Available managed-registry version. The known Value-to-f32 infinity-comparison defect remains in the library; current owned decoding and pinned schema validation use integer/f64 paths instead. arbitrary_precision remains off. Qualify the actual large-number, const/enum and nonfinite rejection paths before accepting this substitution. |
| jsonschema | =0.50.1, defaults off, no optional features | Native output-schema validation; explicit Draft202012, schema validation, local/internal resources only, no HTTP/file retrieval. Bound schema/result size and complexity; qualify pathological inputs. |
| Bach; Bolero | 0.1.2 at 8bddd1958ff66460ea7d0e060bf07b999262aadb; 0.13.4 at 71e5de0eceb043f3eda2f62ccd02d2be2b398c44 | Development dependencies for focused owned-state testing. Exact maintained patch/features and resolved generator closure still require verification. |
| Codex | Candidate cached package 0.163.0-alpha.10, x86_64-unknown-linux-musl | Local-stdio app-server. Local metadata associates cd643519fb56011557d09b41053a3a4c0e013e1f; this is not an independently verified upstream source identity. Pin the actual package, not a moving launcher track. |
| Git | Observed 2.43.0; Ubuntu 1:2.43.0-1ubuntu7.3 | Existing command-line integration, not a new libgit2 dependency. Verify the four target hosts before rollout. |
| OpenSSH | Observed 9.6p1; Ubuntu 1:9.6p1-3ubuntu13.19 | Existing authenticated host access/loopback forwarding; no Rust SSH library, key service or certificate system. |
| Duvet; Python | Existing Duvet 0.4.3; retained tooling requires Python 3.11 or later | Specification/planning tooling only. Keep existing runtime lock and copied snapshot unchanged. |

Direct version constraints are exact. The current ordinary lock resolves 256 dependency packages; actual target/feature qualification is recorded separately. A version table is not a checksum-complete lockfile. Qualification produces one ordinary Cargo.lock and feature inventory for actual Linux normal/build/development targets; it does not recreate separate qualification packages for every caller.

## SQLite Source And Linking

Official artifact: sqlite-amalgamation-3540000.zip. Published SHA3-256: 7b670a62fdfbd672b75fef004cb703c8a3e87d3a5cc7d675b4a08337004a2d93. Source identity: 2026-10-09 15:46:58 be8d059e9a49089ab2dce5ed26dd87aaf598fdc5fbe0b107c0dd758464bcd5e3. These published identities were the packet input. The subsequent authorized build verified the archive hash and compiled the private SQLite library; detailed results remain in its execution receipt.

The supported linked build path uses explicit SQLITE3_LIB_DIR, SQLITE3_INCLUDE_DIR and SQLITE3_STATIC=1, with a private sqlite3.pc and PKG_CONFIG_LIBDIR restricted to it. Use the existing GCC13.3/ar toolchain, -O2 -fPIC -std=c11 and SQLITE_THREADSAFE=1, SQLITE_DEFAULT_FOREIGN_KEYS=1, SQLITE_ENABLE_API_ARMOR. No engine fork or source generator is proposed. Disable extension loading and use only application-owned SQL. The exact native link closure remains to verify.

The released rusqlite/libsqlite3-sys bundled alternative contains SQLite3.53.2. It contains the WAL-reset fix but is not 3.54.0; labelling that bundle as the new engine would be false. The selected proposal instead pairs its supported linked mode and pregenerated bindings with the explicit newer static engine. Acceptance checks link symbols, actual library/source identity, compile options and used APIs, and proves that the old system library was not selected accidentally.

The system package observed here is SQLite3.45.1-1ubuntu2.8. Its local backport history did not establish the WAL-reset correction, so automatic system-library discovery is not the qualification path. FULL versus NORMAL synchronization is a durability choice, not a benchmark shortcut; cancellation of an async waiter does not prove a database transaction rolled back.

The controller uses existing Linux procfs descriptor/mount metadata and metadata-only open flags to verify its local filesystem before SQLite access. It recognizes ext4, XFS and Btrfs filesystem semantics and refuses network, FUSE, overlay, memory-backed or unknown families. Actual local tests cover ext4; other recognized deployments remain unqualified. This adds no package, command-line utility, mount setup or filesystem migration. The checks follow the [kernel procfs contract](https://docs.kernel.org/filesystems/proc.html) and the selected engine's [local-host WAL constraint](https://sqlite.org/wal.html).

## Document Content Identity API Reuse

The collaboration document helper uses the already selected AWS-LC package through a direct Rust API edge. Its content identity is `sha256:` followed by 64 lowercase hexadecimal digits over the fixed bytes `borg.collaboration.document-content.v1` plus one zero byte, followed by the existing typed compact JSON encoding of `DocumentContent` in `title`, then `body` field order. The identity binds exact text, including Unicode and escapes; it does not authenticate the author, authorize a mutation or encrypt content. Existing workflow text/record and transport body/identity bounds apply before variable-size encoding and output. Supplied content identities are checked by the canonical document operation; retained versions recompute the same identity.

The candidate direct declaration is aws-lc-rs1.18.1 with defaults disabled and only aws-lc-sys explicitly requested. aws-lc-rs1.18.1 and aws-lc-sys0.45.0 are already in the selected TLS closure. Default alloc/ring-io/ring-sig-verify features are not enabled. Before integration, compare the complete offline package/source/checksum, resolved feature and build-script inventory against the unchanged current graph; only the root package's direct dependency edge may be added. An unexpected union change stops this integration. This is an API-role reuse, not a new acquisition or a separate cryptographic subsystem. The versioned content preimage is not silently reinterpreted after a format change; future content-schema or encoding changes need an explicit compatible identity/migration disposition.

## Known Feature And Evidence Limits

- libsqlite3-sys still has pkg-config/vcpkg build dependencies through its defaults. Their exact resolved versions are recorded in the ordinary lockfile.
- Reqwest brings Tower/Tower-HTTP transitively, and its TLS closure includes AWS-LC/platform verification and native build inputs. Omitting an application middleware framework does not remove those packages.
- jsonschema brings referencing/regex/fancy-regex/fraction/idna and other dependencies even with defaults off. No global compile/validation deadline has been proved. Schema/result bounds and pathological-case qualification are required; typed Serde alone is not an arbitrary JSON Schema validator.
- Current Codex documentation is not a schema generated from alpha.10. Qualify the exact pinned binary's methods, permissions, history behavior, unknown-start handling and process cleanup. Experimental API support is not a vendor production guarantee.
- The ordinary lockfile records resolved package checksums; Cargo metadata records the selected feature union. Successful runtime build, provider access and scale performance remain unproved at this checkpoint. At packet freeze no package had been acquired or executed; the current build progress below distinguishes subsequent qualification from that historical state.


## Frozen Build Profile And Local Inputs

This is the one selected direct-input qualification profile for all four batches. The integrator resolves one full normal/build/development dependency graph and produces one ordinary lockfile before releasing the compiled shared fixture. Resolution checks exact direct pins, transitive source/checksums, feature union, license/build scripts and the observed target. Any unexpected package or incompatible feature change is reported once for the whole package; owners do not create separate package closures or switch versions independently. No full transitive checksum claim is possible before that resolution has occurred.

The initial browser implementation uses owned Rust to render escaped HTML and ordinary forms/links. No frontend package manager, client JavaScript runtime, template framework or separate asset build stack is needed for the reviewed first UI. Browser correctness and actual human interaction still require browser acceptance. This does not select a new browser automation dependency; reuse an existing supported harness where available and record the actual browser/profile when qualification starts.

Read-only local file observations for this packet:

| Input | SHA256 | Qualification meaning |
|---|---|---|
| Existing stable-toolchain rustc | f3834d26669b03f6855fa54bd2381443123e860167bc0c7a8dd137e5cf6e5e4f | Matches the observed toolchain file; verify recorded version/target when executing it. |
| Existing stable-toolchain cargo | e951141cc55a6cd7b9876d187bd30a3720e6086b99413af95e3b8de1cdd72f14 | Same whole-package build entry point; no new lock resolved. |
| Cached Codex0.163.0-alpha.10 entrypoint | 70000c54b0eef44deabaddd249533c757533c8ab80d3906d73681029d3f23072 | Exact local executable candidate only; not upstream attestation or successful schema/lifecycle qualification. |
| Cached Codex package metadata | baba4cd9a6535c513cbae64b1855219cf7d6c39572dc6c51eb994523b3afc170 | Records target/layout and the entrypoint/resources paths; preserve the package, not just one executable. |
| Explicit /usr/bin/git | 2a8c18fbf43da9f692d75474c72bea9dfd796c260b0f3dfe456376abc3bbd668 | Use the explicit installed Git executable for qualification. PATH currently resolves git through an unrelated wrapper; that wrapper is not added to this profile. |
| /usr/bin/ssh | 31430165113e51a046f3b9fa2a60ec98e073cc6c0411a081b3b1d76ed6a4eb6a | Existing transport executable; host credentials/configuration are unchanged. |

Qualification has three bounded checks: (1) package build and exact SQLite identity/transaction/restart behavior; (2) pinned Codex-generated schema, start/event/interrupt/history/process cleanup against the actual binary; (3) actual Git checkpoint acknowledgment/reconciliation and native HTTPS/schema behavior. These belong to the integrator and component batches, not to a new qualification service. The current documentation check confirms the chosen local-stdio lifecycle, but it cannot certify the cached alpha's exact ABI. The official [app-server guide](https://learn.chatgpt.com/docs/app-server) and [developer command status](https://learn.chatgpt.com/docs/developer-commands) remain the published references.

## Primary Sources

- [SQLite download/source identity](https://www.sqlite.org/download.html), [3.54.0 release](https://www.sqlite.org/releaselog/3_54_0.html), [WAL constraints and reset correction](https://www.sqlite.org/wal.html), [synchronous behavior](https://www.sqlite.org/pragma.html#pragma_synchronous).
- [rusqlite0.40.2 manifest](https://raw.githubusercontent.com/rusqlite/rusqlite/v0.40.2/Cargo.toml), [sys0.38.2 manifest](https://raw.githubusercontent.com/rusqlite/rusqlite/v0.40.2/libsqlite3-sys/Cargo.toml), [linked build path](https://raw.githubusercontent.com/rusqlite/rusqlite/v0.40.2/libsqlite3-sys/build.rs).
- [Tokio manifest](https://raw.githubusercontent.com/tokio-rs/tokio/tokio-1.53.2/tokio/Cargo.toml), [Reqwest manifest](https://raw.githubusercontent.com/seanmonstar/reqwest/v0.13.5/Cargo.toml), [Hyper manifest](https://raw.githubusercontent.com/hyperium/hyper/v1.11.1/Cargo.toml), [hyper-util manifest](https://raw.githubusercontent.com/hyperium/hyper-util/v0.1.21/Cargo.toml).
- [Serde manifest](https://raw.githubusercontent.com/serde-rs/serde/v1.0.229/serde/Cargo.toml), [serde_json release fixes](https://github.com/serde-rs/json/releases), [jsonschema0.50.1 manifest](https://raw.githubusercontent.com/Stranger6667/jsonschema/rust-v0.50.1/crates/jsonschema/Cargo.toml), [validator options](https://raw.githubusercontent.com/Stranger6667/jsonschema/rust-v0.50.1/crates/jsonschema/src/options.rs).
- [Codex app-server](https://learn.chatgpt.com/docs/app-server), [developer command limits](https://learn.chatgpt.com/docs/developer-commands), [state locations](https://learn.chatgpt.com/docs/config-file/environment-variables), [Git push](https://git-scm.com/docs/git-push), [OpenSSH configuration](https://man.openbsd.org/ssh_config).

## Current Qualification Checkpoint

The authorized v2 build acquired the exact SQLite3.54.0 amalgamation, verified its published SHA3-256, built the private library and passed primitive source-identity, committed/rolled-back transaction and reopen checks. These do not yet qualify the Rust static link or full crash behavior. The pinned Codex binary generated its schema and passed a local initialize/initialized handshake without a model call.

The earlier serde_json1.0.154 pin was unavailable from the managed Cargo registry. The selected replacement is1.0.151, with the same direct feature set and the usage limits above. Normal Cargo lock resolution and metadata acquisition now succeed through the unchanged managed registry. The [version qualification receipts](../../../../borg-runtime/artifacts/v2/build-60156282/serde-json-alternative/) preserve the earlier inputs, selected checksum, resolved graph and actual test results. No exception request or alternate registry was used. Runtime/browser and provider qualification remain incomplete.

The first package manifest makes the existing default development roles explicit: Bach net only (other optional features off), Bolero std only, and the selected Bolero revision patches its generator/derive crates so Bach does not resolve a competing generator source. Optional fuzz engines are not selected for execution. The normal/build/development graph now resolves; target-specific build and execution evidence remain distinct from resolution.
