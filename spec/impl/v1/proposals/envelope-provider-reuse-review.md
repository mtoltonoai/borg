# Envelope Provider Reuse Review

Review option only. The original [envelope qualification inputs](envelope-integration-qualification.md) are now selected for private qualification. This AWS-LC alternative remains unselected; it does not replace that bundle, authorize an envelope consumption of AWS-LC, select a production cipher or alter the key hierarchy.

## Recommendation

Consider reusing the installed AWS-LC provider for mutable encryption, key derivation and hashing. Retain the separately reviewed CMAC-SIV provider for the original immutable-blob and key-wrapper hypothesis. This could remove three requested top-level RustCrypto libraries without inventing a cryptographic construction.

The exact reduced dependency closure has not been resolved. Do not describe three fewer roots as three fewer transitive packages or as an executed integration. The selected original bundle and its pinned inventory remain the basis for qualification; this unselected alternative is not a silent replacement.

## Verified Existing Inputs

The installed signature profile uses `aws-lc-rs` 1.18.1 and `aws-lc-sys` 0.45.0. Default features are disabled; selected features are `aws-lc-sys` and `prebuilt-nasm`. Source inspection found these public APIs in that profile, without additional algorithm features:

| Original proposed use | Existing API | Result |
| --- | --- | --- |
| AES-256-GCM mutable encryption | `aead::AES_256_GCM`, explicit nonce and in-place seal/open APIs | Available for a proposed envelope consumer; that use is not yet approved. |
| HKDF-SHA256 derivation | `hkdf::HKDF_SHA256`, `Prk::expand`, `Okm::fill` | Available; context encoding and output bounds still need qualification. |
| SHA-256 ciphertext addresses and derivation inputs | `digest::SHA256` | Available; Borg identity and binding rules remain owned behavior. |
| Deterministic AES-256-CMAC-SIV | No equivalent public AEAD | The original separately reviewed provider remains necessary for this hypothesis. |

The installed `zeroize` version is 1.9.0. The open request specifies 1.9.1. These are different supplied identities; this review neither upgrades nor substitutes either version. Provider-internal key cleanup does not establish that all caller-owned key buffers are cleared.

## Boundaries That Stay Unchanged

AWS-LC's AES-256-GCM-SIV is a different construction from AES-CMAC-SIV. The former uses a 32-byte key and 12-byte nonce. AES-256-CMAC-SIV uses two 256-bit subkeys, totaling 64 key bytes. Public CMAC alone is a MAC primitive, not a complete SIV encryption implementation. No new cipher is to be assembled from those primitives to avoid a provider review.

RFC 8452 recommends random nonces and discourages intentional nonce reuse. Its misuse resistance does not justify a fixed-nonce substitution for the proposed immutable format. Such a profile would need its own analysis, limits and test vectors; it is not selected here. [RFC 8452, section 9](https://www.rfc-editor.org/rfc/rfc8452.html#section-9), [RFC 5297, section 2.6](https://www.rfc-editor.org/rfc/rfc5297.html#section-2.6).

The original nonce reservation, injected entropy, lost-commit handling, exact retry bytes, tenant/owner/purpose binding and live-key deletion requirements remain. The approved [backup-key retention exception](../implementation.md#backup-key-retention) also remains. No key-service, state-layout or recovery-policy choice changes.

## Additional Qualification Cases For Reuse

- A failed in-place open can modify its buffer. Return no plaintext and clear affected owned buffers on every error path.
- HKDF concatenates its information slices. Distinct structured contexts need distinct encoded bytes; separate slice boundaries alone do not establish domain separation.
- In-place payload APIs do not prove absence of internal allocation. Check both provider and owned allocation behavior against the actual component bounds.
- Use the explicit nonce boundary under the original injection and reservation rules. Do not introduce a provider-generated nonce path without review.
- Run the original envelope, key-lifecycle, compatibility, restart and simulator/probe cases. A successful signature or TLS test is not envelope evidence.

## Evidence

The [independent source review](/home/dev-user/borg-runtime/artifacts/execution/envelope-aws-lc-reuse-elqm8swk/review.json), SHA256 `29ff4626f313bc0fef38746f597804f3d7f1eec9d425870dd7e5b9311da6813e`, verifies three installed archive checksums and eleven inspected files against their archives. It captures sixteen inputs without drift. No package was acquired, built or executed by this review.

Primary API references: [AWS-LC AEAD](https://docs.rs/aws-lc-rs/1.18.1/aws_lc_rs/aead/index.html), [AWS-LC HKDF](https://docs.rs/aws-lc-rs/1.18.1/aws_lc_rs/hkdf/index.html).

The original request's [candidate inventory](/home/dev-user/borg-runtime/qualification/key-service/artifacts/next-envelope-proposal-20261012/candidate-dependencies.json) remains pinned at SHA256 `01697f2e75ce20c1d5bfe73cd77b4a398fbb0115fd491eddbed9029fc32f40e9`. Its 28 package identities are a conservative metadata inventory, not the reduced graph or an executed Cargo resolution.
