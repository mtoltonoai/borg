# V1 Decision Record

This descriptive record states the implementation's selections, changes and rationale. Normative behavior is defined by the main, capability and phase specifications. Per-declaration adoption status is recorded in implementation-review.json. This record is not a fourth normative source and does not grant execution authorization.

## Fixed Selections

The implementation uses one-tenant dedicated hosts/stores, direct execution, Bach, Bolero and the specified Duvet interface. These choices remain defined in the main specification. Exception authority also remains in the main specification and is not delegated by this record.

## Coordination Scope

Coordination and file ownership are development-process boundaries, not platform runtime behavior. Independent work uses disjoint file boundaries; shared interfaces and specification integration have one coordinating owner. A declaration or generated plan does not establish an assignment or authorize execution.

## Seven-Stage Delivery

The seven-stage sequence establishes testable interfaces, proves real-store atomicity and restart recovery early, integrates durable event/session processing and the direct model/tool harness, completes remaining capabilities, qualifies integrated fault/recovery/isolation and real dependencies, then closes coverage and conformance.

This order gives early persistence and integration feedback without delaying tests until final qualification. Evidence accumulates throughout. Stages are milestone views rather than global barriers; independent work proceeds when its required decisions, interfaces and invariants are ready.

## Collaboration Platform

V1 selects the native build outcome. The shared specification retains reusable build/adapt definitions and a default of none; selecting build for v1 does not select it for every implementation.

The product provides collaboration records and interfaces over existing durability, events, identity and encryption mechanisms. Agent session state and operations remain accessible only through the platform API. This reuses common primitives without a new storage engine or independent agent-state subsystem. No task claim, task lease or goal entity is introduced.

The implementation scope is COLLAB, six capability dependencies, seven delivery milestones, 109 shared-source allocations and their qualification dependencies. COLLAB and its seven milestones retain their approved status; the 25 foundation capabilities and 93 foundation milestones remain proposed in the separate review metadata. The seven-stage structure is approved. Intended normative wording does not alter these statuses.

The imperative session-management and checkers-only outcomes remain selected. The five declarative collaboration clauses remain conditional-inactive. Product go-aheads do not widen platform authority or replace decision hooks. Runtime exclusivity is not a task lease and does not prohibit legitimate concurrent sessions.

### Non-Emergency Pause Boundary

Product lifecycle intent is the path for non-emergency collaboration pauses. Existing core emergency controls remain independent of product availability and lifecycle ownership, subject to their existing grants, safe-point timing and lifecycle precedence. Limiting the product pause clauses to non-emergency requests prevents the product from obstructing that existing core behavior; it adds no emergency power.

### Retired Pause Identities

The pause section heading is unchanged. The two sentence replacements narrow the collaboration-specific path; evidence for a retired sentence is not automatically evidence for its replacement.

| Shared file and section | Retired sentence | Replacement sentence |
|---|---|---|
| `spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path` | An agent's run, paused, or retired intent MUST be the only way to pause its sessions. | An agent's run, paused, or retired intent MUST be the collaboration platform's mechanism for requesting a non-emergency pause of its sessions. |
| `spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path` | A pause requested through any path MUST take effect by setting that intent. | A non-emergency pause requested through the collaboration platform MUST take effect by setting that intent. |

## Normative Presentation Cleanup

Independent obligations use separate sentences under stable headings. Shared Source quotations are references rather than newly authored v1 obligations; this prevents reference extraction from duplicating requirements. The main specification distinguishes product behavior, fixed selections, informative explanation and development/change-process constraints.

## Review-Ready Specification Presentation

The normative documents describe the complete intended behavior. Separately hash-bound review metadata records actual adoption status so the whole specification can be reviewed without silently adopting foundation declarations. Missing, stale or inconsistent metadata is rejected; valid hashes establish input integrity rather than independent proof of authorization.

Provider and self-durable store choices remain unapproved. Their milestone readiness still requires approved choices. The coalesced event-hook contract requires interface review; acknowledgement, capacity, retention and compatibility details remain unspecified. Existing drafts concerning engine/codec reuse, constituent-preserving event delivery and publication-time entitlement retain their separate status; this record does not change shared contracts or make those remaining decisions.
