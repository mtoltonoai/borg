# Receipt Close Fencing Clarification

Status: the recommended meaning is selected in [the v1 implementation specification](../implementation.md#receipt-close-fencing). The shared tool-call text and actual wire/release state are unchanged. The proposed shared edits below are historical review wording, not active shared replacements. Exact source and compatibility accounting is in [the decision record](../decisions.md#five-decision-compatibility-disposition).

## Decision

An authenticated request with the correct tenant, session and call claim asks to
close an existing active call after the receiver has recorded a newer ownership
generation. The current contract requires receipt replay regardless of generation,
fences close like admission, and requires an in-flight close to end the call.
Those clauses do not explicitly distinguish answering the existing receipt from
admitting a new cancellation mutation.

Recommend retaining the recorded-state answer while distinctly refusing the stale
close's lifecycle mutation. The old owner can inspect its call but cannot end it
after the receiver observes the newer generation. Receipt state, the generation
fence, and the running effect remain unchanged by the refusal.

The runner-up returns only a distinct superseded-owner refusal; the caller obtains
state through the existing unfenced reconcile operation. That is a simpler single
response but needs a broader exception to the existing close-state and receipt-replay
requirements. Allowing the stale request to end an active call is another possible
reading of admission's existing-receipt exemption; it weakens the proposed boundary
between observing existing work and controlling its lifecycle.

## Exact Current Source

Source: [tool-call.md](../../../contracts/tool-call.md), SHA256
`9acde2335baead640e0f5c8e0beab81dfb5263c4f8b82b4f0393c58bde53fa43`.

| Stable section | Exact sentence |
|---|---|
| A Service Dedupes By Call Id | A service the platform's operators run MUST answer a close for a call id with the call's final state: closed, in flight, or settled. |
| A Service Dedupes By Call Id | A close of a call in flight MUST end the call. |
| A Service Fences A Superseded Owner | A call that has a receipt MUST be answered from the receipt whatever its lease generation. |
| A Service Fences A Superseded Owner | A service MUST fence a close by lease generation as it fences an admission. |

The receiver's highest observed generation remains the existing fence; this
proposal does not substitute a global ownership lookup or a new lease mechanism.
The existing authentication and assertion-call binding requirements still precede
receipt access. This proposal adds no scope check for a session's own close or
reconcile beyond the valid session assertion required by the existing contract.

## Proposed Source Delta

Replace the in-flight close sentence with:

> An admitted close of a call in flight MUST end the call.

Replace the close-fencing sentence with these atomic clauses under its existing
heading:

> A service MUST refuse a close's requested lifecycle mutation when its lease generation is older than the highest the service has recorded for that session.

> A service that refuses a close's requested lifecycle mutation because its lease generation is stale, and holds a receipt for the call, MUST return the recorded state with a distinct superseded-owner refusal.

> A close refused by the generation fence MUST leave the receipt and generation fence unchanged.

Terminal receipt replay remains idempotent and is not a new lifecycle mutation.
An absent-receipt close still undergoes the generation fence before creating its
closed receipt. This proposal selects no result encoding or protocol version.
Before source adoption, review whether any actual deployed client observes the
affected branch; any non-additive contract change needs the existing version and
migration discipline. Historical exact citations remain historical, and changed
sentences need explicit allocation reconciliation.

## Discriminating Acceptance

- With an active receipt and a recorded newer generation, the old owner's close
  returns the recorded state plus the distinct refusal, does not request ending,
  and changes neither receipt nor fence.
- The current owner's admitted close follows the existing end-request/end-proof
  contract; a map update alone does not prove that a managed call ended.
- A terminal receipt replays without a new effect or lifecycle mutation.
- A stale close without a receipt creates nothing and does not change the fence.
- Reconcile remains unfenced after the existing identity and scope checks.
- Wrong call claims and missing authority cannot use this branch to read state.

Only this stale-close contract direction needs review. Retention validation,
receiver-local time witnesses and independent lifecycle evidence can proceed
without choosing this branch. The current implementation's
`OutsideQualifiedDomain` marker remains a visible qualification gap until the
source meaning is resolved; it is not an invented operational response.
