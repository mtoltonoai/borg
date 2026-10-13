# Assertion Signing And Local Verification Qualification

Status: private qualification selected on 2026-10-12; see [the decision record](../decisions.md#assertion-signature-qualification) and [the selected consumption scope](../technology-inventory.md#assertion-signature-qualification-bundle). The pre-approval proposal is preserved in the decision checkpoint. This extends the existing verifier and service selections to the bounded application-signature fixture below. It selects no production assertion format or signature scheme and is not evidence that a cryptographic probe has passed.

## Recommendation And Narrow Scope

Qualify the already materialized aws-lc-rs1.18.1 verifier with the already selected OpenBao2.7.1 service, using a disposable plain Ed25519 primitive fixture. The service retains the private key; the Rust verifier receives only a public key and exact message/signature bytes. This first experiment establishes interoperability and separation of signing from verification. It does not define a Borg token or release an authenticated runtime caller.

This reuses the active TLS cryptographic implementation rather than introducing a second active provider. The serious alternative is ring0.17.14 with the same service-held signer. Its source already exists in the transport lock, but there is no demonstrated advantage here sufficient to favor that additional active consumption path. Both alternatives need their direct assertion use recorded before execution.

Plain Ed25519 is proposed for the disposable primitive fixture because it avoids RSA-PSS salt-length and ECDSA signature-format parameter disagreement. This is not an inference that every supported service/library combination interoperates. OpenBao documents maximum-length RSA-PSS salt by default; AWS-LC and ring require hash-length salt. An eventual production scheme and exact assertion encoding remain separate decisions supported by the qualification result.

## Inputs And Consumption Boundaries

| Input | Exact existing subject and proposed use |
| --- | --- |
| Rust verifier | aws-lc-rs1.18.1 with aws-lc-sys0.45.0; retain the already materialized `aws-lc-sys,prebuilt-nasm` feature profile and its recorded native build inputs. A new private `qualification/assertion-signature` consumer would use its fixed-algorithm byte verification API. |
| Dependency provenance | Existing transport lock SHA256 `31c563817acb93575f29b27c646f706e45fc751fc746a1ab0bd9203d9c1484ff` and [exact package records](/home/dev-user/borg-runtime/qualification/native-runtime/artifacts/responses-transport-research/resolution151-h2-ilda5z3k/locked-package-records.json). This is the containing inventory, not a claim that all 162 records compile in the proposed verifier profile. Resolve and record the actual existing subset/features before any execution; additions are outside this packet. |
| Signing service | Existing OpenBao2.7.1 official Linux bundle, archive SHA256 `0e2f1ce10d124e03112b50dd2fbec6b78003783253bc3a91587938f39d1e2243`, SBOM SHA256 `96e64e2d8d81158c7e385fbb2d52cc31c5736b3782c2a583027e0804ed977c31`. Built-in Transit, disposable non-derived Ed25519 key, synthetic private fixture only. No plugin, external custody service or real tenant key. |
| Fixture profile | Exact bounded message bytes, one raw 32-byte public-key representation and one 64-byte signature representation at the verifier boundary. Service response decoding and its version prefix are measured adapter behavior, not a proposed Borg token encoding. No prehash mode or algorithm negotiation. |
| Test supervision | The existing system Python3.12.3 tooling may supervise the disposable service and provide vectors. Owned verification/integration logic remains Rust. No maintained Python application path is introduced. |

The complete acquired binary and dependency/build manifests remain those in the [technology inventory](../technology-inventory.md#openbao-qualification-bundle). The proposal requests a new bounded use of them, not an upgrade. The service's exact asymmetric behavior has not been executed against the selected binary. Current documentation is evidence for screening, not proof of that behavior.

## Concrete Qualification Cases

1. The private service signs an exact bounded synthetic message. A public-key-only local verifier accepts that original message and signature while the signing service is unavailable to the verifier. Capture the exact service/version, public representation, library build and byte vectors.
2. Alter one message byte, signature byte or public-key byte independently. Reject every case, as well as absent, truncated, overlong and wrong-family signatures/keys. Reject an SPKI key at the fixture's raw-only boundary even if the library itself accepts both forms.
3. Reject accidental prehash mode, malformed service version prefixes and mismatched service-key-version/public-key associations. Do not reinterpret the service key version as Borg's signing lineage or key identity.
4. A synthetic signing-only credential cannot administer keys. The verifier has neither signing material nor signing-service credentials. Service unavailability produces no assertion; an unknown signing outcome gives no permission to execute an effect.
5. Preserve exact passing and failing vectors, commands, executables, source hashes and cleanup receipts. Distinguish primitive interoperability from later authenticated assertion, current grant, fence and effect-admission checks.

No real credentials or provider request are involved. No production role, access grant, network service or key-administration policy is changed by this proposal. Private fixture credentials, if the experiment is authorized, are disposable test data within its isolated service.

## Runtime Contract Still To Implement

The existing [signed-assertion contract](../../../contracts/signed-assertion.md#the-signature-is-asymmetric) requires asymmetric signing, local verification and claims carried only inside the signed payload. [Verification](../../../contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match) checks the received signature before decoding any claim, accepts one scheme and encoding, and binds the request's tenant, audience, call, lease and time constraints. Bounded framing may locate payload/signature/key hints, but it cannot use unauthenticated identity or authority claims first. A second logical claim object is not authentication evidence.

The actual receiving service verifies its own assertion and applies [call scoping](../../../contracts/signed-assertion.md#the-assertion-scopes-the-call) before its authorization. Signature validity never replaces current durable grants, fence/receipt handling or durable-before-effect admission. The current `LogicalVerifiedAssertion` and `ReceivingAssertionValidator` inputs expose this missing producer; their existing logical tests do not prove a wire signature was verified.

The [key-publication rules](../../../contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap) require lineage-bound publication before issuance, overlap until outstanding assertions expire, authenticated bounded refresh, all-source atomic replacement and fail-closed expiry. The [declared identity defaults](../../../record.md#identity) remain: 300-second maximum assertion lifetime, 60-second issue-time allowance with none on expiry, node-key rotation within six hours, 60-second key-cache age and 90-second publication lead. This proposal changes none of them or the separation of signing-root administration from deployment authority.

After a format and use scope are selected, owned framing, time, key-cache and caller-binding logic need Bach tests and the same expectations against real primitives/services. Wrong audience, tenant, call, method/target/body digest, ordered chain, invalid lineage and expiry must not produce authenticated caller facts or effects. A presented lease that differs from the signed claim fails verification. A superseded generation follows the existing admission, close, receipt and reconcile rules; it does not make a cryptographically valid assertion invalid or erase the existing-receipt and unfenced-reconcile behavior. Streamed-body checks precede body reads and the final digest precedes commit. Those are later actual integration gates, not claims established by the primitive fixture.

## Review Evidence And Remaining Choices

[Source review receipt](/home/dev-user/borg-runtime/artifacts/execution/assertion-signature-research-1slctylg/research-receipt.json), SHA256 `de072cadf0248fb36e419c95456f3da134b53a05e9c9dff8aa0452c6efda178d`, binds the current owned seams, original requirements and materialized dependency source. Primary sources: [AWS-LC1.18.1 signature API](https://docs.rs/aws-lc-rs/1.18.1/aws_lc_rs/signature/index.html), [ring0.17.14 signature API](https://docs.rs/ring/0.17.14/ring/signature/), [OpenBao Transit API](https://openbao.org/docs/api/secret/transit/).

The scoped approval covers this disposable signing/local-verification qualification and its exact consumption sites. Production token/field encoding, signature scheme, key publication/rotation adapter and administrative custody integration remain separate decisions. The settled v1 backup-key-retention exception is unchanged. No new question is sent by maintaining this packet.
