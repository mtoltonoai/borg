# Requested Contract Directions: Clarification Draft

Status: descriptive proposal only. The operator requested these directions;
authoritative approval recording is on hold after automatic approval review
rejected the attempted write. No contract, approved specification, capability,
milestone or implementation is changed. Each detailed rule below needs separate
review; this is not a workaround for the rejected action.

## Event Hooks: Latest State With Every Constituent Event

Direction for review: a latest-state envelope retains every constituent event's
required information and identity, while the receiver applies only the latest
state rather than replaying intermediate state transitions.

The unchanged [event-hook contract, Delivery Comes From The Durable Outbox, lines 39-51](../../../../contracts/event-hook.md:39)
requires durable delivery, every event at least once, ordered sequence, dedupe
identity, signature and retry. [A Backlog Coalesces, lines 55-56](../../../../contracts/event-hook.md:55)
requires coalescing; [A Receiver Dedupes And Orders, lines 82-86](../../../../contracts/event-hook.md:82)
requires verified, duplicate-safe state application. The current wording does not
already define constituent delivery separately from state replay.

For unsent `idle #10` followed by `blocked #11`, the requested direction would
retain both event identities and the blocked reason, but apply only state #11.
It is not permission to erase #10 or report it delivered merely because #11 was
delivered. The closed event kinds and required fields in [lines 23-33](../../../../contracts/event-hook.md:23)
remain intact; this direction does not classify some kinds as disposable.

Review the following unresolved details before drafting an authoritative change:

| Question | Constraint on the eventual wording |
|---|---|
| Event delivery versus state application | Define when a constituent event is considered delivered, what information the receiver retains, and how applying only the latest state differs from replay. A latest-state value alone is not evidence that all event information arrived. |
| Acknowledgements and retries | Specify whether acknowledgement is per constituent, contiguous sequence prefix or complete envelope, and the durable receiver action it attests. Define lost acknowledgement, duplicate/overlapping batches, crash between recording events and applying state, and rebatching. No discarded event can be labelled acknowledged/delivered. No acknowledgement rule is selected here. |
| Identity and sequence | Define constituent identities versus any envelope identity, sequence ranges and gaps. Preserve ordering across retry, failover and overlapping envelopes; prevent older state overwrite. Do not invent sequence renumbering or collapse multiple event identities into one. |
| Signature | Specify the exact authenticated content, timestamp, constituent identities/order and latest-state binding, and verification before any effect. No serialization, signing algorithm, digest or format version is selected here. |
| Bounded envelope versus total history | A bounded envelope can partition transfer, but lossless constituent history and total traffic still grow with backlog. Total retained capacity, retention and behavior during an arbitrarily slow destination remain unresolved. Do not invent eviction, silent loss or an acknowledgement that claims discarded events were delivered. |
| Session isolation | [A Hook Never Affects Its Session, line 60](../../../../contracts/event-hook.md:60) remains binding. Batching alone does not prove that a slow destination cannot exhaust resources and affect the session. A concrete capacity/failure contract is still needed; none is adopted. |
| Compatibility | Determine old-receiver behavior, whether the change is additive, and any version increment/migration required by [Additive Evolution, lines 92-96](../../../../contracts/event-hook.md:92). No new envelope is silently enabled. |

Proposed acceptance examples, not executed tests: normal two-event coalescing;
lost response after receiver persistence; duplicate and overlapping batches;
out-of-order old envelope; tampered constituent/state binding; restart at each
ack/application boundary; and a destination that stays slow while events continue.
The final case cannot be declared passed until total-history/resource behavior is
specified. Ordinary noncoalesced retry tests do not close `FEED_S5`'s coalescing gate.

## Subscriptions: Entitlement At Publication

Direction for review: delivery entitlement is determined at publication;
unsubscribe stops eligibility for later publications but does not itself retract
work already entitled. Current security revocation, retention and explicit drops
still apply. Subscribe/rejoin start cursor is unresolved.

Original constraints: [Events And Subscriptions, Subscriptions Come From Definitions, Sessions, And Grants, lines 28-33](../../../../core/events-and-subscriptions.md:28),
[Publish Latency Is Independent Of Subscribers, lines 81-97](../../../../core/events-and-subscriptions.md:81),
[Each Subscriber Receives In Order, lines 101-105](../../../../core/events-and-subscriptions.md:101),
and [A Topic's Queue Is Bounded, lines 109-117](../../../../core/events-and-subscriptions.md:109).
This direction fills a membership-cutoff question; it does not replace those rules.

The proposed externally observable distinction is: if an event is entitled before
an unsubscribe takes effect, later fan-out does not revoke that entitlement merely
because membership is now absent. If the unsubscribe takes effect before a later
publish, that publish creates no new entitlement for that subscription.

Unresolved details for separate review:

- Entitlement changes follow successfully admitted publication and membership
  operations. A denied publish creates no entitlement; a denied unsubscribe
  changes no eligibility. Preserve the group's membership deciders and separate
  publish gate ([original lines 43-57](../../../../core/events-and-subscriptions.md:43)).
- Define the ordering/linearization of publish versus subscribe/unsubscribe,
  including races, owner moves and sharded topics. Do not infer it from wall time.
- Define what proves historical entitlement and how long that proof is retained.
  Do not silently choose a membership snapshot, log, schema or per-subscriber
  write at publish; publisher latency and bounded keys still apply.
- Specify first subscribe and rejoin cursor choices, including retained-history
  replay and lost lineage. Neither current-head start nor automatic replay is
  selected, and the viewer-only no-history rule does not answer this question.
- Keep current authorization independent of historical entitlement. A removed
  grant is revoked at the next decision and ancestor revocation propagates
  ([Identity And Delegation, lines 90-102](../../../../core/identity-delegation-and-secrets.md:90));
  a consumer still needs the current read check
  ([Tenancy And Confidentiality, line 108](../../../../core/tenancy-and-confidentiality.md:108)).
- Apply retention and explicit unrecoverable/drop reporting without converting
  ordinary unsubscribe into a silent drop or overriding emergency/security controls.

Proposed acceptance examples: both commit orderings of publish/unsubscribe; a
delayed fan-out batch after unsubscribe; unsubscribe/rejoin with a still-unselected
cursor policy; concurrent owner move; grant or ancestor revocation before actual
delivery; and retention expiry with explicit dropped range. These tests must use
the same logical ordering in simulator and real store. No implementation is run.

The affected slice is `ED_S5`'s session subscription contract. Neither this direction
nor the event-hook direction approves the 85 detailed milestones, 81 capability
edges, a storage/format choice or a runtime restart.
