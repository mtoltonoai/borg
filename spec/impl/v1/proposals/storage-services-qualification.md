# Object And Key Services: Qualification Decisions

Prepared 2026-10-12. The OpenBao experiment below is now selected for private
qualification in the main specification; its production custody design remains
unselected. Object-store candidates remain proposals. Matthew had already
directed v1 to provide a key service and approved the PostgreSQL/Python
development stack.

Initial review response: Matthew requested Ceph/Kubo and key-service tradeoffs
instead of approving these experiments. The broader
[comparison](storage-service-tradeoffs.md) supersedes the object-candidate ranking
below: SeaweedFS remains a smaller experiment, not an established winner over
Ceph. Subsequently Matthew accepted OpenBao as the reuse candidate; the exact
bounded qualification selection is recorded in [the decision record](../decisions.md#openbao-qualification-choice).
This does not approve either object candidate, a production custody mechanism or
the separate store-layout proposal.

## Recommendation

Evaluate SeaweedFS 4.48 for encrypted large-object storage and OpenBao 2.7.1 for
key-service primitives in two separate disposable local experiments. Keep the
production choice open until the original requirements pass. In particular,
ordinary key deletion with recoverable old snapshots is not sufficient for Borg.
The key experiment should expose that failure before any custody design is chosen.

The object-store experiment can progress independently of key-service success by
using synthetic ciphertext. Such bytes do not qualify encryption or key erasure.
Transactional state, ownership, pins and references stay in the existing durable
framework; large object content belongs behind the object-store interface. The
existing [blob default](../../../record.md#durable-substrate) already separates
these roles. No new production storage subsystem is adopted by this proposal.

## Exact External Technology Scope

| Candidate bundle | Exact proposed input | Consumer boundary |
| --- | --- | --- |
| SeaweedFS 4.48 | Tag commit `530be3e37337488ecc34d58441e0bc476e121c93`; official ordinary `linux_amd64.tar.gz`, published SHA256 `4a7d108384d044d95212d1342cdda9533fa55842c1c9b41f606ca3c8a9561124` | Master, volume server, filer and S3 gateway, with their packaged consensus, index, transport and other linked dependencies. PostgreSQL 16.15 filer metadata is a new site for the already approved test database. |
| OpenBao 2.7.1 | Official `openbao_2.7.1_linux_amd64.tar.gz`, published SHA256 `0e2f1ce10d124e03112b50dd2fbec6b78003783253bc3a91587938f39d1e2243`; accompanying SBOM SHA256 `96e64e2d8d81158c7e385fbb2d52cc31c5736b3782c2a583027e0804ed977c31` | Packaged server/CLI, built-in Transit, token/policy/audit and explicitly configured bundled Raft storage, including their linked dependencies. Synthetic fixture only: built-in Shamir seal and aes256-gcm96 Transit keys. No external HSM/KMS/auth/plugin service. |
| Existing harness dependencies | System Python 3.12.3 and standard library; already approved PostgreSQL 16.15 qualification installation | HTTP/serialization/hashing and disposable fixtures. No new client SDK, container platform, database driver or production service. |

The listed artifact hashes come from official release metadata, not a downloaded
binary inspection. Before any launch, the experiment records the acquired package
hash, executable identity, published SBOM/build data, linked native dependencies,
and effective configuration. A mismatch or unexpected external runtime dependency
halts that experiment rather than silently substituting a package.

SeaweedFS's tagged manifest declares pgx/v5 5.10.0, gRPC
1.85.0-dev.0.20260915183914-4e49413dcab7, protobuf 1.36.12 and Go 1.26.6. Its source
manifest is broader than the actual linked closure. The requested bundle includes
its packaged dependencies; it does not authorize separately installing those
libraries or a Go compiler. PostgreSQL registration is not restricted to the full
SeaweedFS build. The fixture explicitly checks that PostgreSQL is the active filer
backend and refuses an embedded-store fallback.

Sources: [SeaweedFS release](https://github.com/seaweedfs/seaweedfs/releases/tag/4.48),
[asset metadata](https://github.com/seaweedfs/seaweedfs/releases/expanded_assets/4.48),
[dependency manifest](https://github.com/seaweedfs/seaweedfs/blob/4.48/go.mod),
[PostgreSQL registration](https://github.com/seaweedfs/seaweedfs/blob/4.48/weed/command/imports.go#L26),
[OpenBao release](https://github.com/openbao/openbao/releases/tag/v2.7.1), and
[OpenBao asset/SBOM metadata](https://github.com/openbao/openbao/releases/expanded_assets/v2.7.1).

## Object-Store Qualification

SeaweedFS is the recommended first candidate because it supplies the service
interface and supports a PostgreSQL metadata backend already available in the
approved test stack. It adds several server roles and has more operating surface
than Garage. Garage 2.4.1 is the simpler comparison candidate, but its documented
metadata-sync choices leave an unresolved acknowledged-write durability concern;
it is not recommended ahead of a candidate that can pass the durability probes.
Neither is production-selected.

SeaweedFS's upload path propagates an assigned fsync flag. PostgreSQL durability
alone says nothing about volume-byte durability. Verify the actual volume rule,
upload propagation and database fsync/commit settings. Conditional PUTs are checked
within object-write locking/routing, but concurrent gateways and interrupted
writes still need execution evidence.
[Upload path](https://github.com/seaweedfs/seaweedfs/blob/4.48/weed/operation/upload_content.go#L183),
[PUT implementation](https://github.com/seaweedfs/seaweedfs/blob/4.48/weed/s3api/s3api_object_handlers_put.go#L343),
[Garage configuration](https://github.com/deuxfleurs-org/garage/blob/v2.4.1/doc/book/reference-manual/configuration.md#metadata_fsync).

| Probe | Observable acceptance or failure |
| --- | --- |
| O1: immutable retry | Concurrent create-only writes, lost replies and duplicate retries leave one immutable ciphertext value. A different body under its claimed ciphertext hash cannot overwrite or be accepted as valid. |
| O2: visibility and error identity | Read immediately through another gateway and after gateway/filer restart. Preserve absent versus unavailable versus forbidden; a backend permission failure raises the original internal-error/alarm path rather than becoming not found. |
| O3: data/metadata interruption | Interrupt before/after volume acknowledgment and metadata commit. No acknowledged referenced object becomes an accepted dangling reference. Observe actual ordering, not only a successful final PUT. |
| O4: corruption and restore | Truncate/corrupt disposable volume/cache bytes and refuse invalid content. Restore matching metadata and volumes into fresh directories; record which medium survived. Process restart is not power-loss or all-medium-loss proof. |
| O5: owner/reference boundary | Compose the actual ownership/pin plans with uploads and synthetic key availability. Race new references against owner deletion and refuse referenced reclamation. Keep this separate from real key-service erasure qualification. |

The acceptance comes from [immutable stored blobs](../../../contracts/stored-form.md#blobs),
[owner and pin rules](../../../core/encryption-and-blobs.md#the-blob-store), and
[key destruction](../../../core/encryption-and-blobs.md#deletion-is-key-destruction).
The vendor server itself is not running inside Bach. The owned adapter and its
simulator need the same contract assertions exercised against this real service;
passing a protocol smoke test is insufficient.

## Key-Service Qualification

OpenBao Transit provides useful key wrapping, context-derived and versioned-key
operations. Its API also supports backup/restore. A recoverable pre-deletion
snapshot may reintroduce deleted key material; that is a source-based inference
for the first probe, not an executed finding here. Stock soft deletion or a policy
tombstone cannot be counted as irreversible erasure. Disabling a decryption API
does not prove that retained key bytes cannot decrypt offline.
[Transit API](https://openbao.org/docs/api/secret/transit/),
[snapshot commands](https://openbao.org/docs/commands/operator/raft/).

Stock Transit plus recoverable storage is not a conforming complete Borg
erasure/custody solution as-is. The recommended experiment is useful to establish
that boundary concretely and assess which primitives can be reused. If ordinary
recoverable storage fails the erasure gate, record the failure and stop that
production claim. Do not introduce a new
hardware service, deletion exception, root-custody arrangement or wrapper protocol
to hide it. An integrated service using reviewed cryptographic building blocks is
an alternative, but adds erasure/recovery/custody work rather than avoiding it.

| Probe | Observable acceptance or failure |
| --- | --- |
| K1: owner/root erasure and old recovery | With export/backup flags disabled, wrap synthetic keys for two owners, preserve a disposable pre-delete storage snapshot, delete one owner and restore old storage into a separate fixture. Attempt unwrap/decrypt with retained recovery material. Any readable deleted-owner copy fails Borg's erasure contract, even if the current service refuses it; the unaffected owner is a positive control. |
| K2: backup/export controls | Confirm use identities cannot enable plaintext backup/export, change policy or restore keys. Separately create a deliberately exportable synthetic key with plaintext backup enabled, preserve its backup, delete it and restore under a fresh identity/instance. A denied current endpoint does not discharge old-copy destruction. |
| K3: administration versus use | Test separate deploy/admin/use identities and policy changes that could grant decryption. Record privileged recovery/initialization powers explicitly. Local token ACL separation alone does not prove the infrastructure-deployer boundary. |
| K4: primitive binding and separate client obligations | Wrong synthetic context/authenticated data is refused by the selected Transit primitive. This proves only primitive binding. Borg's authenticated tenant/owner/range fields, trusted key-label resolution and plaintext-cache expiry remain separate adapter obligations, unqualified until their actual implementations are exercised. Live key/version refusal does not prove cached data-key erasure. |
| K5: restart and bounded failure | Observe request loss, duplicate retry, service restart, unavailable key and delayed resolution. Fail closed; exercise bounded wait/hedge and paced resumes without invoking a real customer key. |

These cases follow [root administration separation](../../../core/tenancy-and-confidentiality.md#root-key-administration-is-separated),
[wrapped-key recovery](../../../contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys),
[key-service client limits](../../../core/encryption-and-blobs.md#the-key-service-stays-off-the-critical-path),
and [deletion across all copies](../../../core/encryption-and-blobs.md#deletion-is-key-destruction).
The experiment selects no production cipher or serialized envelope. The synthetic
Shamir/aes256-gcm96 configuration is confined to the disposable service fixture;
it is not the Borg envelope profile or blob encryption mode. Fixture keys
and tokens are local test data, not new authority over any existing service.

The service's own key cache, an already returned plaintext data key, and Borg's
client cache are different copies. Raising a minimum accepted key version is API
access control, not proof of destruction. Retaining a derivation parent and context
also does not supply independently erasable owner keys. A deployment-controlling
identity is not proven unable to use keys merely because two API tokens have
different policies. The real custody, client-cache and human-administration
boundaries remain original acceptance gates.
[Pinned deletion source](https://github.com/openbao/openbao/blob/v2.7.1/sdk/helper/keysutil/lock_manager.go#L494),
[security model](https://openbao.org/docs/internals/security/#out-of-scope),
[root policy](https://openbao.org/docs/concepts/policies/#root-policy).

## Proposed Approval Wording

The two decisions can be approved independently:

1. Approve the exact SeaweedFS release bundle above for private, disposable object
   service qualification using the already approved PostgreSQL/Python stack and
   synthetic ciphertext, with acquisition/closure identities recorded before
   execution. Production backend and deployment remain unselected.
2. Approve the exact OpenBao release bundle above for private, disposable
   key-service qualification using synthetic keys/tokens and isolated snapshots,
   including deliberate old-snapshot recovery and authority-separation negatives.
   This is not approval of a production custody or deletion mechanism.

For either decision, the alternatives are to adjust its bounded evaluation scope
or request the specific missing evidence before approving it. Independent owned
component work continues. No waiting period implicitly supplies the direct
technology approval required by the implementation specification.

## Scope And Evidence

No server has been acquired, installed or launched for this packet. No shared
source, canonical role, gate selection, actual runtime lock/snapshot, access grant
or existing service changes. Qualification results will have their own source,
executable, command, configuration, assertion and cleanup receipts. Native Duvet
links, executed assertions, simulator parity and trusted conformance stay distinct.

The independent store-layout choice is described separately in
[Durable Store Mapping](durable-store-mapping.md); approving either technology
experiment does not adopt that representation or its ownership-transfer protocol.
