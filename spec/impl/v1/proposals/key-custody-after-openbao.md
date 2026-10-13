# Key Custody After OpenBao Qualification

Proposed design direction, not an adopted custody mechanism, plugin, cloud account
or hardware product. OpenBao 2.7.1 remains the selected private qualification
candidate. Its completed characterization is recorded separately in the runtime
key-service qualification summary.

Current status: deferred after the v1 [Backup Key Retention](../implementation.md#backup-key-retention) decision. Deleting keys from retained backups is not a v1 acceptance requirement. The analysis below concerns a possible future irreversible-backup-erasure capability; it is not a current custody-choice request or an adopted design. Live deletion, authority separation and cache bounds still apply. Historical failed original-erasure results remain unchanged.

## Observed Constraint

The actual K1 test deleted an owner key with export and plaintext backup disabled.
The live service then refused decryption. Restoring an earlier Raft snapshot into
a separate fixture with retained recovery material recovered that same owner's
data key. K2 likewise restored explicitly backed-up key material. The untouched
owner remained readable as a positive control. These are failed erasure
properties, not successful custody qualification.

After an owner is destroyed, no retained recovery combination may reconstruct a
complete decryption path for that owner. At least one indispensable owner-specific
secret must be irrecoverable, or independently enforced destruction must prevent
its use even through recovery. A reversible deny policy does not meet that
condition. This is an inference from the original
[backup requirement](../../../contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys)
and [owner-key destruction requirements](../../../core/encryption-and-blobs.md#deletion-is-key-destruction),
not a proposed weakening of them.

## Recommended Candidate

Keep OpenBao's useful management/API/policy functions and qualify an independently
destructible owner wrapping key outside OpenBao's recoverable storage. Every
resource decryption path would need both the required tenant-root hierarchy and
that owner's destruction key. A tenant ACL or context label cannot replace the
cryptographic tenant-root dependency. Old OpenBao snapshots could retain wrapped
keys and stable references, but could not recreate the indispensable owner key.

This is not yet an envelope format or implementation. The exact provider must
prove destruction over versions, replicas, backup/restore paths and administrative
recovery. It needs a separately authorized provisioning/destruction path. One
owner key may protect the owner's resource keys, but cannot silently omit episode
copies, pending inbox content or historical wraps that deletion covers. The
resource-carried key chain, bounded key-service calls, epoch rewrap and five-minute
decrypt-cache window remain applicable.

The operating choices are:

| Custody setting | Benefit | What remains to decide and prove |
| --- | --- | --- |
| Managed cloud custody under a separate administrative identity | Avoids building a physical key-custody system; a plausible first direction if cloud services are allowed. | Exact provider, per-owner scale/cost, deletion timing, backup behavior, least-privilege use/admin/deploy roles and a supported OpenBao integration. No cloud account or SDK is selected here. |
| Self-operated hardware custody | Keeps the custody boundary under our operation and can prevent ordinary extraction. | Actual hardware/plugin, independent administrators, backup and replica erasure, capacity, availability and recovery. An HSM label alone proves none of these. |
| Software custody-root migration | Can preserve survivor keys under a fresh root before destroying the old recovery domain. | Must migrate every surviving required key/version and actually destroy every old root/share/snapshot copy. Work grows with survivor history; it still needs a trustworthy erasure boundary. This is not the simpler recommended first path. |

If this future capability is reopened, one environment decision will be whether to use managed cloud custody or self-operated infrastructure. That decision is deferred; it is not required to proceed under the current v1 backup-retention exception.

## OpenBao Does Not Supply This Automatically

Auto-unseal protects the service-wide root. Keeping its external key allows old
snapshots to open; destroying it affects unaffected owners too. It is not selective
owner erasure.

OpenBao's external-key support can delegate encrypt/decrypt to a referenced key,
but deleting the OpenBao reference does not destroy the provider key. The pinned
external interface has no generate/destroy operation. The built-in remote Transit
backend merely transfers the same recovery problem to another ordinary server.
PKCS#11 support adds an external plugin and vendor/HSM dependencies that are not
covered by the current qualification approval.

Pinned source also indicates that the tested datakey/rewrap workflow cannot be
assumed to work unchanged for external keys: those paths do not provide the
external-key factory used by regular encrypt/decrypt. That is source evidence,
not an executed external-backend result. External keys do not automatically
provide the tenant-root hierarchy or Borg's deterministic blob mode.
[Seal behavior](https://github.com/openbao/openbao/blob/v2.7.1/website/content/docs/concepts/seal.mdx),
[external registry](https://github.com/openbao/openbao/blob/v2.7.1/internal/vault/external_keys/registry.go),
[KMS interface](https://github.com/openbao/go-kms-wrapping/blob/v2.9.0/kms/kms.go),
[PKCS#11 requirements](https://github.com/openbao/openbao/blob/v2.7.1/website/content/docs/api/system/external-keys/plugins/pkcs11.mdx),
[datakey path](https://github.com/openbao/openbao/blob/v2.7.1/internal/builtin/logical/transit/path_datakey.go).

K3 also demonstrated that an unrestricted service root can widen policy and then
decrypt. Separate ordinary tokens are useful, but do not prove that infrastructure
deployment, key administration or recovery control cannot acquire key use. The
original [role separation](../../../core/tenancy-and-confidentiality.md#root-key-administration-is-separated)
remains a required boundary outside mere token labeling.

## Decisive Qualification For The Next Candidate

Restore every pre-deletion layer, including provider recovery where available;
the deleted owner remains unreadable while survivor controls work. Exercise old
versions, aliases, alternate restores and mandatory episode/inbox copies. Interrupt
destruction and lose replies; resolve the real outcome before claiming completion.
Warm caches and isolate/restart clients; separately establish cache and plaintext
copy bounds. Try direct use, policy escalation and legitimate recovery under each
deploy/admin/use identity. No next provider is accepted solely because its API
advertises non-exportable keys.

The current record already sets decrypt-cache lifetime to five minutes. Root-key
destruction waiting time and plaintext-copy destruction bounds remain declared
parameters, not numbers invented by this proposal. User approval of a specific
technology, custody arrangement and those material parameters precedes new use.
