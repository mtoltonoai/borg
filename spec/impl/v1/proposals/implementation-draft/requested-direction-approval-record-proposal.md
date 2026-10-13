# Proposed Approval-Record Wording

UNAPPROVED text for direct review only. This is a descriptive proposal outside the
authoritative specification. Automatic approval review rejected the earlier
authoritative edit because it considered the approval evidence untrusted; that
action did not execute. No retry, delegated write, authoritative insertion or
approval entry has been made. This proposal records no new approval.

The following four fragments match the requested directions only. Candidate
names and new feasibility recommendations belong solely in the separate research
proposal and remain unapproved. The current scope is evaluation/proposal work;
runtime remains paused. No product, layout, format or blanket milestone adoption
is recorded here.

## 1. Existing Transactional Engine Before An Owned Engine

> Qualify an existing transactional engine against the original applicable Borg
> requirements before choosing to build our own log, snapshot and recovery engine.

## 2. Existing Tagged Codec Before An Owned Codec

> Qualify an existing tagged codec against Borg's full compatibility contract
> before choosing to own one.

## 3. Constituent-Preserving Latest-State Event Hooks

> Retain every constituent event's required information and identity in a
> latest-state envelope while the receiver applies only the latest state.
> Exact delivery, acknowledgement, retry, sequence, signature, bounds and
> compatibility wording remains subject to separate review. Total constituent
> history capacity and retention remain unresolved; bounded envelopes do not
> establish bounded lossless history or satisfy the never-affect-session rule
> by themselves. No discarded event is treated as delivered, and this direction
> does not claim to resolve the unchanged original contract.

## 4. Publish-Time Subscription Entitlement

> Determine subscription delivery entitlement at publication. Unsubscribe stops
> later eligibility without itself retracting prior entitled work, subject to
> current security revocation, retention and explicit drops. Subscribe/rejoin
> start cursor remains unresolved.

The original requirements and exact open questions are linked in
[contract-direction-clarifications.md](contract-direction-clarifications.md).
The candidate evidence and proposed failure probes are in
[engine-codec-qualification.md](engine-codec-qualification.md).
None of this proposed wording has been copied into an authoritative file.
