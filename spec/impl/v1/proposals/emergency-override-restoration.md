# Emergency Override Restoration

Status: the recommended meaning is selected in [the v1 implementation specification](../implementation.md#emergency-override-restoration). The shared governance text is unchanged. The proposed shared edits below are historical review wording, not active shared replacements; actual implementation evidence is separate.

## Decision

When a temporary emergency policy is corrected by a second emergency override,
recommend restoring the original ordinary policy when the current override
expires. Each replacement still needs the existing directly authenticated-person
quorum and confirmation window. A new override receives its own configured expiry;
a retry does not extend that expiry.

Also recommend that a later, fully governed policy adoption through the
governance-policy tier ends earlier pending emergency restorations for that same
policy. An old expiry then cannot undo the newly adopted policy. This includes a
different policy, not just re-adoption of the exact temporary replacement; that
extension needs explicit review alongside overlapping overrides.

| Alternative | Observable result | Tradeoff |
| --- | --- | --- |
| Supersede while retaining the original restoration target: recommended | A is replaced temporarily by B, then by C. B's timer cannot displace C. C's expiry restores A. | One active override and one restoration target; a correction does not later revive the policy it corrected. Returning to B needs a new authorized action. |
| Resume an unexpired predecessor | If C expires before B's original deadline, restore B for its remaining time, then A. Expired predecessors are skipped. | Supports nested temporary policy changes, but requires a bounded restoration-stack and inactive-expiry contract that the current source does not define. |

Rejecting all overlapping overrides is the current implementation's qualification
limit, not a selected long-term product rule. It would prevent correcting an
active temporary policy through the same emergency path.

## Exact Source And Ambiguity

Source: [Two Paths Sit Outside The Pipeline](../../../core/governance.md#two-paths-sit-outside-the-pipeline),
SHA256 `d687a5a53ccb79da3fc377f816969c9ea3a23b82466a4b125bfd152be5f384a3`.
The relevant original identities are:

> A replaced policy MUST be kept as a version.

> A policy replacement made by emergency override MUST expire after the configured period unless it has been re-made through the governance-policy tier.

> At expiry the replaced policy MUST be restored with an event.

These sentences specify temporary replacement and restoration, but do not state
how a second override affects the first one's pending restoration. Naively
restoring each immediate predecessor can produce A -> B -> C -> A -> B: B's
expiry displaces C, then C's expiry resurrects B after B's deadline. Versioned
current-state checks are necessary; choosing the intended restoration target is
still a behavioral decision.

## Proposed Source Delta

Keep the existing version-retention and configured-expiry sentences unchanged.
Replace only the last quoted sentence, under the same heading, with:

> At expiry of the active emergency override, its recorded restoration version MUST be restored with an event.

Add the following atomic clauses under that heading:

> An emergency override for a policy without a pending emergency-override restoration MUST record the effective policy it replaces as its restoration version.

> An emergency override for a policy with a pending emergency-override restoration MUST inherit that restoration's recorded version.

> A superseded emergency override MUST NOT change the effective policy when its expiry is delivered.

> A successfully committed non-emergency policy adoption through the governance-policy tier MUST end every pending emergency-override restoration for the same tenant and policy.

> A policy adoption at any other tier MUST NOT end a pending emergency-override restoration for that policy.

> An emergency-override audit event MUST identify the recorded restoration version.

The last three proposed clauses make normal adoption, the existing privileged-tier
exception and the selected restoration target explicit. The audit-field addition
is part of this proposed clarification, not a claim that the existing event format
already carries it. No wire/storage encoding is selected. If adopted, the old
restoration tuple would be retired explicitly, its replacement allocated, and the
additional tuples traced; historical citations would not be rewritten as proof.

Pending restoration includes an expired override whose restoration has not yet
committed. Its expired replacement is not treated as ordinary policy just because
the restoration worker is late. It means the currently authoritative restoration
obligation, not retained history or a superseded queued expiry. Once restoration
or normal adoption commits, an old timer cannot recreate that obligation.
Non-emergency adoption here is the explicit
fully governed policy adoption operation, not an emergency operation targeting
the same tier or an incidental metadata write.

## Discriminating Acceptance

| Scenario | Recommended result |
| --- | --- |
| A -> B -> C; B's deadline arrives first | C remains effective; B's expiry cannot restore A. C's expiry restores A with its event. |
| A -> B -> C; C's deadline arrives first | Restore A. Later B expiry has no policy effect. This ordering needs legitimately different configured durations; it is not evidence for changing a live deadline. |
| A -> B -> C, then fully governed adoption D | D remains ordinary policy after either old expiry. The adoption retains the normal policy-analysis and authority checks. |
| Exact C is re-made through the governance-policy tier | C remains; earlier expiry cannot restore A or B. |
| A -> B, then normal adoption D, then a new emergency C | C restores D, not A, when it expires. |
| B expires but restoration delivery is late; emergency C arrives | Determine the effective policy/restoration state atomically. C retains A as its restoration target; expired B is never adopted as ordinary policy or resurrected later. |
| Duplicate expiry, crash/recovery or ownership change | The exact current override/revision is checked in the protected durable transition; retries do not extend authority or produce another policy transition. |
| Equal deadlines, expiry racing C or normal adoption, failed or unknown commit | The durable ordering of exact operation and revision identities determines the result. Equal policy digests do not substitute for these identities; recovery resolves an uncertain commit before another transition. |
| Current override expires while expiry delivery is delayed | Expired authority cannot be retained merely because delivery or recovery is late. Actual time/fence/consumer enforcement requires producer evidence. |

The existing quorum, directly authenticated people, exact replacement digest,
confirmation window, allowed tiers, protected checks, retained versions and alerts
to every registered operator destination remain. A fresh override can renew the
temporary period through a new authorized action; no cumulative renewal cap is
invented here. The selected checkers-only decision-hook policy is unchanged: the
existing emergency control remains its separate core-checked path.

## Evidence And Affected Work

[Source-linked decision evidence](/home/dev-user/borg-runtime/contracts/configuration/artifacts/overlap-decision-aty0trk4/decision-evidence.json),
SHA256 `88c357c353d7cf12a3a2e7385cb6158d0dc4753f881174b424b0ca6b17c73763`,
contains both alternatives and the exact source sentences. The
[current CFG checkpoint](/home/dev-user/borg-runtime/contracts/configuration/artifacts/emergency-start-ykx__c1m/completion.json)
qualifies single-active-override owned logic; it does not qualify overlap,
direct-person authentication, durable expiry delivery or real policy consumers.

This decision affects overlapping CFG emergency-policy replacement and its
expiry/adoption integration. Other configuration, hook, store and transport work
can continue. No expiry duration, quorum, scheduler, restoration encoding,
store layout, technology or production readiness is selected by this proposal.
