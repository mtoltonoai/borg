# Storage Service Tradeoffs

Descriptive review response, 2026-10-12. No production product is adopted by this
comparison. Matthew asked why SeaweedFS rather than a more established or
simpler alternative, specifically Ceph and Kubo, and requested the key-service
alternatives. The prior qualification questions received requests for detail,
not approval. Their exact wording and the questioned proposal are preserved in
the runtime storage-qualification-review-vum0oi1f evidence directory.

Subsequent update: Matthew accepted OpenBao as the reuse candidate. Its exact
private qualification scope is now in the main specification and
[decision record](../decisions.md#openbao-qualification-choice). The production
custody/erasure design and object backend remain unselected. This later selection
does not relabel the comparison's source review as executed evidence.

## What The First Recommendation Optimized

The first object-store comparison covered SeaweedFS and Garage and favored a
convenient private qualification with the already approved PostgreSQL fixture.
That did not establish SeaweedFS as more hardened or justify excluding Ceph.
Source review now makes Ceph RGW the leading durability-focused candidate to
evaluate, conditional on operating its storage cluster. SeaweedFS remains a
lighter experiment. Neither conclusion is a measured operating-cost estimate or
a completed Borg qualification.

## Object Backends

| Candidate | Advantage | Cost and remaining Borg work |
| --- | --- | --- |
| Ceph RGW, current supported Tentacle 20.2.4 | Explicit supported-release process; S3 object interface over RADOS, read-after-write consistency, atomic conditional creation, replicated commit and recovery mechanisms. | A real storage cluster: monitors, managers, OSD/BlueStore and gateway. No CephFS/MDS or PostgreSQL is needed for this object interface. Need correct device/failure-domain placement, degraded-write policy, data/metadata pools, restore and Borg adapter qualification. |
| SeaweedFS 4.48 | Multiple roles from one executable; object interface and a PostgreSQL filer backend reusable in the existing private test environment. | Still master, volume, filer and gateway roles plus the proposed PostgreSQL backend. Volume-byte durability and metadata durability need joint proof. The reviewed source has conditional-write handling, but no equivalent supported-version lifetime was established. |
| Kubo 0.43.1 | Direct immutable content addressing; a single local daemon is comparatively small; peer-to-peer exchange if that is actually useful. | Local pins prevent local GC, not loss of all storage replicas. Replication/repair needs IPFS Cluster, Borg-owned coordination or durable underlying storage. CID representation, private networking, tenant/owner authorization and recovery require explicit integration. |
| Garage 2.4.1 | Smaller self-contained S3 alternative screened before the first recommendation. | Documented fsync choices leave an unresolved acknowledged-write durability concern. A simple deployment does not waive that acceptance gate. |

Ceph's documented consistency and commit paths make it worth evaluating first
when storage recovery and maturity outweigh the operating burden. Actual safety
still depends on the selected pool and device configuration. A replica count of
three is not three surviving copies after every degraded write; multisite
replication is asynchronous. A single-host trial proves no independent host/media
failure domain. No backend survives destruction of every copy without an
independently surviving medium.

Sources: [supported Ceph releases](https://docs.ceph.com/en/latest/releases/),
[Ceph architecture](https://docs.ceph.com/en/tentacle/architecture/),
[RGW consistency](https://docs.ceph.com/en/tentacle/dev/radosgw/bucket_index/#consistency-guarantee),
[20.2.4 write path](https://github.com/ceph/ceph/blob/v20.2.4/src/rgw/driver/rados/rgw_rados.cc#L3058),
[replicated commit](https://github.com/ceph/ceph/blob/v20.2.4/src/osd/ReplicatedBackend.cc#L517),
[deployment](https://docs.ceph.com/en/tentacle/cephadm/install/),
[SeaweedFS release](https://github.com/seaweedfs/seaweedfs/releases/tag/4.48),
[Garage settings](https://github.com/deuxfleurs-org/garage/blob/v2.4.1/doc/book/reference-manual/configuration.md#metadata_fsync).

### Kubo Is Viable, But Not A Drop-In Answer

Normal UnixFS import chunks a file into a DAG; its root CID is generally not the
hash of the original ciphertext bytes. A raw block can carry that digest directly,
but standard Kubo rejects blocks above 2 MiB unless the caller permits blocks that
normal Bitswap cannot transfer. Borg's current 8 MiB segments therefore need a
reviewed mapping/chunking adapter or a format/default decision; silently changing
Borg's ciphertext-digest identity is not an option.
[CID semantics](https://docs.ipfs.tech/concepts/content-addressing/),
[current RPC reference](https://docs.ipfs.tech/reference/kubo/rpc/#api-v0-block-put).

Kubo has local durability mechanisms; it is inaccurate to say it has no durable
writes. It also has current Basic/Bearer API authorization and allowed endpoint
prefixes. Neither provides Borg's owner/tenant/pin policy automatically. S3 is not
a Borg requirement, so a direct Kubo adapter is possible without an S3 facade.
[Datastore behavior](https://github.com/ipfs/kubo/blob/v0.43.1/docs/datastores.md),
[API authorization](https://github.com/ipfs/kubo/blob/v0.43.1/docs/config.md#apiauthorizations).

IPFS Cluster is an additional dependency and retained-state system. Accepting a
pin intention precedes asynchronous pinning; its wait option waits for at least
one peer. Requested replication therefore is not proof of the required completed
copies. Cluster pinset backups do not contain Kubo's content bytes. A private
network also needs explicit discovery/network controls; no public replication
or confidentiality should be inferred from encrypting the payload alone.
[Persistence](https://docs.ipfs.tech/concepts/persistence/),
[Cluster pinning](https://ipfscluster.io/documentation/guides/pinning/),
[Cluster backups](https://ipfscluster.io/documentation/guides/backups/),
[private-network configuration](https://github.com/ipfs/kubo/blob/v0.43.1/docs/experimental-features.md#private-networks).

Recommendation: retain Kubo if peer-to-peer exchange, independent replica
operators or IPFS interoperability becomes a real requirement. For the present
centralized dedicated deployment, conventional object storage has a more direct
fit. This is an integration/operations judgment, not a claim of a Kubo defect.

## Key Services: What Was Actually Reviewed

OpenBao 2.7.1 received source/API/recovery review. AWS, Google and Azure services
were screened through official documentation; no cloud account/service was
configured or probed. A Borg-built key service and an HSM-backed custody boundary
are architectural alternatives, not selected libraries or hardware.

| Option | Benefit | Main tradeoff |
| --- | --- | --- |
| OpenBao Transit | Reuse key APIs, wrapping, versioning, ACLs and audit in a self-operated service. | We own availability, patching and recovery. Recoverable snapshots can restore deleted keys, and token separation does not exclude a deployment/process controller. It is not a complete conforming custody solution as-is. |
| Ordinary AWS KMS | Managed non-exportable key custody and less key-service operation. | New cloud dependency, IAM/policy boundary, quotas and per-key/request cost. Final deletion follows a waiting window; default administrators can grant usage, so defaults do not establish Borg's separation. |
| Google Cloud KMS / Cloud HSM | Managed operations and administration/use roles; software/HSM choices. | Documented destruction can take up to 45 days after scheduled destruction across infrastructure/backups; reconcile exact destruction timing with Borg before claiming erasure. |
| Azure Key Vault / Managed HSM | Managed service; Managed HSM separates control-plane infrastructure from data-plane key access. | Earlier backup blobs/restored copies are independent of deleting the original key. Recovery and role-assignment powers remain material gates. |
| Borg-integrated service using existing crypto | Exact control of the owner lifecycle and interface. | Largest owned burden: authentication, storage, replication, recovery, upgrades, audit, custody and erasure. A cipher library solves only the cryptographic primitive. |
| HSM-backed service | Potentially separates key material from ordinary process memory and strengthens custody. | Additional hardware/service, operations and backup/recovery design. It can underpin another service; it is not a competing API by itself. Old HSM backups can still defeat erasure. |

Sources: [OpenBao security model](https://openbao.org/docs/internals/security/),
[Transit](https://openbao.org/docs/secrets/transit/),
[AWS protection](https://docs.aws.amazon.com/kms/latest/developerguide/data-protection.html),
[AWS deletion](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html),
[AWS administrator powers](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-default.html),
[Google destruction](https://docs.cloud.google.com/kms/docs/destroy-restore),
[Azure backup semantics](https://learn.microsoft.com/en-us/azure/key-vault/general/backup),
[Managed HSM roles](https://learn.microsoft.com/en-us/azure/key-vault/managed-hsm/access-control).

### The Hard Requirement Is Custody And Erasure, Not The API

Borg requires destroying an owner's key to make that owner's data unreadable
through retained backups too. A live tenant root plus recoverable wrapped owner
keys cannot discharge that requirement merely because a service currently refuses
the request. Cached plaintext data keys also remain Borg's responsibility. The
platform-deployment identity and key-administration identity must not acquire use
authority through policy changes or recovery. Those requirements apply whether
the service is open source, managed, or built by us.
[Owner erasure](../../../core/encryption-and-blobs.md#deletion-is-key-destruction),
[administration separation](../../../core/tenancy-and-confidentiality.md#root-key-administration-is-separated).

Per-owner managed keys may make recurring cost material; using fewer parent keys
can reduce calls/cost but cannot quietly give up independent owner erasure. No
peak owner count or production cost has been assumed here.

Recommendation: treat OpenBao as a possible reusable key-management component,
not the chosen security boundary. Keep ordinary AWS KMS as a managed comparison
if a new cloud service is acceptable, and establish the concrete owner-erasure and
custody design before substantial integration. Do not build an entire key service
until existing options fail a concrete requirement, and do not claim an HSM alone
fixes backup erasure. The original proposed OpenBao experiment remains a small way
to establish its limits, not a hidden production adoption.

## Next Work And Decision Status

No immediate new confirmation is requested by this review response. The next
operator decision should compare the actual operating profile and requirements,
not approve a package solely because a local test is convenient. Existing owned
logic and approved PostgreSQL qualification continue independently. All products
in this comparison remain unapproved for new consumption unless separately
recorded; no acquired binary, deployment or access change follows from this file.
