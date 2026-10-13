# Envelope Integration Qualification

The technology inputs below are selected for private qualification, as recorded
in the [implementation specification](../implementation.md#externally-supplied-technologies)
and [decision record](../decisions.md#envelope-workspace-inspection-and-inbound-technology-qualification).
The hierarchy/profile below remains an experiment, not an adopted production
cipher, durable format or custody design. The v1 backup-key exception remains.

## Recommendation

Implement and qualify one owned envelope component using existing cryptographic
libraries, the selected OpenBao fixture and the already-approved PostgreSQL and
minicbor qualification stack. This closes an actual implementation gap: the
current integration fixtures persist synthetic tenant content as plaintext, and
the blob component implements ownership/pins without encryption.

The first delivery is a sealed private state/inbox/receipt flow with actual
persistence and restart, then reuse of that same component in the governed
model/tool path. All data and credentials are synthetic. This experiment does not
establish production readiness or authorize a live-model call.

## Selected Qualification Inputs

| Candidate | Exact version | Proposed use |
|---|---|---|
| RustCrypto aes-gcm | 0.11.1 | AES-256-GCM authenticated encryption of mutable content. |
| RustCrypto aes-siv | 0.8.0 | Deterministic AES-256-CMAC-SIV for immutable blobs and key wrappers. |
| RustCrypto hkdf | 0.13.0 | Domain-separated local epoch and purpose-key derivation. |
| RustCrypto sha2 | 0.11.0 | SHA-256 for derivation and stored-ciphertext addresses. |
| zeroize | 1.9.1 | Explicit destruction of owned key buffers. |

The [candidate dependency inventory](../../../../../borg-runtime/qualification/key-service/artifacts/next-envelope-proposal-20261012/candidate-dependencies.json)
lists the five roots, selected features and 28 conservative package identities
with archive hashes. Its SHA256 is
`01697f2e75ce20c1d5bfe73cd77b4a398fbb0115fd491eddbed9029fc32f40e9`.
It is registry metadata, not an executed Cargo resolution. An approved use first
verifies the actual lock, features and acquired source bytes against this bundle;
an additional dependency or changed identity is not silently accepted.

Default features and ambient entropy are disabled. The experiment uses in-place
operations, caller-provided buffers and injected entropy. The existing approved
Rust/Cargo, Bach/Bolero, system Python, PostgreSQL/libpq, OpenBao and minicbor
versions are reused. No new SDK, service, compiler or HTTP/TLS library is requested.

The requested consumption sites are `contracts/envelope` and
`integration/sealed-store`, followed by the existing `integration/durable-session`
consumer. Library internals are real external dependencies; execution of owned
logic under Bach does not prove that vendor internals are simulation-qualified.

## Comparison And Limits

RustCrypto is the recommended private experiment because its primitive APIs
support explicit buffers and injected inputs, and its deterministic mode covers
the immutable-retry requirement without inventing a cipher. The screened
alternative, ring 0.17.14, has a different implementation/audit history but does
not supply the required deterministic SIV mode in its inspected public AEAD API;
it would still require a second provider and adds native build/entropy machinery.

The aes-gcm project reports an NCC Group audit. That is not an audit of every
current dependency or this proposed composition. The aes-siv 0.8.0 source explicitly
warns that its implementation has not been audited or thoroughly assessed for
constant-time behavior. This is a material production limitation. This request is
for bounded private qualification, not approval to protect production data.
Zeroization of owned buffers also does not prove erasure of unknown allocator,
transport, register or host copies.

Primary references: [AES-GCM](https://docs.rs/aes-gcm/0.11.1/aes_gcm/),
[AES-SIV](https://docs.rs/aes-siv/0.8.0/aes_siv/),
[HKDF](https://docs.rs/hkdf/0.13.0/hkdf/),
[zeroize](https://docs.rs/zeroize/1.9.1/zeroize/),
[ring API](https://docs.rs/crate/ring/0.17.14/source/src/aead.rs),
[RFC5297](https://www.rfc-editor.org/rfc/rfc5297.html#section-4).

## Concrete Integration Hypothesis

Keep two independent live dependencies: a dedicated tenant root and a removable
owner key. A root-protected capsule contains a tenant derivation seed and the
owner-wrapped owner secret. An owned Borg resolver selects both OpenBao paths from
trusted provisioned mappings, checks the current authorized tenant/owner, and
resolves the capsule. The node makes one resolver call; the resolver's two
underlying OpenBao decrypt calls are recorded separately, not described as one
OpenBao call. Off-host release returns only the authorized owner's data key.

The owned component derives an epoch branch locally and wraps each random resource
key under both that branch and the owner secret, with separate purpose contexts.
No branch-only resource wrapper is retained. Live owner deletion prevents new
owner resolution; live tenant-root deletion prevents new tenant resolution.
Existing five-minute cache bounds remain. An epoch rewrap with valid cached
material makes no key-service call; expired material fails closed.

Mutable values use a fresh injected 96-bit GCM nonce. The private experiment caps
encryption attempts at 2^20 per resource-key generation before rotation. This
bounds collision risk; it does not claim impossible collision. Durable attempt
reservations are charged before encryption, including aborted writes and concurrent
or restarted callers. An uncertain commit reuses its previously sealed bytes.
Immutable blobs use deterministic SIV; the resource key, wrapped key and exact
ciphertext are durably prepared before publication and reused across retries.
Re-provisioning a random wrapper must not silently change an immutable object.

This is an integration hypothesis requiring review and actual tests. OpenBao's
existing qualifier did not test this hierarchy, resolver, local rotation or
envelope. Its admin-policy checks also did not prove production deployment/admin/
use separation. No production role or credential arrangement is selected here.

## Private Format And Test Boundary

Reuse the existing strict-minicbor prevalidation and borrowed-span work. The
temporary tagged profile uses definite maps, unique integer tags, minimal length/
integer encodings and explicit algorithm/format variants. The header binds the
expected tenant, owner, resource/range, session/event identities where applicable,
lease and key generation. Store-provided labels never select a key. Unknown fields
survive permitted edits; unknown mandatory variants fail without replacing bytes.

Proposed first-experiment bounds are 64 KiB payload, 16 KiB envelope/header and
nesting depth 32. These are disposable fixture limits, not replacements for shared
production limits. The complete proposal fixes injected Python `os.urandom` for
real private entropy and Bach entropy for simulation. It does not adopt a durable
format or migrate plaintext-era fixtures.

Acceptance checks include:

- Published primitive vectors; altered authenticated fields and corrupt tags
  return no plaintext; tenant/owner/purpose swaps fail.
- A cold read makes one node-to-resolver call; epoch rewrap makes zero calls;
  denied, expired and unavailable resolutions fail closed.
- Live owner deletion leaves another owner's positive control readable; live
  root deletion prevents all tenant resolution after cache expiry. Backup recovery
  is reported under the v1 exception, without an original-erasure success claim.
- Identical immutable retry inputs yield identical stored bytes and addresses
  across restart; changed content cannot replace an existing immutable identity.
- Database snapshots contain no synthetic plaintext/key sentinels in selected
  content-bearing columns. Lost commit replies and process restart recover the
  exact envelope and apply the effect once.
- The same owned expectations run under Bach and against actual resolver/store
  probes. Actual dependency failures and scope gaps remain visible.
- Nested unknown-field edits, duplicate/nonminimal/truncated values and allocation
  limits exercise the full strict profile, not only a decode/encode round trip.

The complete [source-backed proposal and replay matrix](../../../../../borg-runtime/qualification/key-service/artifacts/next-envelope-proposal-20261012/proposal.json)
retains its original hashes and reviewed counterexamples. This batch does not
relax [stored-form](../../../contracts/stored-form.md),
[envelope/key boundaries](../../../core/encryption-and-blobs.md),
[shared encryption](../../../core/shared-framework.md#envelope-encryption), or
[root administration](../../../core/tenancy-and-confidentiality.md#root-key-administration-is-separated).

## Current Qualification Scope

The pinned technology bundle may now be acquired and verified for the synthetic
qualification and integration described here. Actual resolution, input hashes,
feature scope and primitive/owned integration evidence remain required. Production
algorithm, hierarchy, format and custody adoption remain separate from this
technology selection. The AWS-LC comparison is not a selected substitution.
There is no renewed backup-erasure or KMS choice in this work.
