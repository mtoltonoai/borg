# Implementation Capability Specification

This document is the single authored capability source in the linked [implementation specification set](implementation.md#single-source-top-level-capabilities). The main specification defines v1 scope, shared-specification precedence and declaration semantics. Deliverables, acceptance and prerequisites specify the required capability behavior and integration conditions.

## Source Scope

The complete graph has 27 capabilities and 94 edges. The pinned shared evaluation source is distinct from the preserved runtime target.

Source snapshot: ae8c0fca8fe823f5bb53585dc12bd54c20058072de70f8510810f70e5f6867b1

Source selection digest: 6ba36138d9eb02af757647311add23eab076553e52f8870a6c25812277aa580c

Source fields quote shared requirements by original file, stable section and exact sentence. They retain shared normative strength and applicability; they are references, not newly authored v1 obligations. Five collaboration clauses remain conditional-inactive under imperative session management.

Informative: Allocation comments associate a shared identity with a primary milestone/gate for review. Native Duvet supplies the requirement catalog and citation graph; it does not interpret allocation comments or prove acceptance. Governing requirements keep their change-process classification. Permissions and conditional requirements retain their guards.

## Foundation Declarations

ID: API
Title: Frontend and external controls
Prerequisites: AUTH, CFG, CO, E, FEED, H, L, R
Deliverable: Tenant-scoped external API, deduplicated controls, wait/tail and owner routing. Interface boundary: Authenticated tenant route grammar, core-derived operation tier, durable request equality, bounded admission/controls, uploads and nonspoofable refusal/trace fields.
Acceptance: Real requests authenticate, pass hooks, route to current owners and expose only authorized state across retries and moves. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | source-constraint | The quoted frontend requirement requires an authenticated principal on every call.
Dependency: CFG | inferred | Requires validated definition revision and adoption interfaces for integration.
Dependency: CO | inferred | Requires ownership generation, assignment and routing interfaces for integration.
Dependency: E | inferred | Requires governed ingress and bounded admission interfaces for integration.
Dependency: FEED | inferred | Requires authorized resumable feed and live-tail interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
Dependency: L | inferred | Requires creation, supervision and termination interfaces for integration.
Dependency: R | inferred | Requires receiver admission, receipt and reconciliation interfaces for integration.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/governance.md#an-emergency-control-is-never-blocked-by-policy | A tenant policy MAY flag an authorized emergency pause, quarantine, or stop.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/governance.md#an-emergency-control-is-never-blocked-by-policy | An emergency resume MUST be decided under the normal fail-closed rules.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/governance.md#an-emergency-control-is-never-blocked-by-policy | Once its grant check passes, an emergency pause, quarantine, or stop MUST NOT be blocked by a tenant policy or by a decider's failure.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/governance.md#an-emergency-control-is-never-blocked-by-policy | The grant check for an emergency pause, quarantine, or stop MUST be a substrate check that fails closed.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | A burst of new keys from one tenant MUST NOT shut out another tenant's new callers.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | A limiter charge MUST happen before the call guards admit, so that a throttled request claims no receipt and moves no fence.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | A request whose wait would exceed its class's bound MUST be refused at once with the exact delay.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | A route that must never wait or be refused MUST carry an exempt mark the product sets, never the caller.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | Rate limiting MUST answer a throttled caller with a retry-after.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | Rate limiting MUST stay bounded under a large number of distinct keys.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | The attested person a limit keys on MUST come from the verified chain rather than from a request field.
<!-- Allocation: API_S5#A_KEYED_LIMITER | runtime -->
Source: spec/core/shared-framework.md#rate-limiting | The framework MUST provide keyed rate limiting by tenant, principal, and route class, and by the attested person where a service acts for people.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#a-control-at-template-or-tenant-scope-reaches-every-partition | A keyed instance created while a pause or quarantine holds at its scope MUST start paused or quarantined.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#a-control-at-template-or-tenant-scope-reaches-every-partition | A template-scoped or tenant-scoped control MUST be recorded in a tenant-level record that identifies the control, its scope, the caller's principal and chain, the reason, the time, and its decision id.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#a-control-at-template-or-tenant-scope-reaches-every-partition | A template-scoped or tenant-scoped control MUST reach every partition hosting the tenant without a poll.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#a-control-at-template-or-tenant-scope-reaches-every-partition | Each session's application of a control MUST be an event.
<!-- Allocation: API_S5#B_API_DEFER_STATUS | runtime -->
Source: spec/core/the-frontend.md#a-deferred-operation-is-acknowledged-with-its-decision | A control operation that a policy defers MUST be acknowledged as deferred together with its decision id.
<!-- Allocation: API_S5#B_API_DEFER_STATUS | runtime -->
Source: spec/core/the-frontend.md#a-deferred-operation-is-acknowledged-with-its-decision | The API MUST accept a read of a decision by its id that reports whether the deferred operation applied, lapsed, or was blocked.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-forwarded-identity-comes-only-from-a-configured-proxy | A node whose proxy configuration is empty or unresolved MUST refuse every identity header.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-forwarded-identity-comes-only-from-a-configured-proxy | A request whose forwarded identity is refused MUST be treated as unauthenticated, with a refusal event.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-forwarded-identity-comes-only-from-a-configured-proxy | Traffic between an ingress proxy and the frontend MUST be encrypted.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | A client of the platform MUST read and write session state only through the API rather than through the durable store.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | Each route of the API MUST carry exactly one control-operation kind.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | Every call MUST be scoped to one tenant.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | Every call MUST pass the control-operation hook, so that an external caller meets the same policy a session does.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | Every call to the frontend MUST be made by an authenticated principal.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#a-principal-makes-every-call | The tier of a control operation MUST be determined by the core from the operation's kind rather than supplied by the caller.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#a-read-of-state-or-transcript-returns-what-the-grant-allows | A read of a blocked session's state MUST return the identity of the decider that blocked it and whether that decider answered a block or failed to answer.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#a-read-of-state-or-transcript-returns-what-the-grant-allows | A read of a session's state MUST NOT return content.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#a-read-of-state-or-transcript-returns-what-the-grant-allows | A transcript read MUST carry the compaction count its positions belong to, so that a reader detects a renumbering.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#a-read-of-state-or-transcript-returns-what-the-grant-allows | A transcript read through a read grant MUST omit reasoning content.
<!-- Allocation: API_S5#B_API_REQUEST_BOUNDS | runtime -->
Source: spec/core/the-frontend.md#a-request-completes-within-the-connection-lifetime | A frontend request other than the tail MUST complete within the configured connection-lifetime bound.
<!-- Allocation: API_S5#B_API_REQUEST_BOUNDS | runtime -->
Source: spec/core/the-frontend.md#a-request-completes-within-the-connection-lifetime | Every long-poll bound and keepalive interval on the frontend's path MUST be below the connection lifetime.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A publish or send id MUST be deduplicated per publisher principal and destination, so that the same id reused toward another destination is a fresh event.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A request id reused with a body whose canonical digest differs MUST be refused as a conflict.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A request whose first attempt is older than the dedupe window MUST be refused as stale rather than applied.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A request whose id and body match a record within the dedupe window MUST be answered from the record rather than applied again.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A request without a first-attempt time, or with one further ahead of the node's clock than the clock-skew bound, MUST be refused as invalid.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | A retry MUST repeat the request id and the first-attempt time of the first attempt.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#a-request-that-changes-state-is-deduplicated | Every request that changes state MUST carry a request id and the time of its first attempt.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#a-webhook-delivery-is-deduplicated-and-bounded | A delivery with no id MUST NOT be deduplicated.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#a-webhook-delivery-is-deduplicated-and-bounded | A webhook delivery MUST be deduplicated by the provider's delivery id for the endpoint's configured window.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#a-webhook-delivery-is-deduplicated-and-bounded | A webhook delivery past the endpoint's record cap MUST be refused with a retry delay rather than accepted by evicting an older record.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | A publish or a send MUST refer to a staged upload only when the publisher's principal and chain both equal the recorded provenance.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | A publish whose attachments cannot all be resolved MUST append nothing.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | A staged holder MUST record the uploading call's principal and chain as provenance that grants nothing.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | A staged upload MUST be referable only within the configured window after the commit that recorded it, measured on the owner's clock.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | A window on which a party other than the caller depends MUST be measured from a time the platform recorded on the owner's clock rather than from a time the caller supplied.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | An upload's dedupe record MUST last the life of its pin.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-attachment-is-staged-before-it-is-referenced | The API MUST accept an upload that stages an attachment's bytes under a holder owned by the tenant and returns a reference to it.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-is-granted-and-decided-in-process | A defer or a transform MUST NOT be returned for an emergency control.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-is-granted-and-decided-in-process | An emergency control MUST be decided only by deciders that run inside the platform's own process.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-is-granted-and-decided-in-process | An emergency control MUST require a grant for that control at that scope.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-takes-effect-at-a-safe-point | A model attempt abandoned by an emergency control MUST run again as a new attempt when the session resumes.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-takes-effect-at-a-safe-point | A pause requested by the owner of the list of root sessions MUST take effect at the session's next turn boundary.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-takes-effect-at-a-safe-point | A tool call already dispatched when an emergency control takes effect MUST be left to finish under the fence.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-takes-effect-at-a-safe-point | An emergency control on a session MUST reach its sub-sessions and forks.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#an-emergency-control-takes-effect-at-a-safe-point | An emergency pause, quarantine, or stop MUST take effect at the current step's safe point rather than wait for a turn boundary.
<!-- Allocation: API_S3#B_API_DEDUPE | runtime -->
Source: spec/core/the-frontend.md#an-event-is-an-opaque-deduped-payload | An event accepted by the API MUST carry a topic, a priority, and an id that dedupes retries.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#an-operator-of-the-platform-sets-a-tenant-s-caps | A cap set for a tenant MUST take effect on every node from the tenant's next request.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#an-operator-of-the-platform-sets-a-tenant-s-caps | A request that sets caps MUST identify the tenant it acts on as its resource, distinct from the caller's tenant.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#an-operator-of-the-platform-sets-a-tenant-s-caps | A tenant MUST be able to read the caps in force for it.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#an-operator-of-the-platform-sets-a-tenant-s-caps | A tenant's caps MUST be set for the tenant by an operator of the platform rather than by the tenant.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#an-operator-of-the-platform-sets-a-tenant-s-caps | Every change to a tenant's caps MUST be published as an event identifying the tenant, the operator, and the caps.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-upload-is-checked-before-its-body-is-read | A reserved slot MUST be released early only on a definite answer that the attempt committed nothing.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-upload-is-checked-before-its-body-is-read | A tenant's count of staged uploads MUST be reserved before any byte of the body is read.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-upload-is-checked-before-its-body-is-read | An upload whose bytes do not match the declared digest MUST commit nothing.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-upload-is-checked-before-its-body-is-read | An upload's caller MUST declare the digest of the body in a signed header before the body is sent.
<!-- Allocation: API_S5#B_API_ATTACHMENT | runtime -->
Source: spec/core/the-frontend.md#an-upload-is-checked-before-its-body-is-read | Every check on an upload other than the comparison of the body's digest MUST run before any byte of the body is read.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | A cap MUST be counted across the cluster rather than per node.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | A cap check MUST be atomic with the operation it bounds, so that concurrent requests cannot exceed the cap together.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | A request past a cap in force MUST be refused before any of its effects is applied.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | A send to a session whose queue is at its depth cap MUST be refused with a retryable status that states the cap.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | A tenant MUST have a configurable cap on each of its sessions, its concurrent model steps, each session's queue depth, its message rate, and its topic groups.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#caps-are-configuration | Every operational cap MUST resolve at runtime from configuration rather than from a constant in code or in a node's configuration file.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#development-modes-stay-on-development-stages | A development-only authentication or transport mode MUST refuse to start outside the development stage.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#development-modes-stay-on-development-stages | A stage left unset MUST be treated as deployed.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list | An emergency control MUST NOT be rate limited.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list | An emergency control MUST take effect at its commit without waiting on any store outside the cluster.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list | An emergency control MUST work when the owner of the list of root sessions is unreachable.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list | The API MUST expose pause, stop, quarantine, and resume, per root session, per definition or template, and per tenant.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#emergency-controls-work-without-the-owner-of-the-list | The frontend MUST accept an emergency control during a drain.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#every-refusal-has-one-shape | A refusal of a path or a method no route serves MUST carry the same error shape and a stable code as every other refusal.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#every-refusal-has-one-shape | A refusal the frontend makes before a route is matched MUST be counted and published like a routed refusal.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#every-refusal-has-one-shape | A refused request body MUST carry a status that distinguishes an unsupported media type, a wrong shape, an oversized body, and an unparsable body.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#every-route-has-an-exact-grammar | A request MUST carry its tenant in exactly one place.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#every-route-has-an-exact-grammar | A route that takes no query MUST refuse a request that carries one.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#every-route-has-an-exact-grammar | The frontend MUST refuse a request whose path does not match its route's grammar exactly, with no percent-encoding and no trailing, empty, or doubled segment, before any route or grant check runs.
<!-- Allocation: API_S1#B_API_GRAMMAR | design -->
Source: spec/core/the-frontend.md#every-route-has-an-exact-grammar | The frontend MUST state the grammar of every path segment of every route.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/core/the-frontend.md#governance-operations-are-governance-tier | A grant MUST be written by the identity adapter rather than by an arbitrary caller.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/core/the-frontend.md#governance-operations-are-governance-tier | Binding a hook destination MUST be a governance-tier operation.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/core/the-frontend.md#governance-operations-are-governance-tier | Binding, changing, or unbinding a webhook endpoint MUST be a governance-tier operation.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/core/the-frontend.md#governance-operations-are-governance-tier | Writing a policy, a grant, or a catalog entry MUST be a governance-tier operation.
<!-- Allocation: API_S5#B_API_CALL_ADMISSION | runtime -->
Source: spec/core/the-frontend.md#operator-operations-are-admitted-by-role | A platform-operator operation MUST be admitted only for a principal that holds a configured operator role rather than for every principal of the operator's account.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A refusal body MUST NOT carry content or a credential.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A refusal body MUST carry its class, the decision id where a decision gave it, and ids.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A refusal of a reference the caller is not permitted to use MUST be indistinguishable from the reference not existing.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A refusal that follows from the caller's own configuration MUST be answered with a non-retryable status that states what was refused.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A request for a tenant whose configuration record supplies no capacity source MUST be refused with a configuration reason before anything runs.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A request that fails because a dependency of the platform was unreachable, throttled, or timed out MUST be answered with a retryable status distinct from a refusal of the request.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | A throttled refusal MUST carry the delay after which a retry can succeed and the level of the limit that refused.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | Each refusal class MUST state whether a retry of the same request can succeed.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#refusal-classes-are-a-closed-set | The refusal classes of the API MUST be a closed set.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#replies-are-legible-and-traceable | A refusal MUST carry a stable error code that does not change with its wording.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#replies-are-legible-and-traceable | A reply MUST echo the trace context it continued or started, including a refusal made before a route is matched.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#replies-are-legible-and-traceable | A request over a size cap MUST be refused with a status that states the cap it exceeded.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#replies-are-legible-and-traceable | Every reply the API returns and every event a call causes MUST carry the request id of that call.
<!-- Allocation: API_S5#B_API_REFUSALS_TRACING | runtime -->
Source: spec/core/the-frontend.md#replies-are-legible-and-traceable | The API MUST propagate distributed trace context, continuing a caller's trace or starting a new one.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#sessions-are-listed-in-bounded-pages | A listing MUST reflect a session's creation and its removal from the commit that made each.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#sessions-are-listed-in-bounded-pages | A listing cursor MUST be valid only within the tenant that issued it.
<!-- Allocation: API_S5#B_API_READS_LISTS | runtime -->
Source: spec/core/the-frontend.md#sessions-are-listed-in-bounded-pages | The API MUST accept a listing of the caller's tenant's sessions in bounded pages.
<!-- Allocation: API_S5#B_API_REQUEST_BOUNDS | runtime -->
Source: spec/core/the-frontend.md#the-access-log-records-requests-without-content | A configuration change MUST NOT lower the access log's retention.
<!-- Allocation: API_S5#B_API_REQUEST_BOUNDS | runtime -->
Source: spec/core/the-frontend.md#the-access-log-records-requests-without-content | The access log's retention MUST be at least the configured minimum.
<!-- Allocation: API_S5#B_API_REQUEST_BOUNDS | runtime -->
Source: spec/core/the-frontend.md#the-access-log-records-requests-without-content | The frontend's access log MUST record each request's authenticated principal, method, path, and response code, and no header or body beyond those.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#the-effective-lifecycle-is-the-most-restrictive | A change requested by the owner of the list of root sessions MUST NOT lift an emergency control.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#the-effective-lifecycle-is-the-most-restrictive | A resume MUST NOT override a pause or a retirement the owner of the list requests, or the out-of-budget state.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#the-effective-lifecycle-is-the-most-restrictive | A session's effective lifecycle MUST be the most restrictive of the lifecycle its owner requests, the emergency controls in force at its session, its template, and its tenant, and any quarantine the platform placed.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#the-effective-lifecycle-is-the-most-restrictive | An emergency control MUST hold while no resume at its own scope has lifted it.
<!-- Allocation: API_S5#B_EMERGENCY_CONTROLS | runtime -->
Source: spec/core/the-frontend.md#the-effective-lifecycle-is-the-most-restrictive | The restrictiveness order MUST be run, then paused, then quarantined, then stopped or retired.
<!-- Allocation: API_S5#B_API_TIMERS | runtime -->
Source: spec/core/the-frontend.md#timer-operations-reach-only-the-caller-s-timers | A timer operation the target has not applied within the call's deadline MUST be acknowledged as accepted.
<!-- Allocation: API_S5#B_API_TIMERS | runtime -->
Source: spec/core/the-frontend.md#timer-operations-reach-only-the-caller-s-timers | The API MUST accept setting, re-arming, and cancelling a timer on a session from a principal holding a grant for it, reaching only the timers that principal set.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#usage-is-counted-whether-or-not-a-cap-is-in-force | Each usage count MUST be published as an event carrying the tenant and the measure, so that a cap can be chosen from what tenants use.
<!-- Allocation: API_S5#B_API_CAPS | runtime -->
Source: spec/core/the-frontend.md#usage-is-counted-whether-or-not-a-cap-is-in-force | The frontend MUST count each tenant's usage of every capped quantity whether or not a cap is in force.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#webhooks-let-an-external-system-publish | A provider's own check on a webhook MUST NOT decide the principal.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#webhooks-let-an-external-system-publish | A webhook MUST NOT refer to an attachment.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#webhooks-let-an-external-system-publish | A webhook MUST be an authenticated endpoint mapped to a principal.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#webhooks-let-an-external-system-publish | A webhook endpoint MUST map to a service principal in its own tenant rather than to a person or a template.
<!-- Allocation: API_S5#B_API_WEBHOOK | runtime -->
Source: spec/core/the-frontend.md#webhooks-let-an-external-system-publish | A webhook endpoint's shared secret MUST be referenced by its configuration rather than held in it, readable only by the verifier.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept a publish to a topic.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept a question to a session answered through a fork that is discarded after it answers and does not modify the original.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept a read of a session's state, and of its transcript ranges with a read grant.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept a send to one session, or to a keyed template with its key, creating the instance on first use.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept creation, replacement, removal, and listing of a tenant's topic groups.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept management of a session's dynamic subscriptions on its behalf.
<!-- Allocation: API_S5#B_API_DATA_PLANE | runtime -->
Source: spec/core/the-frontend.md#what-the-data-plane-accepts | The API MUST accept resolution of a defer, from a principal the defer allows.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/core/durability-and-deployment.md#platform-limits-are-runtime-settings | A platform operator MUST be able to change a runtime setting through the API without a redeploy or a restart.
<!-- Allocation: API_S5#B_API_GOVERNANCE_ROUTES | runtime -->
Source: spec/decisions/session-management/imperative.md#a-policy-holds-settings-a-session-inherits | The API MUST accept a policy record, referenced by name, that holds settings a root session may inherit.

---

ID: AUTH
Title: Authentication, assertions and credential transport
Prerequisites: SF, SR
Deliverable: Caller verification, signed call assertions, peer authentication and per-call credential delivery. Interface boundary: Authenticated caller/actor chain, signed per-call assertions, request/body/deadline bindings, broker leases, registered egress and refusal before admission.
Acceptance: Wrong tenant, audience, expiry, lineage or call bindings fail; secrets reach only their executor; rotation preserves valid calls. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: SF | inferred | Requires sealing, key lifecycle and compatible durable codecs for integration.
Dependency: SR | inferred | Requires cluster-independent resource and signing-root interfaces for integration.
<!-- Allocation: AUTH_S3#C_CALLER_AUTH | runtime -->
Source: constitution.md#identity-comes-from-authentication | A human sign-on credential MUST NOT be used to run automation.
<!-- Allocation: AUTH_S3#C_CALLER_AUTH | runtime -->
Source: constitution.md#identity-comes-from-authentication | The acting identity of a call MUST come from authentication rather than from an argument, a header the caller controls, or a payload.
<!-- Allocation: AUTH_S6#C_NO_SECRETS | runtime -->
Source: constitution.md#secrets-never-reach-the-model-or-the-records | A secret MUST NOT appear in model context, a transcript, session state, an event, an observation record, a metric, or a log.
<!-- Allocation: AUTH_S6#C_NO_SECRETS | runtime -->
Source: constitution.md#v-secure-by-default | A component that cannot obtain a policy decision, a key, or a credential MUST fail closed.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A signing lineage a tenant has never seen MUST raise exactly one event across every node and restart.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST accept exactly one signature scheme.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST act only on claims decoded from the signed payload.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST check the signature over the bytes it received before it decodes any claim.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse a call whose presented lease generation differs from the one the assertion carries.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse a chain longer than the configured bound.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion in any encoding other than the one the contract declares.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion presented for a call id other than the one it carries.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion signed by a key outside the published set.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion that carries a tenant other than the one the call is addressed to.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion that declares no signature, another signature scheme, or another token type.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion whose audience is not the verifier.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion whose expiry has passed.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion whose issue time, less the configured clock-skew allowance, is later than the verifier's current time.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST refuse an assertion whose lifetime exceeds the configured maximum.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier MUST trust a forwarded claim only from a node signing under the lineage of the partition owner the call is addressed to.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier of a streamed call MUST commit nothing on a digest mismatch.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier of a streamed call MUST compare the streamed bytes with the declared digest at stream end.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier of a streamed call MUST run every check other than the body comparison before it reads the body.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | A verifier that cannot fetch the key set MUST fail closed.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#a-verifier-refuses-what-does-not-match | Every refusal MUST be published as an event carrying one reason class from a closed set and never the token.
<!-- Allocation: AUTH_S1#B_ASSERTION_ENCODING_CONTRACT | design -->
Source: spec/contracts/signed-assertion.md#additive-evolution-of-this-contract | An integer field that may exceed the declared exact-integer bound MUST be carried as a decimal string.
<!-- Allocation: AUTH_S1#B_ASSERTION_ENCODING_CONTRACT | design -->
Source: spec/contracts/signed-assertion.md#additive-evolution-of-this-contract | Each field's encoding MUST be declared by this contract rather than chosen per value.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST NOT carry a credential, a secret, or transcript content.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST carry an expiry no later than the configured assertion lifetime after issue.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST carry its issue time.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST carry its issuer.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST carry the scopes that apply to the call, derived from the session's grants at the moment of issue.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST carry the session's lease generation.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-carries-what-the-call-may-do | An assertion MUST identify the grant from which its scopes were derived, so that the service's audit record identifies the grant.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | A chain MUST have at most the configured number of entries.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion MUST carry the session id.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion MUST carry the session's incarnation as a claim separate from the session id.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion MUST carry the session's on-behalf-of chain in order from the outermost delegator to the session.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion MUST carry the session's principal.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion MUST carry the tenant the session belongs to.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion for a sub-session or a fork MAY identify the parent session.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion issued for a fork MUST identify the fork rather than its parent.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | An assertion presented by a node that forwards another caller's call MUST carry that caller's verified principal and chain unchanged in a forwarded claim.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-actor | Each entry of the chain MUST carry its relation, delegated or attested, and its attester.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-call-it-is-bound-to | A call the platform makes on its own behalf, with no session, MUST carry a call id that can never equal a session's call id and is unique per calling actor within the receipt retention.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-call-it-is-bound-to | An assertion MUST carry a digest of the request it is bound to: the method, the target as sent, and the body's digest.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-call-it-is-bound-to | An assertion MUST carry the audience that identifies the one service it is addressed to.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-call-it-is-bound-to | An assertion MUST carry the call id of the one call it was issued for.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#an-assertion-identifies-the-call-it-is-bound-to | An assertion for a streamed body MUST carry the digest of the body that the caller declared before the body was sent.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#every-outbound-call-carries-one | Every credential lease the platform requests MUST carry an assertion addressed to the credential broker.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#every-outbound-call-carries-one | Every tool call the platform makes MUST carry an assertion addressed to the tool server.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/contracts/signed-assertion.md#every-outbound-call-carries-one | Where a definition controller exists, every read it makes MUST carry an assertion that identifies the binding's tenant, or, where the source cannot verify an assertion, a credential the binding identifies by reference.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A failed refresh MUST be published as an event.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A refresh MUST replace the key set only when every configured key source was read.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A refresh that fails or reads no key MUST keep the key set read last while that set is within the configured age.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A rotation MUST NOT fail a call that was in flight when it began.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A rotation MUST keep the previous signing key verifiable until the last assertion signed under it has expired.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A signing key MUST be published to verifiers before the first assertion signed under it.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A signing key MUST sign nothing until the configured publication lead after its publication has passed.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A verifier MUST bound how often it fetches a key lineage it has not seen.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A verifier MUST read the key set only through a path that authenticates the verifier, never through an unauthenticated route.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A verifier MUST refresh its cached key set within the configured age and drop any key the set no longer lists.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | A verifier that holds no verification key MUST refuse to start.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | Each key id MUST carry the id of the lineage that signs under it, so that a verifier can bound which lineage's keys it trusts.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/contracts/signed-assertion.md#keys-are-published-and-rotated-without-a-gap | The platform MUST publish the set of verification keys at a location every verifier can fetch.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A close of the caller's own call MUST require no scope beyond a valid assertion for the session.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A reconcile of the caller's own call MUST require no scope beyond a valid assertion for the session.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A refusal for a missing scope MUST be distinct from an authentication failure and from a fence refusal.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A refusal for a stale lease generation MUST be distinct from an authorization failure.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST accept an assertion that carries an ended claim only for a close or a reconcile of a call the ended incarnation left open.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST decide, record, and attribute a forwarded call on the forwarded principal and chain rather than on the identity of the node that forwarded it.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST refuse a call that has no receipt and whose lease generation is older than the highest it has recorded for that session.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST scope everything a call does to the assertion's tenant, session, principal chain, and scopes before the service's own authorization runs.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST take the acting identity of a call from the assertion rather than from an argument or a header the caller controls.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | A service MUST verify every assertion it receives itself rather than accept a verification performed by another component on the request's path.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | An assertion for a close or a reconcile of a call that an ended incarnation left open MUST carry an ended claim.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-assertion-scopes-the-call | The identity a forwarding node presents MUST hold only the scope of the kind of hop it makes.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-signature-is-asymmetric | An assertion MUST be signed with an asymmetric scheme, so that a verifier holding the verification key cannot issue an assertion.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-signature-is-asymmetric | An assertion MUST carry its claims only as the signed payload, never as a second structured copy beside the signature.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-signature-is-asymmetric | An assertion MUST have exactly one encoding.
<!-- Allocation: AUTH_S4#B_AUTH_ASSERTION_VERIFY | runtime -->
Source: spec/contracts/signed-assertion.md#the-signature-is-asymmetric | Verifying an assertion MUST NOT require a call to the key service.
<!-- Allocation: AUTH_S6#C_DEPENDENCY_AUTH_BACKOFF | runtime -->
Source: spec/core/failure-handling.md#an-authentication-failure-toward-a-dependency | A refused role MUST NOT be tried again before the configured bound elapses or the tenant's configuration changes.
<!-- Allocation: AUTH_S6#C_DEPENDENCY_AUTH_BACKOFF | runtime -->
Source: spec/core/failure-handling.md#an-authentication-failure-toward-a-dependency | A tenant-supplied role that cannot be assumed MUST block the tenant's affected sessions with the reason.
<!-- Allocation: AUTH_S6#C_DEPENDENCY_AUTH_BACKOFF | runtime -->
Source: spec/core/failure-handling.md#an-authentication-failure-toward-a-dependency | An authentication failure toward a dependency MUST be handled once per cluster, with backoff and an alert, rather than once per session.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-definition-has-its-own-principal | A definition MUST NOT specify a principal of its own other than the derived one.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-definition-has-its-own-principal | A definition MUST carry no grant or credential.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-definition-has-its-own-principal | A definition's principal MUST be derived from its tenant and the definition's identity within the tenant, never from a person.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-definition-has-its-own-principal | An agent MUST act as its definition's principal rather than as a person or a service, whatever starts its sessions.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#an-agent-identity-outlives-its-sessions | An agent identity MUST be a durable record that outlives any one of its sessions.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#an-agent-identity-outlives-its-sessions | An agent identity MUST be able to have more than one session at a time.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#an-agent-identity-outlives-its-sessions | Every session MUST carry the agent identity it runs as.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A chain entry MUST carry its relation to the previous entry, delegated or attested, and the party that attested it.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A person a service attests MUST enter the chain as an entry attested by that service rather than as the acting principal.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A person identified in a service's request MUST be honored only under a grant that permits that service to attest that person for that action.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A record that a person runs an agent MUST NOT enter an assertion or a chain.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A record that a person runs an agent MUST grant nothing.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A session MUST run as a principal.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | A session's configuration MUST identify whom it acts on behalf of.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | Every action MUST be attributed to a principal.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-action-has-a-principal | The on-behalf-of chain MUST be carried with every model call, tool call, outbound message, control operation, and event.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#higher-levels-supply-providers-policy-the-broker-and-flows | Policy content MUST be supplied by the tenant rather than carried by the core.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#higher-levels-supply-providers-policy-the-broker-and-flows | The credential broker's backends MUST be supplied through one interface.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#higher-levels-supply-providers-policy-the-broker-and-flows | The identity adapter MUST write an operator's grant when the operator starts a session, capped by the operator's permissions and the role's upper bound.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#higher-levels-supply-providers-policy-the-broker-and-flows | The mapping from external identities to principals MUST be supplied by an identity adapter at a higher level.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#no-human-credential-runs-automation | A connection's lifetime MUST NOT exceed the lifetime of the credential that opened it.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#no-human-credential-runs-automation | A node's own access to its dependencies MUST come from a machine credential that refreshes without a person.
<!-- Allocation: AUTH_S5#B_AUTH_ADAPTER_AND_MACHINE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#no-human-credential-runs-automation | Schema migrations MUST run under an identity the service does not hold at run time.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#principal-ids-carry-their-provider-and-tenant | A person MUST be able to belong to more than one tenant under one provider-qualified id.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#principal-ids-carry-their-provider-and-tenant | A principal id MUST NOT be reissued to another holder.
<!-- Allocation: AUTH_S6#B_AUTH_PRINCIPALS | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#principal-ids-carry-their-provider-and-tenant | A principal id MUST carry its identity provider, so that adding a provider adds data rather than changing the ids of existing principals.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records | A bearer credential MUST NOT be written to a session's configuration or to the durable store in plaintext.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records | A tool result and an event payload MUST pass the structured-secret decider.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records | At dispatch the core MUST obtain a short-lived credential from the broker for the principal, chain, and target.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records | The core MUST deliver a leased credential only to the executor.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#secrets-stay-out-of-the-model-and-the-records | The full output of a tool call MUST pass the structured-secret check before it is stored.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion | A call the platform makes on its own behalf, with no session, MUST carry a call id that can never equal a session's call id, unique per calling actor within the receipt retention.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion | A capability-style token MUST NOT be the authoritative record of authority.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion | A capability-style token MUST be used only at trust boundaries and be short-lived.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion | An audit record of a call the platform makes on its own behalf MUST identify the calling actor beside the call id.
<!-- Allocation: AUTH_S6#B_AUTH_ASSERTION_ISSUE | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#services-take-identity-from-an-assertion | For each call or lease, the platform MUST present a short-lived signed assertion bound to that call, of the session's principal, chain, tenant, scopes, and lease generation.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#signing-roots-are-held-apart-from-the-cluster | A lineage's signing root MUST NOT be created or administered by the cluster's own deployment.
<!-- Allocation: AUTH_S6#B_AUTH_KEY_ROTATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#signing-roots-are-held-apart-from-the-cluster | A node MUST retry obtaining its signing grant rather than report itself ready without it.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | A credential MUST NOT be reused for another call.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | A credential MUST be leased for one call.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | A credential exchange MUST be bound to the call's assertion, tenant, session, and call id.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | A credential the broker issues MUST be scoped to the principal, its chain, and the target.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | A delegated credential MUST be obtainable only by the broker's own identity.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | Every credential issued, and every broker call that fails, MUST be an event that identifies the grant and the identity requested, never the secret.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | The broker MUST decide on the assertion alone, without reading the durable store.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | The broker's identity MUST be assumable only from the service's own deployment.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | The broker's own base credentials MUST carry no delegated identity of their own.
<!-- Allocation: AUTH_S6#B_AUTH_LEASE_BROKER | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-broker-is-its-own-trust-boundary | The scopes of an issued credential MUST derive from the call's scopes and nothing broader.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | A refused assumption of a tenant's role MUST be tried again at the next call rather than recorded as permanently invalid.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | Each operator's sessions MUST act with credentials issued for that operator's delegation, attributable and revocable per operator.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | Every role a tenant supplies MUST be assumed with the tenant's identifier as the condition value the role requires of its caller, presented by the platform rather than taken from the caller.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | The operator carried into a target's audit trail MUST be the outermost delegator of the session's chain.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | The platform MUST NOT present an operator to a target through a path the target does not verify.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | The rendering of a principal id carried into an external audit trail MUST map no two principals to one value.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | Where a target authorizes the platform's own identity rather than the operator, the per-operator limit MUST be enforced by the scopes of the call's assertion before the call is made.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | Where a target system offers an on-behalf-of exchange, the platform MUST present the operator as the subject and itself as the actor.
<!-- Allocation: AUTH_S5#B_AUTH_OPERATOR_TARGET | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#the-operator-is-carried-into-the-audit-trail | Where a target system offers no on-behalf-of path, the platform MUST act as itself and record the operator in its audit and in the visible text.
<!-- Allocation: AUTH_S3#A_CALL_AUTH | runtime -->
Source: spec/core/shared-framework.md#caller-authentication-and-call-guards | The framework MUST provide caller authentication that turns a request into a caller whose identity comes from authentication.
<!-- Allocation: AUTH_S6#A_PEER_TRANSPORT | runtime -->
Source: spec/core/shared-framework.md#node-transport-is-authenticated | Every connection between the platform's nodes, including the durable store's replication, membership, and control traffic and the coordinator's placement and partition-transfer traffic, MUST authenticate both peers.
<!-- Allocation: AUTH_S6#A_PEER_TRANSPORT | runtime -->
Source: spec/core/shared-framework.md#node-transport-is-authenticated | The credential from which peer authentication is derived MUST be rotated within the configured interval.
<!-- Allocation: AUTH_S6#A_PEER_TRANSPORT | runtime -->
Source: spec/core/shared-framework.md#node-transport-is-authenticated | The credential from which peer authentication is derived MUST be scoped to one cluster.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | A connection the egress point refuses MUST be published as an event carrying the destination and the address.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | A destination inside a refused range MUST be admitted only when it is registered by name for the request's use, resolves into the address range the deployment records for that internal path, and presents a certificate that verifies for that name.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | A destination registered for one use MUST be admitted only for a request of that use.
<!-- Allocation: AUTH_S6#ALL_CALLER_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | Every outbound connection the platform makes, to a tool server, a hook destination, a decision-hook destination, or any other registered destination, MUST leave through the egress point.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | The egress point MUST NOT follow a redirect off the registered destination.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | The egress point MUST admit only the destinations the tool's binding lists.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | The egress point MUST check a destination's resolved address at every connection, in every address form, so that a host name that resolves to a refused address is refused too.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | The egress point MUST refuse a destination that resolves to a loopback address, a link-local address, an unspecified address, the host's own service range, a private range, or an address in the cluster's own network ranges, unless the tenant's configuration allows that range on the development stage.
<!-- Allocation: AUTH_S4#C_TOOL_EGRESS | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | The egress point MUST verify the certificate of every destination it admits.

---

ID: B
Title: Blob ownership and reclamation
Prerequisites: DA, SF
Deliverable: Immutable blobs, owners, pins, transfers and reclamation. Interface boundary: Immutable owner-scoped sealed objects, trusted provenance, pins, transfer, deletion and delayed-error/retention boundaries.
Acceptance: Real publication, pin and deletion races preserve references; transfer re-seals and deletion destroys required keys. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
Dependency: SF | inferred | Requires sealing, key lifecycle and compatible durable codecs for integration.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-carries-its-key-chain | A blob's header MUST carry its wrapped key chain, so that the blob store and the key service alone can recover a session's history.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-is-addressed-by-its-stored-bytes | A blob MUST be addressed by the hash of its stored ciphertext.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-is-addressed-by-its-stored-bytes | A blob's name MUST reveal nothing about its plaintext.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-is-addressed-by-its-stored-bytes | A store or cache MUST be able to verify a blob's integrity from its name without holding any key.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-is-immutable-and-owned | A blob MUST NOT change once written.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/contracts/stored-form.md#a-blob-is-immutable-and-owned | A blob MUST be stored under its tenant and its owner.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/core/encryption-and-blobs.md#a-blob-is-owned-and-immutable | An owner id MUST NOT be reused.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/core/encryption-and-blobs.md#a-blob-is-owned-and-immutable | Every blob MUST have one owner and be stored under its tenant and owner.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/core/encryption-and-blobs.md#a-blob-is-owned-and-immutable | The owner's writer MUST be the only writer of a blob's pins, transfers, and key deletion, under the owner's generation, so that a new reference cannot race a delete.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | A blob's name and its content id MUST be kept as separate values, neither derived from the other.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | A content id MUST be the hash of the bytes its consumers can compute and verify.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | A content id MUST resolve only within its tenant unless it is content published across tenants on purpose.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | A content kind whose bytes a party outside the platform computes MUST be identified by the digest of its canonical plaintext bytes.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | A plaintext content id MUST appear only in records kept under the tenant's key.
<!-- Allocation: B_S5#A_BLOB_CONTENT_IDS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-content-id-is-the-hash-its-consumers-can-compute | An event, usage record, metric, log line, or error text MUST identify a catalog entry by its catalog id and version rather than by its artifact's content id.
<!-- Allocation: B_S5#A_BLOB_RECLAIM_ERRORS | runtime -->
Source: spec/core/encryption-and-blobs.md#a-permission-refusal-is-never-not-found | A permission refusal from the object store MUST be an internal error with an alarm, never a not-found answer.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | A node MAY share one compiled module or one loaded model per content id in memory across the tenants it hosts, provided the object is built only from platform-published bytes, is immutable, keeps request state per tenant, and serves no mixed batch.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | A tenant MUST be served from a shared compiled form only while the tenant's own stored copy exists and verifies to the same id.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | A tenant's copy of platform-published content MUST be verified against the content ids fixed in the platform's reviewed build.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | A tenant's own writer MUST copy platform-published content from a release location that holds no tenant data.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | A tenant-authored module MUST NOT share a compiled form across tenants.
<!-- Allocation: B_S5#A_PLATFORM_CONTENT | runtime -->
Source: spec/core/encryption-and-blobs.md#platform-published-content-is-copied-per-tenant | Platform-published content such as decider modules, policy engines, and model weights MUST be copied into each tenant under that tenant's own key rather than shared as stored content across tenants.
<!-- Allocation: B_S5#A_BLOB_RECLAIM_ERRORS | runtime -->
Source: spec/core/encryption-and-blobs.md#reclamation-never-removes-a-referenced-blob | Owners and pins MUST be recorded from the first write.
<!-- Allocation: B_S5#A_BLOB_RECLAIM_ERRORS | runtime -->
Source: spec/core/encryption-and-blobs.md#reclamation-never-removes-a-referenced-blob | Reclaiming bytes MUST run after the write, at low priority, rather than as part of the write.
<!-- Allocation: B_S5#A_BLOB_RECLAIM_ERRORS | runtime -->
Source: spec/core/encryption-and-blobs.md#reclamation-never-removes-a-referenced-blob | The blob store MUST NOT reclaim a blob that any owner or pin references.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A blob MUST NOT be deduplicated across tenants.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A pin MUST NOT admit a holder of another tenant.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A pin MUST NOT outlive its owner's key.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A pin MUST record its scope, its holder class, and its own expiry, written by the owner's writer in the commit that gives out the reference.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A pin for a deleted owner MUST be refused.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A pin's name MUST NOT be reused.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | A read through a lapsed or missing pin MUST fail closed with the same not-found answer whether or not the blob exists.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | Deduplication inside a tenant, where it is shown to reduce cost, MUST be a tenant-owned class addressed by a keyed hash under a tenant key.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | Ownership MUST transfer only through the current owner.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | Sharing a blob across owners MUST be an explicit pin with a holder and an expiry, recorded through the owner before the reference is used.
<!-- Allocation: B_S5#A_BLOB_PINS | runtime -->
Source: spec/core/encryption-and-blobs.md#sharing-is-an-explicit-pin | The holder a read is checked as, and the pin it reads through, MUST be set by the platform from its own records, never from a payload, a decider input, a tool argument, or a program a tenant supplies.
<!-- Allocation: B_S5#A_BLOB_STORAGE | runtime -->
Source: spec/core/shared-framework.md#the-blob-store | The framework MUST provide the owner-scoped blob store, in which a blob is addressed by the hash of its ciphertext.

---

ID: C
Title: Tool dispatch and recovery
Prerequisites: AUTH, D, H, O, R, S, T
Deliverable: Committed dispatch, gated arguments/results, sending or beginning execution and semantics-specific recovery. Interface boundary: Definition/schema/hook admission, committed dispatch, result gating, semantics-specific recovery, close/reconcile, cancellation, late-result identity and native-service boundary.
Acceptance: Real effects never precede committed intent and required checks; recovery, late results, deadlines and usage retain call identity. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | inferred | Requires verified caller, assertion and authenticated transport interfaces for integration.
Dependency: D | inferred | Requires immutable loaded-definition and grant-check interfaces for integration.
Dependency: H | source-constraint | The quoted dispatch requirement orders hook passage before recording the call.
Dependency: O | inferred | Requires stable work identities, usage append and audit export interfaces for integration.
Dependency: R | inferred | Requires receiver admission, receipt and reconciliation interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
Dependency: T | inferred | Requires deadline, cancellation and occurrence interfaces for integration.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | A call MAY return before its work completes and deliver its result later as an inbox event.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | A call's horizon MUST be its deadline plus the configured outcome-unknown window.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | A final result that arrives after the horizon MUST be dropped with an audit event.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | A late result MUST be retried under one stable id.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | A late result MUST identify the call id of the dispatch it answers.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | Delivery of a late result MUST end at the first of the end of the call's session, the platform's refusal of the server's sends, or the call's horizon.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/contracts/tool-call.md#a-long-call-may-finish-later | Retries of a late result MUST end when the platform acknowledges it.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A reference returned for a result MUST also appear in the result's plain text.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A result MAY carry image, audio, and binary content, each typed by its media type and bounded by the configured size.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A result MUST reach the transcript as a bounded view whose bounds come from the tool spec.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A result part the platform does not model MUST reach the model as a textual reference carrying its address, name, and size rather than be dropped.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A result's structured content MUST be checked against the tool's output schema where the server declares one.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A server MAY return a result over its declared inline bound as a reference with a bounded preview and the result's shape.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A staged output MUST be usable only by the result of the dispatch that admitted it.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A tool server MAY deliver a result larger than the payload bound by staging it through the API as an attachment that its result event refers to.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A tool server MUST be allowed to send into a session only if that session has dispatched to the server, on the server's own topic and at a bounded priority.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | A tool server's right to stage an attachment MUST be bound to one open dispatch to that server, identified by the session and the call id in fields the server signs.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | The full output of a call MUST be stored once, so that a session can page through it.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | The platform MUST check that the dispatch is open to that server before it accepts any byte of the staged output.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/contracts/tool-call.md#a-result-is-bounded-in-the-transcript | The platform's answer to that check MUST reveal only whether the call is open to that server.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/contracts/tool-call.md#a-tool-error-is-a-result-not-a-failure | A blocked tool result MUST reach the model as an error result that states the call completed and its result was withheld, with the reason.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/contracts/tool-call.md#a-tool-error-is-a-result-not-a-failure | A deferred call that is dispatched MUST run under its original call id.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/contracts/tool-call.md#a-tool-error-is-a-result-not-a-failure | A deferred tool call MUST return an immediate result to the model that carries the decision id and the deadline.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/contracts/tool-call.md#a-tool-error-is-a-result-not-a-failure | A re-issue of a held side-effecting call with the same tool and the same arguments MUST return the same pending result and decision id rather than a second defer.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/contracts/tool-call.md#a-tool-error-is-a-result-not-a-failure | An error a tool returns MUST reach the model as an error result rather than as a failed turn.
<!-- Allocation: C_S4#C_TOOL_BOUNDED_MULTIMODAL | runtime -->
Source: spec/contracts/tool-call.md#additive-evolution-of-this-contract | A party MUST carry a content part it does not recognize through unchanged rather than reject the message.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/contracts/tool-call.md#an-interrupted-side-effect-reports-unknown | A side-effecting call interrupted before its outcome was recorded MUST return an unknown outcome that states what was sent and when.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#every-call-carries-its-identity | A tool server the operators run MUST continue the caller's trace under a span of its own and carry its trace id on every event it publishes for the call.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#every-call-carries-its-identity | Every call MUST carry a call id.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#every-call-carries-its-identity | Every call MUST carry the caller's assertion, addressed to the server.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#every-call-carries-its-identity | Every call MUST carry the caller's distributed trace context.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#every-call-carries-its-identity | Every call MUST carry the caller's lease generation.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#the-call-id-is-the-dedupe-key-end-to-end | A call a service makes on a session's behalf from inside a call MUST carry a call id derived from the outer call's id.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#the-call-id-is-the-dedupe-key-end-to-end | A re-issued side-effecting call that follows an unknown outcome MUST reuse the original call id.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/contracts/tool-call.md#the-call-id-is-the-dedupe-key-end-to-end | The call id MUST be given to the executor and to any target system that accepts an idempotency key.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A definition MUST only make a tool's semantics more conservative than the server declared.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A tool MAY declare a reconcile binding that reports whether an in-flight effect took effect.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A tool that is not declared cancellable MUST be treated as not cancellable.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A tool's semantics MUST default to the server's declared annotations.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A tool's semantics MUST include whether it is idempotent, whether it has side effects, its timeout, its retry policy, and whether it is cancellable.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-semantics | A tool's semantics MUST state whether a failed attempt may be removed from the model's view once a retry succeeds.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A server MUST announce a change to its list of tools.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A server MUST list its tools on request.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A tool's spec MAY declare the inline bound of its results, which a definition can only lower.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A tool's spec MAY declare the operational limits a caller respects, such as what the tool cannot do, its expected duration, and its rate bound.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A tool's spec MAY declare the scope a caller holds to call it.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | A tool's spec MUST consist of a name, a description, and an input schema, declared by the server that provides it.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-server-declares-the-spec | The platform MUST treat a spec as untrusted input, since its description enters the model's context.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-spec-declares-call-types-and-credentials | A spec MAY declare call-type tags that hook policies are written against.
<!-- Allocation: C_S4#C_TOOL_DISCOVERY_SEMANTICS | runtime -->
Source: spec/contracts/tool-call.md#the-spec-declares-call-types-and-credentials | A spec MUST request any credential it needs by credential reference rather than by value.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#an-unreachable-or-timed-out-tool | A side-effecting tool call whose server is unreachable or timed out MUST return an unknown outcome to the model.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#an-unreachable-or-timed-out-tool | An idempotent tool call whose server is unreachable or timed out MUST be retried within its budget.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/core/failure-handling.md#every-side-effect-can-be-redone | A tool call's idempotency key MUST be its call id.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/failure-handling.md#every-unit-of-work-has-a-deadline | Every tool call MUST have a deadline.
<!-- Allocation: C_S5#C_TOOL_SERVER_REQUESTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-server-may-ask-the-platform | A tool server's log and progress notifications MUST be published as events of the calling session.
<!-- Allocation: C_S5#C_TOOL_SERVER_REQUESTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-server-may-ask-the-platform | A tool server's request directed at a person MUST be delivered as a question to the session's principal with a deadline, or be refused.
<!-- Allocation: C_S5#C_TOOL_SERVER_REQUESTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-server-may-ask-the-platform | A tool server's request for a model completion MUST run as a model call of the calling session, under its deciders and its budget, or be refused.
<!-- Allocation: C_S5#C_TOOL_SERVER_REQUESTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-server-may-ask-the-platform | A tool server's request the platform does not support MUST be refused with a protocol-level error rather than left unanswered.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A call that answered with a non-final result MUST NOT be run again at its deadline.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A call that received a non-final result MUST keep its dispatch record open while its final result has not arrived and its deadline has not passed.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A dispatch record MUST stay matchable to a late result, by call id, through the call's horizon.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A final result that arrives after the horizon MUST be dropped with an audit event.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A late result for a call reported as outcome unknown MUST be delivered to the model as a notification identifying the call.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A late result for a call that already has a recorded result MUST be dropped as a duplicate.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A model output that asks for more calls than the cap leaves free MUST receive an error result for each extra call that states the cap, before any dispatch record commits.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A session MUST be evictable while a long tool run is in progress.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | A session's open dispatch records and its outcome-unknown records MUST each be bounded by a configured cap.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | At its deadline, a call that answered with a non-final result MUST be reconciled or reported as outcome unknown, by its semantics.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | At the outcome-unknown cap a side-effecting call MUST be refused with an error result while no record has passed its cutoff.
<!-- Allocation: C_S5#C_TOOL_LATE_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-session-may-be-evicted-while-a-tool-runs | Reaching the outcome-unknown cap MUST raise an alert.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A call the model makes to a tool the current configuration does not list MUST be answered with an error result rather than fail the step.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A connection or listing request to a tool server MUST be retried within the call's deadline, honoring a retry-after when one is given.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A revocation of a tool MUST apply at dispatch, so that a revoked tool cannot run even from a turn already in flight.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A server's announced change to its tool list MUST reach running sessions as live reconfiguration, within what the loaded definition allows.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A step whose server fails to list its tools MUST run with the server's last listing rather than fail.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A tool call in flight when its tool is removed MUST run to completion under its dispatch record.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | A transcript MUST remain renderable for every provider after a tool it references is removed.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | Adding a server to a definition MUST be a widening change.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | Every retry of a tool-server request MUST be an event carrying the server, the attempt, and the reason.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | The platform MUST list a server's tools again on every reconnect.
<!-- Allocation: C_S5#C_TOOL_RECONFIGURATION | runtime -->
Source: spec/core/tools-and-dispatch.md#changes-reach-sessions-as-reconfiguration | The result of a call whose tool was removed while it ran MUST be committed to the transcript.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A configuration that states a protocol revision the platform does not implement MUST be refused with the revisions the platform implements.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A configuration whose declared tools share a name MUST be refused when submitted.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A definition MUST list the tool servers, and optionally the tools on each, that its sessions may call.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A listing whose tool names do not fit the providers' name constraints MUST be refused with the reason.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A native tool MUST be offered to the model only when the session's configuration lists it.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | A tool name listed by more than one server MUST be published as an event identifying each server and which one is offered.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | Dispatch MUST refuse a call to a server the session's loaded definition does not list.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | Dispatch MUST refuse a call to a tool the session's loaded definition excludes.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | The platform MUST NOT keep a registry of tools apart from the definitions.
<!-- Allocation: C_S4#C_TOOL_ELIGIBILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-refuses-what-the-definition-does-not-list | The platform MUST NOT rely on a provider to enforce which tools a model has loaded.
<!-- Allocation: C_S5#C_TOOL_NATIVE_STATE | runtime -->
Source: spec/core/tools-and-dispatch.md#native-tools-act-only-on-the-platform-s-own-state | A function that does not act on the platform's own state MUST be provided by a service the definition lists.
<!-- Allocation: C_S5#C_TOOL_NATIVE_STATE | runtime -->
Source: spec/core/tools-and-dispatch.md#native-tools-act-only-on-the-platform-s-own-state | The platform MUST NOT implement natively a tool that does not act on its own state.
<!-- Allocation: C_S5#C_TOOL_NATIVE_STATE | runtime -->
Source: spec/core/tools-and-dispatch.md#native-tools-act-only-on-the-platform-s-own-state | The platform MUST implement natively the tools that act on its own state: subscribe and unsubscribe, publish and send, start a sub-session, fork a session, end a child, wait for a child's result, list a session's children, set the interrupt threshold, the timer operations, the key-value slot, loading a deferred tool the definition already lists, and paging a stored output.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A call whose outcome is not known at its cutoff MUST be recorded as outcome unknown rather than as not run.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A call's cutoff MUST be its first dispatch's deadline plus the configured outcome-unknown window, less a margin of the assertion lifetime plus twice the clock-skew allowance.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A re-issue of a call id MUST NOT move the call's cutoff.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A re-issue's own deadline MUST be cut to the cutoff.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A result returning to the platform MUST NOT be refused because it carries an older lease generation.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | A tool with a reconcile binding MUST be asked on recovery whether its effect took effect.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | An in-flight idempotent call found on recovery MUST run again.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | An outcome-unknown record MUST NOT stay matchable past the compaction that drops its turn.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | An outcome-unknown record MUST stay matchable for an identical re-issue through the call's cutoff.
<!-- Allocation: C_S4#C_TOOL_UNKNOWN_RECOVERY | runtime -->
Source: spec/core/tools-and-dispatch.md#recovery-follows-the-tool-s-semantics | The platform MUST NOT present a call id to a service, as a re-issue, a close, or a reconcile, after the call's cutoff.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#specs-are-loaded-hashed-and-checked | A call's arguments MUST be checked against the spec's schema at the tool-call hook.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#specs-are-loaded-hashed-and-checked | A dispatch record MUST carry the hash of the spec the model saw and the build the server reported.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#specs-are-loaded-hashed-and-checked | A tool spec MUST pass the input deciders when it loads.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#specs-are-loaded-hashed-and-checked | The arguments sent to a tool MUST be the arguments that passed the schema check, so that a transform's patch is checked before the call runs.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#specs-are-loaded-hashed-and-checked | The platform MUST hash each tool spec a session loads.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A blocked tool result MUST reach the model as an error result that states the call completed and its result was withheld, with the reason.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A page of a stored tool output MUST pass the tool-result hook before the model sees it.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A refusal for insufficient scope MUST reach the model as an error result that states the scope required.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A server's refusal for a missing credential, its refusal for insufficient scope, and its refusal of a superseded owner MUST be three distinct results.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A streamed chunk of a tool's output MUST NOT be committed as a result of its own.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool call MUST be resolved against the session's loaded definition before anything else.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool call MUST pass the tool-call hook before it is recorded.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool call's deadline MUST NOT be set later than the configured upper bound after the call's issue.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool call's output MUST be streamed while it runs.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool result MUST be committed as a state transition.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool result MUST pass the tool-result hook before the model sees it.
<!-- Allocation: C_S4#C_TOOL_RESULT_PIPELINE | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A tool result whose staged output is unavailable MUST be recorded as outcome unknown.
<!-- Allocation: C_S4#C_TOOL_CALL_IDENTITY | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | Every call the platform makes to a tool server MUST carry the session's distributed trace context.
<!-- Allocation: C_S4#C_TOOL_GOVERNED_BOUNDARY | runtime -->
Source: spec/core/tools-and-dispatch.md#the-sandbox-is-not-a-tool-executor | The sandboxed program runtime MUST be used for deciders rather than as a tool executor.
<!-- Allocation: C_S4#C_TOOL_EXECUTOR_KINDS | runtime -->
Source: spec/core/tools-and-dispatch.md#three-executor-kinds | The platform MUST support native tools, tool-protocol servers, and plain request-response servers as executors.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A service MAY exchange a call's assertion for credentials of its own at the credential broker.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A service MAY send into a session in its own tenant only while that session has dispatched to the service, on the service's own topic and at a bounded priority.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A session's own publish or send whose payload exceeds the bound MUST be refused at the tool-call hook with an error result that states the bound.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A staged output MUST be usable only by the result of the dispatch that admitted it.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A tool server MAY deliver a result larger than the payload bound by staging it through the API as an attachment that its result event refers to.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | A tool server's right to stage an attachment MUST be bound to one open dispatch to that server, identified by the session and the call id in fields the server signs.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | The answer to that check MUST reveal only whether the call is open to that server.
<!-- Allocation: C_S5#C_TOOL_STAGED_OUTPUT | runtime -->
Source: spec/core/tools-and-dispatch.md#what-every-service-receives | The platform MUST check that the dispatch is open to that server before it accepts any byte of the staged output.
<!-- Allocation: C_S4#C_TOOL_GOVERNED_BOUNDARY | runtime -->
Source: constitution.md#tools-execute-through-governed-interfaces | The platform MUST NOT execute tenant-supplied code except as a decider running in the sandbox or through governed tool execution.
<!-- Allocation: C_S4#C_TOOL_GOVERNED_BOUNDARY | runtime -->
Source: constitution.md#tools-execute-through-governed-interfaces | A tool MUST execute through a governed interface separate from the agent's reasoning loop.
<!-- Allocation: C_S4#C_TOOL_HOSTING_PERMISSIONS | runtime -->
Source: constitution.md#tools-execute-through-governed-interfaces | A tool MAY be hosted within the platform.
<!-- Allocation: C_S4#C_TOOL_HOSTING_PERMISSIONS | runtime -->
Source: constitution.md#tools-execute-through-governed-interfaces | A tool MAY be provided by an external service.
<!-- Allocation: C_S4#C_TOOL_DISPATCH_ORDER | runtime -->
Source: spec/core/tools-and-dispatch.md#the-dispatch-sequence | A dispatch record MUST be committed before the call is sent or begins execution.

---

ID: CFG
Title: Configuration admission and adoption
Prerequisites: D, H, S
Deliverable: Bootstrap, governed revisions, validation and lazy session/template adoption. Interface boundary: One-time tenant bootstrap, validated effective configuration, turn-boundary adoption, change classification, catalogs and nonauthor governance approval.
Acceptance: Ordinary changes pass governance, retain revocation rules and apply only at permitted boundaries; template changes have constant write cost. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: D | inferred | Requires immutable loaded-definition and grant-check interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#p2-behavior-is-configuration | Policies, prompts, tools, models, deciders, thresholds, and triggers MUST be data the core reads rather than code the core carries.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#p2-behavior-is-configuration | The core MAY carry safe defaults for configuration a tenant has not supplied.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: constitution.md#p3-change-without-redeploying | A behavior change MUST take effect in a running session at its next turn boundary without a redeploy.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#p3-change-without-redeploying | A code change MUST be required only for a new mechanism, such as a new executor kind or a new hook point.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#p4-generic-over-the-domain | Meaning MUST be given to a topic, a payload, a tool, or a call type by an adapter rather than by the core.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#p4-generic-over-the-domain | The core MUST NOT interpret a topic, a payload, a tool, or a call type.
<!-- Allocation: CFG_S3#C_CONFIGURED_DOMAIN_CORE | runtime -->
Source: constitution.md#tools-execute-through-governed-interfaces | An agent definition MUST be data the tenant supplies rather than code written into the platform.
<!-- Allocation: CFG_S5#A_RUNTIME_SETTINGS | runtime -->
Source: spec/core/durability-and-deployment.md#platform-limits-are-runtime-settings | A change to a runtime setting MUST be published as an event identifying the operator and the setting.
<!-- Allocation: CFG_S5#A_RUNTIME_SETTINGS | runtime -->
Source: spec/core/durability-and-deployment.md#platform-limits-are-runtime-settings | A limit on the platform's own operation, such as a cap, a cadence, a timeout, or a pool size, MUST be a runtime setting rather than a constant fixed in code.
<!-- Allocation: CFG_S5#A_RUNTIME_SETTINGS | runtime -->
Source: spec/core/durability-and-deployment.md#platform-limits-are-runtime-settings | A setting that takes effect only when a node starts MUST be node configuration rather than a runtime setting.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | A change's authored-for set MUST include every human principal in the session's chain and the requester.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | A mechanism that gates an action on a person's decision MUST act only on a recorded positive decision whose fields match the action, never on the absence of a block.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | An automated approver admitted by policy for agent-authored work MUST be a deterministic mechanism rather than an agent or a model judge.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A configuration change MUST be classified as widening, narrowing, or neutral by comparing the effective configuration before and after it, each composed with the tenant's defaults in force at the time of the comparison.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A definition version that has no predecessor MUST be compared with the empty definition.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A field that no classification rule covers MUST be treated as widening.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A longer idle time before a keyed instance ends, or the removal of an end condition, MUST be classified as widening.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A narrowing or neutral change MAY apply by default.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A policy change MUST pass automated analysis before it can enforce.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A policy change declared as narrowing-only MUST prove it with a sound checker.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A policy on the control-operation hook MUST be able to match on the operation's kind and tier.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A session principal MUST NOT write at the governance-policy tier.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A tool that writes a definition MUST declare the definition-write call type.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | A widening change MUST defer as a whole to the approver the tenant's governance designates, unless the binding or grant under which the change arrives is trusted for that class of change.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | Content the core produces or admits itself MUST carry a reserved call-type tag per kind, for a model's text output, a tool spec at load, and a definition version's content at admission.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | Every configuration change, including a session's own deciders, MUST be a control operation that passes the control-operation hook.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | The default policy MUST deny a session any governance-tier write whose scope reaches its own session, its definition, or an ancestor's definition, including the catalog entries those definitions reference.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | The default policy MUST deny a session weakening its own governance.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | The tool-call hook MUST be able to deny a session writing its own definition or an ancestor's.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/governance.md#changing-configuration-passes-a-hook | Version metadata, such as the version number, the parent digest, and the list of restrictive fields, MUST NOT enter the comparison.
<!-- Allocation: CFG_S5#B_CFG_CATALOG | runtime -->
Source: spec/core/governance.md#decider-code-enters-through-the-catalog | A catalog entry MUST declare the decider's kind, its artifact by content id, the hooks it may run at, its call-type scope, the decision set it can return, what part of the request it reads, and whether it is stateless, stateful, or temporal.
<!-- Allocation: CFG_S5#B_CFG_CATALOG | runtime -->
Source: spec/core/governance.md#decider-code-enters-through-the-catalog | A decider's code, model, or weights MUST enter the platform only as a catalog entry written by a governance-tier operation and pinned by version.
<!-- Allocation: CFG_S5#B_CFG_CATALOG | runtime -->
Source: spec/core/governance.md#decider-code-enters-through-the-catalog | A definition MAY carry decider data such as patterns, criteria, and policies.
<!-- Allocation: CFG_S5#B_CFG_CATALOG | runtime -->
Source: spec/core/governance.md#decider-code-enters-through-the-catalog | A definition's decider addition MAY only narrow that entry's scope, tighten its failure outcome, or require that a truncation denies.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#people-approve-policies-not-instances | A class of change MUST be governed by a gate spec that states the evidence under which it may proceed.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#people-approve-policies-not-instances | A gate spec MUST be approved once.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#people-approve-policies-not-instances | A gate spec MUST decide on reproducible evidence bound to the artifact.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/governance.md#people-approve-policies-not-instances | A gate spec MUST run where the agent cannot access or modify it.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A bootstrap bundle MUST NOT be installed again over a live tenant.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A bootstrap bundle MUST carry the tenant's governance-policy set, the grants for the emergency controls, the principals allowed to invoke the emergency override with their quorum and the override's expiry, the call-type tags of the never-automate set, and the tenant's defaults for the definition fields a version omits.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A bootstrap bundle's digest MUST be recorded with the tenant.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A definition field a version omits MUST take the tenant's default.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A policy replacement made by emergency override MUST expire after the configured period unless it has been re-made through the governance-policy tier.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A replaced policy MUST be kept as a version.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A stage MUST refuse every governance-tier and governance-policy-tier policy write while its emergency-override path is not in service.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | A tenant's bootstrap bundle MUST be the only configuration installed outside the governance pipeline other than through the emergency-override path.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | An emergency override MUST be invoked only by principals the bootstrap bundle lists, as many as the configured quorum.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | An emergency override MUST only replace, remove, or restore a policy at the governance or governance-policy tier.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | An emergency override principal MUST be a person authenticated directly rather than attested by a service.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | An emergency-override path, checked by the core itself and fully audited, MUST be able to replace a policy that blocks governance from operating.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | At expiry the replaced policy MUST be restored with an event.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | Each invoking principal MUST confirm the same replacement by its digest within the configured window.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | Each use of the emergency override MUST alert every operator of the tenant through the tenant's registered destinations.
<!-- Allocation: CFG_S5#B_CFG_EMERGENCY_OVERRIDE | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | Each use of the emergency override MUST be an audit event that identifies the caller, the chain, the reason, and the replaced and new policies by digest.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | Governance policy itself MUST change only through a tier above ordinary governance.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | Installing a bootstrap bundle MUST be an audit event.
<!-- Allocation: CFG_S5#B_CFG_CATALOG | runtime -->
Source: spec/core/governance.md#two-paths-sit-outside-the-pipeline | The module that implements a substrate check MUST be pinned by the platform outside any tenant's catalog.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | A change MUST record whom it was authored for.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | A person who approves a change MUST NOT be in the change's authored-for set.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A configuration change MUST NOT apply between a tool call and the delivery of its result.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A configuration change MUST NOT wake an idle instance to adopt it.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A configuration change MUST apply to a running session at its next turn boundary.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A configuration change to a keyed template MUST cost a number of durable writes that does not depend on the number of its instances.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A partition MUST NOT create a keyed instance while a stop holds at its template's or tenant's scope.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A partition MUST apply an emergency control to a non-resident session when it next loads, before it starts a turn.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-change-applies-at-the-next-turn-boundary | A session MUST check for a pending configuration change when it loads, as it does at a turn boundary.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-prefix-change-preserves-the-cache | A field the model never sees MUST apply immediately.
<!-- Allocation: CFG_S6#B_TENANT_RETENTION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#retention-is-per-tenant | Audit metadata MUST be retained longer than content.
<!-- Allocation: CFG_S6#B_TENANT_RETENTION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#retention-is-per-tenant | Copies of episode content MUST be a retention kind of their own in the tenant's configuration.
<!-- Allocation: CFG_S6#B_TENANT_RETENTION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#retention-is-per-tenant | Retention MUST be tenant configuration, per kind of data.
<!-- Allocation: CFG_S3#B_CFG_BOOTSTRAP | runtime -->
Source: spec/core/the-frontend.md#onboarding-installs-the-bootstrap-bundle | A tenant's bootstrap bundle MUST be installed when the tenant is created.
<!-- Allocation: CFG_S5#A_SCHEDULE_AUTHORITY_CHANGE | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | Adding a schedule, shortening a schedule's interval, removing a guard, or raising a schedule's priority MUST be classified as a widening change.
<!-- Allocation: CFG_S5#A_SCHEDULE_AUTHORITY_CHANGE | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | Removing a schedule or adding a guard MUST be classified as a narrowing change.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/decisions/authority-widening/checkers-only.md#a-gate-spec-is-approved-by-a-person-other-than-the-author | A gate spec MUST be approved by a person who is not an author of the changes it governs.
<!-- Allocation: CFG_S1#B_GOVERNANCE_APPROVAL_BOUNDARY | governance -->
Source: spec/decisions/authority-widening/checkers-only.md#a-person-changes-policy-not-instances | A person MUST change what agents may do only by changing policy through the control-operation hook.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#a-policy-holds-settings-a-session-inherits | A change to a policy MUST apply to each session that lists it at that session's next turn boundary.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#a-policy-holds-settings-a-session-inherits | A root session's configuration MAY list policies, whose settings apply in list order with the session's own fields last.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#configuration-is-validated-and-governed | A configuration change that widens authority MUST defer to the approver the tenant's governance identifies, unless the caller's grant covers that class of change.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#configuration-is-validated-and-governed | A configuration that fails validation MUST be refused with a reason.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#configuration-is-validated-and-governed | A refused reconfiguration MUST leave the previous configuration in place.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#configuration-is-validated-and-governed | A root session's configuration MUST be validated against the configuration schema and the tenant's upper bounds at creation and at reconfiguration.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#configuration-is-validated-and-governed | A root session's configuration MUST pass the input deciders.
<!-- Allocation: CFG_S5#A_CONFIG_ADOPTION | runtime -->
Source: spec/decisions/session-management/imperative.md#reconfiguration-applies-at-a-turn-boundary | A reconfigure control operation MUST replace a root session's configuration.
<!-- Allocation: CFG_S3#C_CHARTER_SOURCE_FORM | governance -->
Source: spec/core/governance.md#a-charter-has-normative-requirements | A charter's behavioral requirements MUST use the normative key words with the meanings defined by the constitution.
<!-- Allocation: CFG_S3#C_CHARTER_SOURCE_FORM | governance -->
Source: spec/core/governance.md#a-charter-has-normative-requirements | Each charter requirement MUST be a self-contained sentence carrying one obligation under a stable heading.
<!-- Allocation: CFG_S5#MODEL_CONFIGURATION_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#the-platform-keeps-each-model-s-facts | A session configuration that requires a capability the model's record lacks MUST be refused when the configuration is submitted.
<!-- Allocation: CFG_S5#MODEL_CONFIGURATION_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#the-platform-keeps-each-model-s-facts | A model id whose provider the platform does not know MUST be refused when the configuration is submitted.
<!-- Allocation: CFG_S5#B_CFG_CLASSIFICATION | runtime -->
Source: spec/core/tools-and-dispatch.md#calls-leave-through-one-egress-point | A destination given as a numeric literal in a non-canonical form MUST be refused when the configuration is submitted.

---

ID: CO
Title: Partition coordination and routing
Prerequisites: AUTH, DA
Deliverable: Coordinator membership, durable assignments, ownership fencing and forwarded calls. Interface boundary: Partition identity and placement generation, fenced authority transfer, current-owner routing and authenticated forwarded caller context.
Acceptance: Concurrent declarations yield one coordinator/owner; stale owners fail; routing and waiting replies survive moves. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | inferred | Requires verified caller, assertion and authenticated transport interfaces for integration.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
<!-- Allocation: CO_S4#A_FORWARD_CALLER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-forwarded-call-carries-the-caller | A call that one node forwards to the owner of another partition MUST carry the caller's verified principal and chain unchanged in a claim of its own.
<!-- Allocation: CO_S4#A_FORWARD_CALLER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-forwarded-call-carries-the-caller | A forwarded call MUST be authenticated by an assertion of the forwarding node that a verifier checks against the keys of its own lineage only.
<!-- Allocation: CO_S4#A_FORWARD_CALLER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-forwarded-call-carries-the-caller | The audit record of a forwarded operation MUST carry both the forwarding node and the caller's verified principal and chain.
<!-- Allocation: CO_S4#A_FORWARD_CALLER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-forwarded-call-carries-the-caller | The identity a forwarding node presents MUST hold only the scope of the kind of hop it makes.
<!-- Allocation: CO_S4#A_FORWARD_CALLER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-forwarded-call-carries-the-caller | The owner of the partition MUST decide a forwarded operation on the caller's verified principal and chain rather than on the identity of the node that forwarded it.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-s-partition-is-a-function-of-its-id | A session's partition MUST be determined by the leading bits of a hash of its session id, with the bit count taken from configuration.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-s-partition-is-a-function-of-its-id | A split of the partition space MUST move no session's durable data.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-s-partition-is-a-function-of-its-id | Moving a partition between nodes MUST NOT move a session's durable data.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-s-partition-is-a-function-of-its-id | The hash that maps a session id to its partition MUST be part of the durable format, so that changing it is a migration.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A coordinator restart MUST NOT stop a running partition.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A coordinator whose membership view has lost more than the configured fraction of the current owners MUST hold its moves while the view stays that way, within the configured bound.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A draining node MUST release its partitions to the coordinator before it exits rather than wait for its membership lease to lapse.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A new coordinator's first pass MUST wait for a fresh membership view.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A node MUST drain the partitions reassigned away from it.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A node MUST start the partitions it is assigned with the table's generation as their instance generation.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A node MUST start, after the configured grace, any partition assigned to it that is not running, fenced on the generation it observed.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A node that reconnects MUST NOT take a partition over from a host it believes dead before its own membership view has settled.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A node the failure detector declares dead MUST have its partitions reassigned.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | A planned drain MUST be an input to the placement policy.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | Adding a node MUST move only the partitions the placement policy gives to it.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | An orphaned or never-started partition MUST be placed at once.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | Partitions MUST be assigned to nodes by one coordinator per partitioned set, from one consistent view of membership.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | Removing a node MUST move only the partitions it owned.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | The assignment table MUST be durable.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | The assignment table MUST record each partition's node and generation.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/sessions-and-partitions.md#placement-comes-from-a-coordinator | The coordinator MUST check its own generation in the same commit as each batch of moves.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#the-space-splits-by-doubling | A session MUST move only from a partition to one of that partition's two children.
<!-- Allocation: CO_S3#A_PARTITION_HASH_SPLIT | runtime -->
Source: spec/core/sessions-and-partitions.md#the-space-splits-by-doubling | Growing the partition space by one bit MUST split each partition into exactly two children.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/shared-framework.md#the-partitioned-actor-set | A partition's ownership MUST be fenced by generation.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/shared-framework.md#the-partitioned-actor-set | The actor set MUST support draining a node's partitions.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/shared-framework.md#the-partitioned-actor-set | The coordinator MUST assign partitions to nodes by a pluggable placement policy.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/shared-framework.md#the-partitioned-actor-set | The coordinator MUST stay off the critical path.
<!-- Allocation: CO_S3#A_COORDINATOR_PLACEMENT | runtime -->
Source: spec/core/shared-framework.md#the-partitioned-actor-set | The framework MUST provide a partitioned actor set with one coordinator per set.

---

ID: D
Title: Authority and loaded definitions
Prerequisites: AUTH, DA, SF
Deliverable: Tenant-qualified principals, grant chains and immutable loaded definition eligibility. Interface boundary: Tenant-qualified immutable loaded definitions, local narrowing grants and lineage revocation; conditional definition-source contract remains inactive under imperative-only.
Acceptance: Real stored authority survives recovery; narrowing, ancestor revocation and tool eligibility hold for verified callers. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | inferred | Requires verified caller, assertion and authenticated transport interfaces for integration.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
Dependency: SF | inferred | Requires sealing, key lifecycle and compatible durable codecs for integration.
<!-- Allocation: D_S3#C_AGENT_AUTHORITY | runtime -->
Source: constitution.md#agents-never-approve | An agent MUST NOT hold a capability to approve a change.
<!-- Allocation: D_S3#C_AGENT_AUTHORITY | runtime -->
Source: constitution.md#agents-never-approve | Authority MUST be widened only by a sound checker or by a person the tenant's governance designates.
<!-- Allocation: D_S3#C_AGENT_AUTHORITY | runtime -->
Source: constitution.md#identity-comes-from-authentication | Delegated authority MUST only narrow from grantor to grantee.
<!-- Allocation: D_S3#C_AGENT_AUTHORITY | runtime -->
Source: constitution.md#security-guarantees-are-never-downgradable | The fail-closed guarantee MUST NOT be reducible to a warning by any configuration.
<!-- Allocation: D_S3#C_AGENT_AUTHORITY | runtime -->
Source: constitution.md#security-guarantees-are-never-downgradable | The tenant boundary MUST NOT be reducible to a warning by any configuration.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-definition-only-restricts | A definition's base priority MUST NOT exceed its binding's upper bound.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-definition-only-restricts | A definition's own deciders and policies MUST only restrict what the tenant's governance allows.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-source-is-bound-to-one-tenant | A source MUST serve definitions for exactly one tenant per binding.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-source-is-bound-to-one-tenant | A source MUST serve this contract over its well-known interface rather than over the tool protocol.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-source-is-bound-to-one-tenant | The tenant of a definition MUST come from the binding rather than from the served payload.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#a-source-may-announce-its-revision | A source MAY publish its new revision through the platform's event envelope as well as through the feed.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A client MUST keep an accepted version as the bytes it received.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A client MUST treat a known digest served with a version number other than its own as a source fault without fetching it.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A client MUST verify a version's digest by hashing the bytes it received rather than by re-serializing a decoded value.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A definition's version numbers MUST strictly increase.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A source MUST serve the bytes of a version by its digest for as long as any session may pin it.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A version MUST NOT change once it has been served.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A version MUST be addressed by the digest of its canonical serialization.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A version MUST carry the digest of its parent version, or state that it has none.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A version body MUST carry its version number, so that one digest identifies exactly one version.
<!-- Allocation: D_S1#B_DEFINITION_IMMUTABILITY | design -->
Source: spec/contracts/definition-source.md#a-version-is-immutable-and-addressed-by-digest | A version number MUST NOT recur with a different digest, including after the source restores from a backup.
<!-- Allocation: D_S1#B_DEFINITION_ENCODING | design -->
Source: spec/contracts/definition-source.md#additive-evolution-of-this-contract | A counter chosen by a party outside the platform MUST be a decimal string.
<!-- Allocation: D_S1#B_DEFINITION_ENCODING | design -->
Source: spec/contracts/definition-source.md#additive-evolution-of-this-contract | Each field's encoding MUST be declared by this contract rather than chosen per value.
<!-- Allocation: D_S1#B_DEFINITION_ENCODING | design -->
Source: spec/contracts/definition-source.md#additive-evolution-of-this-contract | Each integer field of this contract MUST be declared either a number within the declared exact-integer bound, refused above that bound on encode and on decode, or a decimal string under one canonical grammar.
<!-- Allocation: D_S1#B_DEFINITION_PARITY | design -->
Source: spec/contracts/definition-source.md#one-suite-runs-against-both-sides | A conformance suite for this contract MUST run against a source implementation and against the platform's client.
<!-- Allocation: D_S1#B_DEFINITION_PARITY | design -->
Source: spec/contracts/definition-source.md#one-suite-runs-against-both-sides | The conformance suite MUST check that a source implementation independent of the platform's encoder produces canonical bytes whose digest equals the digest the platform computes for the same value.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#only-a-served-retirement-ends-sessions | A client MUST treat a definition missing from the index as held rather than retired.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#only-a-served-retirement-ends-sessions | What a source serves MUST end a definition's sessions only when it serves the definition as retired.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#pause-is-served-not-inferred | A source MUST serve a paused intent as the only way to pause a definition's sessions.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A change feed MUST support a long-poll that answers as soon as an entry above the given revision exists.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A change feed asked to resume from a revision the source no longer holds, whether because it restored from a backup or because the revision has passed the feed's retention, MUST answer a distinct gone status, so that the client reads the whole index again.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A change-feed entry MUST carry every field an index entry carries.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A change-feed entry for a definition that left the index MUST carry the revision, the definition's identity, and an absent marker.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A client MUST advance its cursor to the revision an empty long-poll answer carries.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A long-poll that expires with no entry MUST answer with no entries and the feed's current revision.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A source MUST serve a change feed that resumes from any revision the client specifies.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-change-feed-resumes-from-any-revision | A source that restores from backup MUST set its revision above the highest it ever served before serving again.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A definition's instancing MUST NOT change once a version has been accepted for it.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A served lifecycle intent MUST apply to its definition whatever became of the version served beside it.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A served rollout fraction MUST apply only to the version whose digest the entry carries.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A source MUST NOT serve a revision lower than one it has served before.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A source MUST serve an index that is a consistent snapshot of every definition it serves, at one revision.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | A source MUST support a long-poll on the index that returns when the revision changes or when the configured bound elapses.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | An entry whose lifecycle, instancing, or rollout value lies outside its closed set MUST be treated as a source fault from which nothing applies.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | An index MUST carry a token a client can present to ask only whether the revision has changed.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | An index entry MUST identify the definition and carry its version number, the digest of its current version, its lifecycle intent, its instancing, and the rollout fraction for its current version.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | An index entry for a paused singleton MAY carry a handback listing the event ids and dropped ranges the source has taken back since the pause, cumulative since the pause.
<!-- Allocation: D_S1#B_DEFINITION_INDEX_FEED | design -->
Source: spec/contracts/definition-source.md#the-index-is-a-consistent-snapshot-at-a-rising-revision | The lifecycle intent, the instancing, the rollout fraction, and any handback MUST sit outside the version's digest, so that a change to one of them creates no new version.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#tools-that-write-definitions-declare-it | A tool that writes a definition MUST declare the definition-write call type on its spec.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST carry its prompt components, model and parameters, tool server references, decider additions, budget caps, deadlines, priority, standing subscriptions, standing schedules, compaction settings, cache settings, observation settings, and hook destinations by name.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST reference a hook destination by a name registered with the tenant rather than by an arbitrary address.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST reference a tool server by a name registered with the tenant rather than by an arbitrary address.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST specify a key only as a label drawn from the tenant's permitted set.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST state its instancing as singleton, keyed by requester, or keyed by a publisher's key.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A definition MUST state its lifecycle intent as one of run, paused, or retired.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-carries | A version body and its components MUST stay within the configured size limits.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-never-carries | A definition MUST NOT carry a credential or a secret.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-never-carries | A definition MUST NOT carry a grant.
<!-- Allocation: D_S1#B_DEFINITION_BINDING_BODY | design -->
Source: spec/contracts/definition-source.md#what-a-definition-never-carries | A definition MUST NOT carry a principal.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-grant-is-data | A grant MUST record who delegates, to which session or definition, which actions and resources, its expiry, its conditions, and its tenant.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-grant-is-data | A grant to an automated identity MUST enumerate each allowed action rather than use a wildcard.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-grant-is-data | Checking a grant MUST be a local evaluation rather than a network call.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-grant-is-data | Deleting a grant MUST revoke it at the next decision.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-grant-is-data | Every grant MUST be visible to audit and to analysis.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-shared-session-acts-for-the-requesting-principal | A decision for a session acting on another principal's event MUST carry the requester's principal and chain.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-shared-session-acts-for-the-requesting-principal | A requester MUST NOT obtain, through the session, authority the requester does not hold.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#a-shared-session-acts-for-the-requesting-principal | The effective authority of a session acting for a requester MUST be the intersection of the session's grants and the requester's.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | A grant from an operator MUST NOT exceed what that operator holds or the role's upper bound.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | A sub-session's budget MUST be allocated out of its parent's.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | A sub-session's grants MUST be references to its parent's rather than copies.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | Every decision MUST traverse the chain of grant references.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | Revoking an ancestor's grant MUST revoke its descendants'.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#delegation-only-narrows | The model-request hook MUST check the shared budget before every call.
<!-- Allocation: D_S3#B_GRANTS_AND_DELEGATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#enforcement-is-at-the-fixed-hooks | A hook that cannot obtain an authorization decision MUST fail closed.
<!-- Allocation: D_S6#B_GRANT_CONSUMER_INTEGRATION | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#enforcement-is-at-the-fixed-hooks | Identity and authorization MUST be enforced at the control-operation, tool-call, output, and tool-result hooks.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#a-lookup-reveals-nothing-across-tenants | A uniqueness violation on an index that spans tenants MUST be an internal error rather than a refusal the caller sees.
<!-- Allocation: D_S6#B_TENANT_READ_SCOPES | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#a-reader-passes-a-scope-check | A consumer or derived store MUST pass a read check against the access scope before it reads.
<!-- Allocation: D_S6#B_TENANT_READ_SCOPES | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#a-reader-passes-a-scope-check | Delivery to a viewer MUST enforce the read check.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A grant MUST cover only its own tenant's sessions and resources.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A node's credentials for the object store and the key service MUST be scoped to the tenants the node hosts, so that a compromised node reaches only those tenants.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A partition MUST publish the refusal as an audit event.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A partition MUST refuse an inbox entry whose recorded publisher tenant differs from the receiving session's tenant.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A session's send MUST address its target only through a form the platform computes with the sender's own tenant: a template with its key, the session's parent, or a sub-session the session started.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A store client MUST refuse an operation whose tenant does not match the credentials it was handed.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | A topic and an observation queue MUST be scoped to one tenant.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#authority-does-not-extend-beyond-the-tenant | The platform's own operators MUST reach a tenant's sessions through grants, as every other principal does.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#nothing-derived-crosses | A content cache MUST be keyed by tenant.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#nothing-derived-crosses | Content shared between tenants MUST be published on purpose rather than reach the other tenant through a derived store.
<!-- Allocation: D_S6#B_TENANT_ISOLATION | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#nothing-derived-crosses | Labels, episodes, and trained models MUST stay within their tenant.
<!-- Allocation: D_S6#B_TENANT_READ_SCOPES | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#scopes-propagate-as-unions | A transcript range, a compaction, and an observation record MUST carry the union of its inputs' access scopes.
<!-- Allocation: D_S3#C_CHARTER_SOURCE_IDENTITY | runtime -->
Source: spec/core/governance.md#a-revision-identifies-authoritative-source | A charter revision reference MUST resolve to the exact authoritative source of that revision under the applicable access and retention rules.
<!-- Allocation: D_S3#C_CHARTER_SOURCE_IDENTITY | runtime -->
Source: spec/core/governance.md#a-revision-identifies-authoritative-source | A charter revision reference MUST reuse an existing source identity when that identity resolves the complete authoritative charter source.
<!-- Allocation: D_S3#C_CHARTER_SOURCE_IDENTITY | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | A charter clause reference MUST resolve to the clause's source file, section, and exact sentence within the identified charter revision.

---

ID: DA
Title: Self-durable transactional store
Prerequisites: SIM
Deliverable: Real store adapter, including its own log, snapshots, indexes and backup/restore. Interface boundary: Snapshot/read validation, atomic commit and chosen precommit/lost-reply outcomes, append/trim/watch and fencing; state which storage survives each failure and what a committed reply promises.
Acceptance: Committed state survives process restart and loss of every node at once without losing committed state; the persistent-storage and fault boundary is explicit. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: SIM | inferred | Requires explicit time, transaction and injectable-fault interfaces for integration.
<!-- Allocation: DA_S2#A_STORE_SELF_DURABILITY | runtime -->
Source: spec/core/shared-framework.md#durable-store-durability | The durable store MUST have a defined backup and restore.
<!-- Allocation: DA_S2#A_STORE_SELF_DURABILITY | runtime -->
Source: spec/core/shared-framework.md#durable-store-durability | The framework MUST provide the durable store with the durable snapshot, the durable index, and the log that the adopted durability outcome requires.
<!-- Allocation: DA_S2#A_STORE_SELF_DURABILITY | runtime -->
Source: spec/decisions/durability/self-durable.md#a-crash-loses-no-committed-state | A crash MUST lose no committed state.
<!-- Allocation: DA_S2#A_STORE_SELF_DURABILITY | runtime -->
Source: spec/decisions/durability/self-durable.md#a-new-cluster-restores-from-the-store-s-backup | A procedure for backing up the durable store and restoring a cluster from the backup MUST be defined.
<!-- Allocation: DA_S2#A_STORE_SELF_DURABILITY | runtime -->
Source: spec/decisions/durability/self-durable.md#the-store-survives-the-loss-of-every-node | The durable store's own write-ahead log, snapshot, and index MUST be the durability mechanism.

---

ID: DR
Title: Whole-platform drain and restore
Prerequisites: API, B, D, DV, ED, M, O, R, SF, SR, T, X
Deliverable: Epoch fencing, ordered drain, complete tenant restore and release drills. Interface boundary: Conditional replacement-cluster epoch fencing, lossless ordered drain, tenant-complete restoration, key/authority/emergency controls before resume and preserved dedupe/identities.
Acceptance: All durable domains restore from the qualified source; authority/controls precede resume; identifiers, dedupe and audit/usage delivery survive replacement. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: API | inferred | Requires authenticated controls and current-owner routing interfaces for integration.
Dependency: B | inferred | Requires owned blob, pin and reclamation interfaces for integration.
Dependency: D | source-constraint | The quoted restore requirement orders authority restoration before any session resumes.
Dependency: DV | inferred | Requires actual store parity and failure-survival evidence for integration.
Dependency: ED | inferred | Requires canonical event delivery and subscription interfaces for integration.
Dependency: M | inferred | Requires model interruption and committed-loop recovery interfaces for integration.
Dependency: O | inferred | Requires stable work identities, usage append and audit export interfaces for integration.
Dependency: R | inferred | Requires receiver admission, receipt and reconciliation interfaces for integration.
Dependency: SF | inferred | Requires sealing, key lifecycle and compatible durable codecs for integration.
Dependency: SR | inferred | Requires cluster-independent resource and signing-root interfaces for integration.
Dependency: T | inferred | Requires deadline, cancellation and occurrence interfaces for integration.
Dependency: X | inferred | Requires owned transcript ranges and freeze/thaw interfaces for integration.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | A drain MUST NOT complete before every emergency control in force is held by the restore source.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | A drain MUST be reversible before the switch to the new cluster, so that the draining cluster resumes serving when the replacement is not accepted.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | A model turn still in flight at the drain deadline MUST be recorded as interrupted.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | A partition MUST freeze after it has applied what is pending.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | On a drain, a partition MUST apply what is pending.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | On a drain, an in-flight model turn MUST be assigned the drain deadline.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | On a drain, final records MUST be written before storage terminates.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#a-drain-is-orderly | On a drain, the frontend MUST refuse every state-changing request other than an emergency control with a retryable status, so that a retried request is deduplicated by its id.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#a-record-variant-is-readable-before-it-is-written | A deployment MUST NOT roll back below the version that reads a record variant while a record of that variant exists.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#a-record-variant-is-readable-before-it-is-written | A new variant of a durable record MUST NOT be written before every reader of that record kind runs code that reads it.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#a-record-variant-is-readable-before-it-is-written | A reader of many records MUST NOT fail the whole read because one record is of a kind it does not know.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A consumer of a change feed whose position is inside the range a restore lost MUST reset and record a gap rather than replay.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A new cluster MUST restore every session from the restore source the adopted durability outcome provides.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A partition whose restore fails MUST be published as an event with its reason.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A pin whose own expiry had passed at the drain MUST keep that expiry.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore MUST NOT cause an identifier issued before the restore to be issued again.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore MUST apply a tenant's authority records before any of the tenant's sessions resumes.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore MUST apply the tenant's emergency controls before any session resumes.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore MUST bring a tenant back whole: its configuration, its session index, and its sessions.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore MUST start the cluster's partitions only once every record is written back.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restore failure of one partition MUST NOT stop the node from starting the partitions it restored.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restored recurring timer's missed occurrences MUST follow the missed-occurrence rule at a paced rate.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restored session MUST receive an event carrying the time of the record it was restored from.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | A restored session MUST resolve its keys when it next wakes, at a paced rate.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | Every partition MUST open within the configured bound of the restore's open point.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/core/durability-and-deployment.md#a-restore-resumes-every-session | The dedupe record of a state-changing request other than a publish or a send MUST survive a cluster replacement, so that a retry against the next cluster is answered from the record.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/core/durability-and-deployment.md#audit-and-usage-leave-through-their-own-sink | A drain MUST deliver every pending audit event and usage record to that sink before the final records are written.
<!-- Allocation: DR_S6#A_RESTORE_RELEASE_DRILL | runtime -->
Source: spec/core/durability-and-deployment.md#restores-are-drilled | A restore MUST be drilled in a pre-production stage every release, since every deploy and every crash depends on it.
<!-- Allocation: DR_S6#A_RESTORE_RELEASE_DRILL | runtime -->
Source: spec/core/durability-and-deployment.md#restores-are-drilled | A restore MUST be drilled into a different failure domain from the one the lost cluster occupied.
<!-- Allocation: DR_S6#A_CLUSTER_EPOCH | runtime -->
Source: spec/core/durability-and-deployment.md#the-epoch-fence | A cluster whose epoch is superseded MUST fail its next write.
<!-- Allocation: DR_S6#A_CLUSTER_EPOCH | runtime -->
Source: spec/core/durability-and-deployment.md#the-epoch-fence | A cluster whose epoch is superseded MUST stop its partitions.
<!-- Allocation: DR_S6#A_CLUSTER_EPOCH | runtime -->
Source: spec/core/durability-and-deployment.md#the-epoch-fence | A new cluster MUST increment an epoch that every write is conditioned on.
<!-- Allocation: DR_S6#A_CLUSTER_EPOCH | runtime -->
Source: spec/core/durability-and-deployment.md#the-epoch-fence | The epoch increment MUST be conditional, so that two new clusters cannot both become the owning cluster.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type | A cluster that holds sessions MUST NOT be deleted before its drain has completed.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type | A deploy that replaces a cluster MUST be a drain, a checkpoint, a new cluster, and a restore, in that order.
<!-- Allocation: DR_S6#A_ROLLOUT_ORDER | runtime -->
Source: spec/core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type | A deploy that rolls a node's image in place MUST transfer that node's partitions under the fence, so that no session is paused.
<!-- Allocation: DR_S6#A_RESTORE_WHOLE_TENANT | runtime -->
Source: spec/decisions/durability/self-durable.md#a-new-cluster-restores-from-the-store-s-backup | A new cluster MUST restore every session from the durable store's own backup.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/decisions/durability/self-durable.md#an-orderly-drain-loses-nothing | An orderly drain MUST lose nothing.
<!-- Allocation: DR_S6#A_ORDERED_DRAIN | runtime -->
Source: spec/contracts/usage-record.md#the-stream-has-its-own-retention | A drain MUST flush pending usage records before the final session records are written.

---

ID: DV
Title: Durable store qualification
Prerequisites: DA, SIM
Deliverable: Store contract probes and crash, all-node-loss, backup and recovery evidence. Interface boundary: One expectation suite against simulated and real transactions, process restart, all-node/cluster loss and backup restoration, with recorded fault activation and outcome.
Acceptance: The same expectations run against simulator and real store; divergence and committed-state loss fail qualification. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
Dependency: SIM | inferred | Requires explicit time, transaction and injectable-fault interfaces for integration.
<!-- Allocation: DV_S6#A_STORE_NODE_LOSS | runtime -->
Source: spec/core/shared-framework.md#durable-store-durability | The durable store MUST survive a full-cluster restart.
<!-- Allocation: DV_S6#A_STORE_NODE_LOSS | runtime -->
Source: spec/decisions/durability/self-durable.md#a-full-cluster-restart-is-drilled | A full-cluster restart MUST be drilled in a pre-production stage every release.
<!-- Allocation: DV_S6#A_STORE_NODE_LOSS | runtime -->
Source: spec/decisions/durability/self-durable.md#the-store-survives-the-loss-of-every-node | The durable store MUST survive the loss of every node at once without losing committed state.

---

ID: E
Title: Governed ingress and inbox bounds
Prerequisites: B, C, ED, H
Deliverable: Publish/subscribe/send admission, attachments, pending caps and held-work exceptions. Interface boundary: Event wire contract, opaque address/actor/provenance, bounded inbox admission, permitted held-work escape paths and attachment admission before publish.
Acceptance: Actual admission enforces tenant/governance checks and bounds; trusted answers to open work survive cap pressure and resync records expose drops. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: B | inferred | Attachment admission consumes owned blob, staged-holder and pin-resolution interfaces before publication.
Dependency: C | inferred | Requires open-dispatch identity, result and close/recovery interfaces for integration.
Dependency: ED | inferred | Requires canonical event delivery and subscription interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
<!-- Allocation: E_S1#A_EVENT_CONTRACT_EVOLUTION | governance -->
Source: spec/contracts/event-envelope.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed publishers and subscribers, or else carry an explicit version increment.
<!-- Allocation: E_S1#A_EVENT_CONTRACT_EVOLUTION | governance -->
Source: spec/contracts/event-envelope.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed publishers and subscribers MUST carry a stated migration path.
<!-- Allocation: E_S3#A_EVENT_WIRE_SERIALIZATION | runtime -->
Source: spec/contracts/event-envelope.md#additive-evolution-of-this-contract | An integer field that may exceed the declared exact-integer bound MUST be carried as a decimal string.
<!-- Allocation: E_S1#A_EVENT_WIRE_ENCODING | design -->
Source: spec/contracts/event-envelope.md#additive-evolution-of-this-contract | Each field's encoding MUST be declared by this contract rather than chosen per value.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#an-event-carries-its-destination | A published event MUST be addressed to a topic.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#an-event-carries-its-destination | A sent event MUST be addressed to a session, or to a keyed template together with its key.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#an-event-carries-its-destination | A topic MUST be scoped within the publisher's tenant.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#an-event-may-carry-a-kind | An event MAY carry a kind, an opaque label the core compares only for equality.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#an-inbound-event-never-dispatches-a-tool | An inbound event MUST NOT carry a tool call for the core to dispatch.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | A publish or a send MUST refer to a staged upload only when the publisher's principal and chain both equal the provenance recorded on the upload.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | A publish whose attachments cannot all be resolved MUST append nothing.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | An attachment whose bytes are unavailable when its event applies MUST render as unavailable with a reason.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | An event MAY carry attachments, each a content type and a blob reference.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | An event whose attachment is unavailable MUST still apply.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/contracts/event-envelope.md#attachments-are-carried-beside-the-payload | The platform MUST render an attachment into a turn as a content block without reading the payload.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#every-event-carries-its-scope | Every event MUST carry the access scope its source allowed.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-priority | Every event MUST carry a priority.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-priority | The core MUST NOT interpret a priority beyond ordering.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-payload-is-opaque-and-bounded | A payload MUST NOT exceed the configured payload bound.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-payload-is-opaque-and-bounded | Content larger than the payload bound MUST be carried by reference.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-payload-is-opaque-and-bounded | The core MUST NOT interpret a payload.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-publisher-is-recorded-never-read | Every delivered event MUST identify its causal parent event.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-publisher-is-recorded-never-read | The platform MUST NOT take a publisher's identity from the payload.
<!-- Allocation: E_S3#A_EVENT_ADDRESS_OPACITY | runtime -->
Source: spec/contracts/event-envelope.md#the-publisher-is-recorded-never-read | The platform MUST record on every delivered event its publisher's authenticated principal and chain.
<!-- Allocation: E_S5#A_EVENT_ATTACHMENT_ADMISSION | runtime -->
Source: spec/core/encryption-and-blobs.md#an-attachment-is-copied-under-its-receiver | An attachment a session receives MUST be copied under the receiving session's own owner before the event applies.
<!-- Allocation: E_S5#A_INGRESS_BUFFER_OPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#an-external-queue-is-at-most-an-ingress-buffer | An external queue or topic MAY buffer publishes outside the platform for publishers that require independence from the cluster's availability.
<!-- Allocation: E_S5#A_INGRESS_BUFFER_OPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#an-external-queue-is-at-most-an-ingress-buffer | An ingress buffer MUST be drained by the platform's ingress into its own topics.
<!-- Allocation: E_S3#A_INBOX_BOUNDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | A session whose inbox kept a dropped-count marker MUST re-sync from the topic when it drains.
<!-- Allocation: E_S3#A_INBOX_BOUNDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | An inbox MUST NOT drop an event silently.
<!-- Allocation: E_S3#A_INBOX_BOUNDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | An inbox past its cap MUST keep, per topic, a marker that records the count and the first and last dropped events in place of further events.
<!-- Allocation: E_S5#A_INBOX_HELD_WORK_INTEGRATION | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | An inbox past its cap MUST still admit an answer to held work: a defer's resolution or timeout, a deferred call's outcome, and a tool result that matches an open dispatch.
<!-- Allocation: E_S3#A_INBOX_BOUNDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | Every inbox entry's payload, with its attachment references counted together, MUST be within the configured payload bound, whatever producer wrote it.
<!-- Allocation: E_S3#A_INBOX_BOUNDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-pending-inbox-is-bounded | The pending inbox of a session that is not draining MUST be bounded by the configured cap.

---

ID: ED
Title: Topics, subscriptions and ordered delivery
Prerequisites: S
Deliverable: Publish receipts, bidirectional subscription indexes and canonical inbox fan-out. Interface boundary: Publisher/destination identities, topic positions, bidirectional subscription state, durable fan-out progress, retention and lineage reset.
Acceptance: Durable fan-out preserves identities/order with atomic progress, bounded transactions, retention and shard behavior. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
<!-- Allocation: ED_S3#C_EVENT_SCOPE | runtime -->
Source: constitution.md#scopes-are-recorded-from-the-first-event | Every event MUST carry the access scope its source allowed, from the first event the platform accepts.
<!-- Allocation: ED_S3#A_TOPIC_POSITION | runtime -->
Source: spec/contracts/event-envelope.md#a-topic-delivery-carries-its-position | A subscriber MUST NOT compare topic positions across lineage epochs.
<!-- Allocation: ED_S3#A_TOPIC_POSITION | runtime -->
Source: spec/contracts/event-envelope.md#a-topic-delivery-carries-its-position | Every delivery of a topic's event to a subscriber MUST carry the event's position on the topic together with the lineage epoch the position belongs to.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A group's deciders MUST be asked when a subscribe or an unsubscribe is made, so that a later change to the group leaves existing subscriptions as they stand.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A late event addressed to a topic that is idle MUST re-create the topic rather than be refused.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A publish, a subscribe, or an unsubscribe addressed to a topic of a group the tenant does not hold MUST be refused.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A subscribe MUST pass its group's subscribe deciders before the subscriber is recorded.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A subscribe that a group's deciders deny MUST be refused with the decider's reason.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A subscribe that a group's deciders deny MUST record no subscriber.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A tenant MUST be able to list its topic groups and the topics of each.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A topic MUST be addressed by its topic group and its topic name.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A topic MUST carry no policy record of its own.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A topic group MAY carry a retention bound and a default event-kind filter that its topics inherit.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A topic group MUST carry the ordered deciders asked before a subscriber is added to any of its topics.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | A topic group MUST carry the ordered deciders asked before a subscriber is removed from any of its topics.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | Creating, replacing, or removing a topic group MUST be a control operation that passes the control-operation hook.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | Every topic MUST belong to exactly one topic group held by its tenant.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | Removing a topic group MUST reclaim its topics together with their subscriptions.
<!-- Allocation: ED_S5#A_TOPIC_GROUPS | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-belongs-to-a-topic-group | Who may publish to a topic MUST be decided by a publish gate of the topic's group, separate from its subscribe deciders.
<!-- Allocation: ED_S5#A_TOPIC_RETENTION | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-s-queue-is-bounded | A re-sync from a position older than the topic's retention MUST report the range as unrecoverable rather than skip it silently.
<!-- Allocation: ED_S5#A_TOPIC_RETENTION | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-s-queue-is-bounded | A restore that cannot preserve a topic's positions MUST deliver the unrecoverable range to each subscriber as a dropped range rather than replay it.
<!-- Allocation: ED_S5#A_TOPIC_RETENTION | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-s-queue-is-bounded | A topic's queue MUST be bounded by its reader with a drop marker per subscriber rather than by blocking a publisher.
<!-- Allocation: ED_S5#A_TOPIC_RETENTION | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-s-queue-is-bounded | A topic's queue MUST be trimmed of the events every subscriber has passed once their retention has elapsed.
<!-- Allocation: ED_S5#A_TOPIC_RETENTION | runtime -->
Source: spec/core/events-and-subscriptions.md#a-topic-s-queue-is-bounded | A topic's queue MUST keep an event as long as its retention has not elapsed.
<!-- Allocation: ED_S5#A_DIGEST_HOLD_OPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#digests-are-a-subscription-option | A session that holds an entry for its next wake MUST set a deadline at the entry's maximum hold.
<!-- Allocation: ED_S5#A_DIGEST_HOLD_OPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#digests-are-a-subscription-option | A subscription MAY ask for its events as a digest rather than one at a time.
<!-- Allocation: ED_S5#A_DIGEST_HOLD_OPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#digests-are-a-subscription-option | A subscription MAY declare a delivery mode and a maximum hold.
<!-- Allocation: ED_S3#A_TOPIC_POSITION | runtime -->
Source: spec/core/events-and-subscriptions.md#each-subscriber-receives-in-order | Each notification MUST carry an id the subscriber can dedupe on.
<!-- Allocation: ED_S3#A_TOPIC_POSITION | runtime -->
Source: spec/core/events-and-subscriptions.md#each-subscriber-receives-in-order | Each subscriber MUST receive a topic's events in the order they were published.
<!-- Allocation: ED_S3#A_TOPIC_POSITION | runtime -->
Source: spec/core/events-and-subscriptions.md#each-subscriber-receives-in-order | Where a topic's source assigns no position, the topic's single writer MUST assign a monotonic position at append.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#indexes-are-kept-in-both-directions | A subscription MUST be held by its subscriber as a durable record independent of whether the topic is resident.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#indexes-are-kept-in-both-directions | The platform MUST keep an index from each session to its subscriptions.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#indexes-are-kept-in-both-directions | The platform MUST keep an index from each topic to its subscribers.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A publish MUST complete once the event is appended to its topic, before fan-out to subscribers.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A publish MUST write a bounded number of keys per topic shard.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A slow or frozen subscriber MUST delay no other subscriber's deliveries.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A topic MUST accept subscribers beyond one fan-out batch's bound by adding shards rather than by refusing the subscribe.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A topic whose subscriber index exceeds the configured count MUST split into topic shards, each with its own queue and index.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | A topic's queue and subscriber index MUST be owned by the partition that owns the topic's key, under that partition's fence.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | Fan-out MUST deliver to subscribers' inboxes and doorbells in batches within the store's configured transaction key bound.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | Fan-out of a topic MUST NOT delay the owning partition's application of its sessions' inboxes.
<!-- Allocation: ED_S5#A_TOPIC_FANOUT | runtime -->
Source: spec/core/events-and-subscriptions.md#publish-latency-is-independent-of-subscribers | The latency of a publish MUST be independent of the number of subscribers to its topic.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#subscriptions-come-from-definitions-sessions-and-grants | A principal other than the session MUST manage the session's subscriptions only through a control operation that passes the hook under a grant.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#subscriptions-come-from-definitions-sessions-and-grants | A session MUST be able to subscribe and unsubscribe itself through native tools.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#subscriptions-come-from-definitions-sessions-and-grants | A standing subscription MUST come from the session's definition.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#three-primitives-opaque-to-the-core | A subscriber MUST be a session or a viewer.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#three-primitives-opaque-to-the-core | The platform MUST provide publish, subscribe, and unsubscribe as primitives.
<!-- Allocation: ED_S6#A_TOPIC_INDEX_SUBSCRIPTION | runtime -->
Source: spec/core/events-and-subscriptions.md#three-primitives-opaque-to-the-core | The platform MUST provide send to one session or to a keyed template with its key.

---

ID: FEED
Title: Change feeds and live observation
Prerequisites: AUTH, D, DA, H
Deliverable: Ordered resumable feeds and a bounded live-stream hub with current reader authorization. Interface boundary: Atomic resumable change cursor, current reader authorization, bounded live/tail delivery, lifecycle outbox ordering/coalescing and signed receiver dedupe.
Acceptance: Durable positions, gone/reset responses, slow viewers and access changes preserve order and confidentiality across reconnects. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | inferred | Requires verified caller, assertion and authenticated transport interfaces for integration.
Dependency: D | inferred | Requires immutable loaded-definition and grant-check interfaces for integration.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#a-backlog-coalesces | A backlog of status events for one session MUST coalesce to the latest state rather than replay every intermediate state.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#a-hook-never-affects-its-session | A failing or slow destination MUST NOT affect the session whose events it receives.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#a-receiver-dedupes-and-orders | A receiver MUST NOT let an older sequence overwrite a newer state.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#a-receiver-dedupes-and-orders | A receiver MUST ignore a delivery whose id it has already applied.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#a-receiver-dedupes-and-orders | A receiver MUST verify the signature before acting on a delivery.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | A delivery that fails MUST be retried with backoff.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | A hook delivery MUST be made from the session's durable outbox rather than from memory.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | Deliveries for one session MUST arrive in order of their sequence.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | Every delivery MUST be signed over its content and a timestamp.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | Every delivery MUST carry a sequence number within its session.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | Every delivery MUST carry an id by which a receiver recognizes a duplicate.
<!-- Allocation: FEED_S6#B_EVENT_HOOK_RECOVERY | runtime -->
Source: spec/contracts/event-hook.md#delivery-comes-from-the-durable-outbox | Every event MUST be delivered at least once.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#destinations-are-registered | A definition MUST NOT specify a destination by an arbitrary address.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#destinations-are-registered | A delivery MUST leave through the egress point.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#destinations-are-registered | A destination MUST be registered per tenant and referenced by name.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#payloads-carry-state-never-content | A payload MUST NOT carry transcript content.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#payloads-carry-state-never-content | A payload MUST carry ids, states, reasons, and usage.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | A blocked event MUST carry its reason.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | A paused event MUST carry its reason and the ids and dropped-count ranges of the pending events the session never applied.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | A resumed event MUST carry the session's new lease generation.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | A waiting-on-a-defer event MUST carry the defer's decision id.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | The set of event kinds MUST include started, adopted a version, woken, idle, blocked, waiting on a defer, paused, resumed, stopped, failed, quarantined, and out of budget.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/contracts/event-hook.md#the-set-is-closed-and-versioned | The set of event kinds a hook delivers MUST be a closed set defined by this contract's version.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#a-viewer-is-one-page-s-connection | A reconnecting page MUST send its subscription set again.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#a-viewer-is-one-page-s-connection | A viewer MUST be one live connection from one person's page, opened with a short-lived viewer token for one person in one tenant.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#a-viewer-is-one-page-s-connection | A viewer MUST subscribe and unsubscribe on its connection as its page changes.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#a-viewer-is-one-page-s-connection | A viewer's subscriptions MUST NOT outlive its connection.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#no-change-is-missed-and-no-history-is-replayed | A page that needs current state MUST load it from the adapter rather than from the viewer stream.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#no-change-is-missed-and-no-history-is-replayed | A viewer MUST NOT receive history.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#no-change-is-missed-and-no-history-is-replayed | A viewer that subscribes to a topic MUST NOT miss a change made between the page loading its view and the subscription taking effect.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#only-what-its-person-may-read | A viewer subscription MUST end when its person's access is revoked.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#only-what-its-person-may-read | A viewer subscription MUST pass a read check against the topic's access scope when it is made.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#only-what-its-person-may-read | A viewer's access MUST be decided again at every open and every resume.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_LIFETIME | runtime -->
Source: spec/core/events-and-subscriptions.md#only-what-its-person-may-read | A viewer's stream MUST end at its credential's expiry.
<!-- Allocation: FEED_S6#ACTUAL_PERSON_ADAPTER_ROUTING | runtime -->
Source: spec/core/events-and-subscriptions.md#person-addressed-routing-is-the-adapter-s | What is addressed to a person MUST be published by the adapter to that person's own topic, which the person's pages subscribe to.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#small-and-lossy-by-design | A slow viewer, or a burst on a busy topic, MUST collapse to one reload notice per topic.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#small-and-lossy-by-design | A viewer MUST NOT slow a publisher, a session, or another viewer.
<!-- Allocation: FEED_S3#A_PAGE_VIEWER_RACES | runtime -->
Source: spec/core/events-and-subscriptions.md#small-and-lossy-by-design | A viewer MUST receive change notices bounded by the publisher's payload bound rather than records.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A change a handler permanently rejects MUST go to a dead-letter record with an alarm.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A change feed's cursor MUST be resumable and fenced by a lease generation.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A change feed's positions MUST be qualified by the restore epoch.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A cursor MUST commit in the consumer's own transaction, checked against the consumer's generation inside it.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A handler MUST produce the same effects in the same order on every redo of a change.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A position the source cannot serve MUST be answered with a distinct gone status rather than silently resumed.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A source MUST NOT answer with a head below a position it has served.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | A source MUST serve changes in a prefix-closed order.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | An external effect a change-feed consumer makes MUST carry an id derived from its position.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | The cursor MUST move past a change that went to the dead-letter record.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | The framework MUST provide a change feed as an ordered log.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | The framework MUST provide a resumable form of the change feed over the network.
<!-- Allocation: FEED_S3#A_CHANGE_FEED | runtime -->
Source: spec/core/shared-framework.md#the-change-feed | The network form of a feed MUST take the tenant from the authenticated caller and never from the path.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | A node MUST bound the buffers of all its streams together.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | A viewer's access MUST be decided afresh at every open and resume.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | A viewer's access MUST be decided again when a change marked as an access change arrives.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | An ungated frame MUST NOT be replayed or stored.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | The framework MUST provide a live-stream hub fed by a change feed.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | The hub MUST apply per-viewer filters and access checks.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | The hub MUST hold a bounded buffer that drops a slow viewer.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | The hub MUST let a viewer resume from a last-seen id.
<!-- Allocation: FEED_S3#A_LIVE_HUB | runtime -->
Source: spec/core/shared-framework.md#the-live-stream-hub | The hub MUST send a reset to a viewer whose last-seen id is older than the bounded retention.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | A resume from a position the tail source still serves MUST continue with no event lost or repeated, under any valid token for the same viewer and session.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | A tail position MUST identify the lineage epoch and the session's own entry counter, independent of the connection, the node, and the token that delivered it.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | A tail position MUST survive a compaction, a freeze, an owner move, and a planned drain and restore.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | A tail position inside a frozen transcript range MUST stay resolvable from that range.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | A viewer MUST be reset only for a position that is off the session's history, inside a range retention has removed, or above the head.
<!-- Allocation: FEED_S6#B_TAIL_RECOVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-position-survives-moves-and-compactions | The reset MUST carry the current head rather than an error.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-relay-runs-under-the-platform-s-identity | A relay of a tail between nodes MUST run under the platform's own identity with a scope limited to reading that session's tail.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-relay-runs-under-the-platform-s-identity | Each relayed tail read MUST be audited with the relaying node, the tenant, and the session.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A model's reasoning content MUST NOT be shown to a viewer, passed to a tool, or carried on an event, except on a tail under the privileged ungated option, marked as ungated.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A tail MUST be relayable from any node through a bounded buffer, so that a slow viewer is dropped and never slows the session.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A tail MUST show output only once its deciders have passed.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A tail read MUST NOT change the session's state.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A tail read of a frozen session MUST NOT load the session.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | A viewer of a tail MUST hold a read grant on the session.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | Live ungated output on a tail MUST be a privileged option marked as such.
<!-- Allocation: FEED_S5#B_TAIL_DELIVERY | runtime -->
Source: spec/core/the-frontend.md#a-tail-streams-gated-activity | The API MUST stream one session's activity as events that resume by id and can start from any point in the transcript.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST NOT be stored.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST be bound to one viewer and one session rather than to a connection.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST expire no later than the configured assertion lifetime after it is issued.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST have a type distinct from a call assertion, so that neither verifies as the other.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST open at most one stream.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | A tail token MUST travel only in a request header, never in an address or a log.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | Access to a tail MUST be decided at the control-operation hook when its token is issued.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | The platform MUST end an open tail at its token's expiry with an end event that states the reason.
<!-- Allocation: FEED_S5#B_TAIL_TOKEN | runtime -->
Source: spec/core/the-frontend.md#a-tail-token-opens-one-stream | The tail route MUST accept no credential other than a tail token.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/core/the-frontend.md#event-hooks-deliver-a-session-s-status | An event hook MUST NOT affect the session whose events it carries.
<!-- Allocation: FEED_S5#B_EVENT_HOOK_SHAPE | runtime -->
Source: spec/core/the-frontend.md#event-hooks-deliver-a-session-s-status | The API MUST deliver a session's lifecycle and status events to the destinations its definition specifies.

---

ID: G
Title: Conformance and execution evidence
Prerequisites: DR, DV, SIM
Deliverable: Exact source traceability, executable requirement checks, dependency parity and reproducible promotion evidence. Interface boundary: Original applicability/strength classification, exact native traceability, per-slice behavior/fault/probe evidence, trusted checker separation and promotion gates.
Acceptance: Absent, stale, unexecuted or violated obligations fail the applicable gate; all dependency probes and release drills have attributable evidence. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: DR | inferred | Requires whole-platform drain/restore and release-drill evidence for integration.
Dependency: DV | inferred | Requires actual store parity and failure-survival evidence for integration.
Dependency: SIM | inferred | Requires explicit time, transaction and injectable-fault interfaces for integration.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: constitution.md#amendment-discipline | An amendment that weakens a protected guarantee MUST require explicit human approval.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: constitution.md#amendment-discipline | An amendment to this constitution MUST be recorded with its rationale.
<!-- Allocation: G_S1#C_LANGUAGE_STRENGTH | language-context -->
Source: constitution.md#constitution | > > The key words **MUST** and **MUST NOT** state an absolute requirement; **SHOULD** and **SHOULD NOT** state a requirement that an implementation may set aside only with a recorded reason; **MAY** states an optional behavior.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: constitution.md#contracts-change-only-by-coordinated-act | A change to a contract that is not additive with respect to deployed clients MUST carry a stated migration path.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: constitution.md#contracts-change-only-by-coordinated-act | A change to a contract that is not additive with respect to deployed clients MUST carry a version increment.
<!-- Allocation: G_S1#C_CONCRETE_SHARED_DESIGN | design -->
Source: constitution.md#i-minimal-and-concrete | A generalization of a mechanism MUST be extracted from at least two working cases that use it, rather than designed ahead of them.
<!-- Allocation: G_S1#C_CONCRETE_SHARED_DESIGN | design -->
Source: constitution.md#i-minimal-and-concrete | A mechanism of the platform MUST be a concrete primitive with a defined interface that a program can be written against, rather than a universal abstraction other mechanisms are encoded into.
<!-- Allocation: G_S1#C_IRREVERSIBLE_DECISIONS | design -->
Source: constitution.md#irreversible-choices-are-decided-first | A property that can be added later without rewriting existing data MAY be deferred until use demands it.
<!-- Allocation: G_S1#C_IRREVERSIBLE_DECISIONS | design -->
Source: constitution.md#irreversible-choices-are-decided-first | A property that cannot be changed once data has been written under it MUST be decided before the first durable record is written.
<!-- Allocation: G_S6#C_TENANT_NO_CROSS | runtime -->
Source: constitution.md#the-tenant-is-the-boundary | Content derived from one tenant's data MUST NOT reach another tenant.
<!-- Allocation: G_S6#C_TENANT_NO_CROSS | runtime -->
Source: constitution.md#the-tenant-is-the-boundary | Every durable record, principal, grant, subscription, and event MUST carry its tenant.
<!-- Allocation: G_S1#C_CONCRETE_SHARED_DESIGN | design -->
Source: constitution.md#vi-generic-mechanisms-belong-in-the-shared-framework | A mechanism that more than one product needs MUST be built as a component of the shared framework rather than kept inside one product.
<!-- Allocation: G_S1#C_CONCRETE_SHARED_DESIGN | design -->
Source: constitution.md#vi-generic-mechanisms-belong-in-the-shared-framework | A product MUST NOT keep a second copy of a mechanism the shared framework provides.
<!-- Allocation: G_S5#C_GOVERNANCE_MECHANISM | runtime -->
Source: constitution.md#vii-what-repeats-becomes-a-mechanism | A decision MUST reach a person only when no mechanism can decide it.
<!-- Allocation: G_S5#C_GOVERNANCE_MECHANISM | runtime -->
Source: constitution.md#vii-what-repeats-becomes-a-mechanism | A decision that reached a person MUST be recorded as a candidate for conversion into a mechanism.
<!-- Allocation: G_S5#C_GOVERNANCE_MECHANISM | runtime -->
Source: constitution.md#vii-what-repeats-becomes-a-mechanism | Whether a change is correct MUST be decided by a mechanism rather than by an approval.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/decision-hook.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed deciders, or else carry an explicit version increment.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/decision-hook.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed deciders MUST carry a stated migration path.
<!-- Allocation: G_S1#B_DEFINITION_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/definition-source.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed sources and clients, or else carry an explicit version increment.
<!-- Allocation: G_S1#B_DEFINITION_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/definition-source.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed sources and clients MUST carry a stated migration path.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/event-hook.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed receivers, or else carry an explicit version increment.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/event-hook.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed receivers MUST carry a stated migration path.
<!-- Allocation: G_S7#C_TRUSTED_EVIDENCE_ORIGIN | runtime -->
Source: spec/contracts/evidence-record.md#a-checker-that-includes-a-model-cannot-admit | An evidence record from a checker that includes a model MUST be used only to block or to flag, never to admit.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/evidence-record.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed gates, or else carry an explicit version increment.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/evidence-record.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed gates MUST carry a stated migration path.
<!-- Allocation: G_S7#C_TRUSTED_EVIDENCE_ORIGIN | runtime -->
Source: spec/contracts/evidence-record.md#evidence-is-the-checker-s-own-record | An evidence record MUST be produced by the checker that ran, rather than by the agent whose work it evaluates.
<!-- Allocation: G_S7#C_TRUSTED_EVIDENCE_ORIGIN | runtime -->
Source: spec/contracts/evidence-record.md#evidence-is-the-checker-s-own-record | An evidence record MUST be signed by the principal of the runner that executed the checker.
<!-- Allocation: G_S7#C_TRUSTED_EVIDENCE_ORIGIN | runtime -->
Source: spec/contracts/evidence-record.md#evidence-is-the-checker-s-own-record | An evidence record produced in an agent's workspace MUST NOT count as evidence for a gate.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-carries-a-result-of-a-closed-set | A fail MUST list the failures it found.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-carries-a-result-of-a-closed-set | An evidence record's result MUST be exactly one of pass, fail, or unknown.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-carries-a-result-of-a-closed-set | An unknown result MUST NOT allow a change.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-identifies-its-subject | An evidence record MUST identify the subject it evaluated by the digest of the artifact and of its change.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-identifies-the-checker | An evidence record MUST identify the checker and carry its version and the digest of the configuration it ran with.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-identifies-the-property | An evidence record MUST identify the property it checked by the digest of the pinned specification version and the version of the gate spec.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-is-an-event | An evidence record MUST be published as an event the gate's hook can read.
<!-- Allocation: G_S7#C_EVIDENCE_REPLAY | runtime -->
Source: spec/contracts/evidence-record.md#the-record-is-reproducible | An evidence record MUST carry what is needed to reproduce it: the commits and the toolchain pins.
<!-- Allocation: G_S7#C_EVIDENCE_REPLAY | runtime -->
Source: spec/contracts/evidence-record.md#the-record-is-reproducible | Re-running an evidence record's reproduction MUST yield the same result and the same failure ids.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-states-its-scope-and-strength | An evidence record MUST list the requirements and the files its check covered.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/contracts/evidence-record.md#the-record-states-its-scope-and-strength | An evidence record MUST state, per requirement it covers, the strength of its evidence as one of presence, executed, mutation-checked, or proved.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/signed-assertion.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed verifiers, or else carry an explicit version increment.
<!-- Allocation: G_S1#B_CONTRACT_CHANGE_GOVERNANCE | governance -->
Source: spec/contracts/signed-assertion.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed verifiers MUST carry a stated migration path.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/tool-call.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed tool servers, or else carry an explicit version increment.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/tool-call.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed tool servers MUST carry a stated migration path.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/usage-record.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to deployed readers, or else carry an explicit version increment.
<!-- Allocation: G_S1#C_CHANGE_DISCIPLINE | governance -->
Source: spec/contracts/usage-record.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to deployed readers MUST carry a stated migration path.
<!-- Allocation: G_S1#A_FRAMEWORK_RELEASE_REVIEW | governance -->
Source: spec/core/shared-framework.md#components-change-through-their-own-review | A change to a framework component MUST go through the framework's own review.
<!-- Allocation: G_S1#A_FRAMEWORK_RELEASE_REVIEW | governance -->
Source: spec/core/shared-framework.md#components-change-through-their-own-review | A component MUST be consumed by a product only through the framework's released version.
<!-- Allocation: G_S1#C_GOVERNING_CLASSIFICATION | governance -->
Source: spec/core/simulation-and-conformance.md#a-governing-requirement-binds-to-the-change-process | A governing requirement excluded from the gating set MUST be recorded at the declared-default location, so that two builds are evaluated against an identical gating set.
<!-- Allocation: G_S1#C_GOVERNING_CLASSIFICATION | governance -->
Source: spec/core/simulation-and-conformance.md#a-governing-requirement-binds-to-the-change-process | A requirement that governs how the specification or its configuration may change MUST bind to the change-process check rather than to a runtime citation.
<!-- Allocation: G_S1#C_GOVERNING_CLASSIFICATION | governance -->
Source: spec/core/simulation-and-conformance.md#a-governing-requirement-binds-to-the-change-process | A requirement that mandates a design or governance discipline rather than a runtime behavior MUST NOT be counted as gating for a build's requirement gate.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#behavior-is-witnessed-by-execution | A cited test's execution MUST be witnessed, so that a citation on code that never runs does not count.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#behavior-is-witnessed-by-execution | A requirement that describes runtime behavior MUST be discharged by a test that executes that behavior and observes its result.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#behavior-is-witnessed-by-execution | A requirement that pins the shape of an artifact MUST NOT be counted as covered by a citation that produces the shape without a path that exercises it.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test | A check for a security property MUST test the absence of the capability rather than the presence of a control by name.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test | A cited test MUST exercise the behavior its requirement describes rather than merely reference the requirement's text.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test | A cited test MUST fail when the specific behavior its requirement describes is removed or violated.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test | A requirement MUST be counted as covered only when it has both an implementation citation and a test citation.
<!-- Allocation: G_S7#C_DISCRIMINATING_EXECUTION | runtime -->
Source: spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test | Two requirements that describe distinct behaviors MUST NOT be discharged by one shared check that cannot fail for one without failing for the other.
<!-- Allocation: G_S1#C_GOVERNING_CLASSIFICATION | governance -->
Source: spec/core/simulation-and-conformance.md#every-requirement-binds-to-an-enforcing-line | A statement that no mechanism can bind to an enforcing line MUST be written as descriptive prose rather than as a requirement.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#every-requirement-binds-to-an-enforcing-line | Every requirement in this specification MUST bind to at least one citation that detects its violation.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#identity-is-the-quoted-sentence | A citation MUST record the requirement it satisfies by quoting the requirement's sentence, so that changing the wording invalidates every citation that no longer matches.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#identity-is-the-quoted-sentence | A citation whose quoted text matches no requirement MUST be a failure that reports its location.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#identity-is-the-quoted-sentence | A requirement's identity MUST be the tuple of its specification file, its section, and its exact quoted sentence.
<!-- Allocation: G_S6#C_GENERATED_FAILURE_EXIT_SUITE | runtime -->
Source: spec/core/simulation-and-conformance.md#properties-are-generated-and-checked-against-a-model | A property test MUST run generated sequences of events, tool outcomes, crashes, moves, and definition changes through the platform and through a reference model, and check the invariants on every run.
<!-- Allocation: G_S7#C_PROMOTION_COMPLETENESS | runtime -->
Source: spec/core/simulation-and-conformance.md#security-controls-are-shown-to-hold | A control that rests on the absence of a permission MUST be shown to hold against every granted permission that carries the same capability.
<!-- Allocation: G_S7#C_PROMOTION_COMPLETENESS | runtime -->
Source: spec/core/simulation-and-conformance.md#security-controls-are-shown-to-hold | A security control MUST be in place in the release that introduces the risk it covers, unless the operator records an exception for that release.
<!-- Allocation: G_S6#C_GENERATED_FAILURE_EXIT_SUITE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-failure-table-is-covered | Each blocked and quarantined state MUST have a scenario showing an exit transition an adapter can trigger.
<!-- Allocation: G_S6#C_GENERATED_FAILURE_EXIT_SUITE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-failure-table-is-covered | Each failure class MUST have a scenario that asserts its handling and the state the session ends in.
<!-- Allocation: G_S7#C_PROMOTION_COMPLETENESS | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gate-decides-promotion | A build in which any absolute requirement lacks both an implementation and a test citation MUST NOT be promoted.
<!-- Allocation: G_S7#C_PROMOTION_COMPLETENESS | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gate-decides-promotion | A requirement that is not absolute MAY be left uncovered.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gate-evaluates-against-an-immutable-snapshot | A gate run MUST evaluate against a content-addressed snapshot of the specification.
<!-- Allocation: G_S7#C_EXACT_NATIVE_TRACEABILITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gate-evaluates-against-an-immutable-snapshot | A gate run MUST record the configuration it evaluated against, so that two runs evaluate against an identical set.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that a call to the API on a node that does not own the session's partition is served.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that a configuration change to a keyed template costs the same number of durable writes for any instance count.
<!-- Allocation: G_S6#C_SCENARIO_COMMIT_FENCE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that a crash at any point leaves the session at its step in progress and the next owner finishes it.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that a partition moving while a caller waits on a session delivers the caller's reply once.
<!-- Allocation: G_S6#C_SCENARIO_SECURITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that a structured secret never reaches the model's context, the transcript, state, events, or logs.
<!-- Allocation: G_S6#C_SCENARIO_COMMIT_FENCE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that each inbox event applies exactly once relative to the session's state, through crashes, moves, duplicate publishes, and retried commits.
<!-- Allocation: G_S6#C_SCENARIO_COMMIT_FENCE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that events apply in order per session.
<!-- Allocation: G_S6#C_SCENARIO_RESOURCE_USAGE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that exactly one usage record exists per model attempt, tool call, and decider call.
<!-- Allocation: G_S6#C_SCENARIO_COMMIT_FENCE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that no commit or dispatch from a superseded owner takes effect.
<!-- Allocation: G_S6#C_SCENARIO_SECURITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that no output acts or leaves before its deciders pass.
<!-- Allocation: G_S6#C_SCENARIO_SECURITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that no tool runs unless the session's loaded definition lists it.
<!-- Allocation: G_S6#C_SCENARIO_RESOURCE_USAGE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that no transaction exceeds the key bound.
<!-- Allocation: G_S6#C_SCENARIO_RESOURCE_USAGE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that queues are trimmed.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that racing first events for one key create exactly one instance.
<!-- Allocation: G_S6#C_SCENARIO_RESOURCE_USAGE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that the transcript head stays within its bound.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check that two nodes declaring the same partitioned set at once yield one coordinator and one owner per partition.
<!-- Allocation: G_S6#C_SCENARIO_ROUTING_LIFECYCLE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check, with two tenants whose templates carry the same names, that no instance, cache entry, or status crosses between them.
<!-- Allocation: G_S6#C_SCENARIO_SECURITY | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario MUST check, with two tenants, that no record, cache entry, blob, or event crosses between them.
<!-- Allocation: G_S6#C_SCENARIO_COMMIT_FENCE | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | A scenario that checks the fence MUST include a superseded owner that regains access to the store after its partition moved.
<!-- Allocation: G_S2#C_FORMAT_COMPATIBILITY_CORPUS | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | Every durable format MUST have a recorded encoding that the current build decodes under test, so that a change that stops the build reading stored data fails before it deploys.
<!-- Allocation: G_S2#C_FORMAT_COMPATIBILITY_CORPUS | runtime -->
Source: spec/core/simulation-and-conformance.md#the-gating-invariants | Every evolvable stored type MUST have a test that a value carrying an unrecognized tag survives a read and a rewrite unchanged.
<!-- Allocation: G_S7#C_EVIDENCE_BOUND_FIELDS | runtime -->
Source: spec/core/governance.md#reference-checks-produce-audit-information | A charter reference check MUST NOT be treated as proof of behavioral compliance.
<!-- Allocation: G_S1#C_IMPL_TARGET_PIN | governance -->
Source: spec/core/implementation-organization.md#each-implementation-identifies-its-target | Each implementation MUST declare a target reference that resolves to the exact immutable shared-specification source it targets.
<!-- Allocation: G_S7#C_IMPL_EVIDENCE_ISOLATION | governance -->
Source: spec/core/implementation-organization.md#evidence-is-attributed-to-its-implementation | Implementation evidence MUST identify the implementation it concerns.
<!-- Allocation: G_S1#C_IMPL_EVIDENCE_ROLES | governance -->
Source: spec/core/implementation-organization.md#evidence-is-attributed-to-its-implementation | An implementation's planning associations MUST remain separate from its implementation citations, test citations, and conformance results.
<!-- Allocation: G_S7#C_IMPL_EVIDENCE_ISOLATION | governance -->
Source: spec/core/implementation-organization.md#evidence-is-attributed-to-its-implementation | An implementation's conformance claim MUST NOT be discharged by another implementation's citations or execution results.
<!-- Allocation: G_S1#C_IMPL_GENERATED_SEPARATION | governance -->
Source: spec/core/implementation-organization.md#generated-artifacts-are-separate | Generated artifacts MUST be kept separate from authored sources.
<!-- Allocation: G_S1#C_IMPL_SHARED_AUTHORITY | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | The specification repository MUST allow authored implementation specifications for multiple implementations of the shared specification.
<!-- Allocation: G_S1#C_IMPL_SHARED_AUTHORITY | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | Implementations described in the specification repository MUST share one authoritative specification.
<!-- Allocation: G_S1#C_IMPL_IDENTITY | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | Each implementation MUST have a stable implementation identifier.
<!-- Allocation: G_S1#C_IMPL_IDENTITY | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | Each implementation's authored implementation specification MUST occupy a distinct implementation directory in the specification repository.
<!-- Allocation: G_S1#C_IMPL_SOURCE_SEPARATION | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | An implementation's source code, tests, tooling, and generated artifacts MUST be kept in an implementation repository separate from the specification repository.
<!-- Allocation: G_S1#C_IMPL_SOURCE_SEPARATION | governance -->
Source: spec/core/implementation-organization.md#implementations-share-one-contract | An implementation's authored decisions, capability declarations, and phase declarations MUST be kept in its implementation specification.
<!-- Allocation: G_S7#C_IMPL_SHARED_TEST_ATTRIBUTION | governance -->
Source: spec/core/implementation-organization.md#shared-tests-keep-results-separate | Shared conformance test definitions MAY be kept outside implementation directories.
<!-- Allocation: G_S7#C_IMPL_SHARED_TEST_ATTRIBUTION | governance -->
Source: spec/core/implementation-organization.md#shared-tests-keep-results-separate | A result of a shared conformance test MUST be attributable to the implementation it exercised.

---

ID: H
Title: Governance decisions and atomic transitions
Prerequisites: D, S
Deliverable: Fixed hook pipeline, legal decisions, held resolutions and durable decision records. Interface boundary: Closed hook request/verdict algebra, actor and subject attribution, layered ordering, fail-closed defaults, atomic gated transition and selected checkers-only rules.
Acceptance: Real deciders fail closed; ordering and atomic gated effects hold; transformed or blocked content cannot escape. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: D | inferred | Requires immutable loaded-definition and grant-check interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-block-takes-precedence-over-a-defer | A block by any decider in a pipeline MUST take precedence over a defer returned by another decider.
<!-- Allocation: H_S3#B_HOOK_IDEMPOTENT_RETRY | runtime -->
Source: spec/contracts/decision-hook.md#a-decider-is-idempotent-by-decision-id | A decider asked the same decision id again MUST answer the same verdict or report the verdict already given.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-hook-is-declared-by-configuration | A decision hook MUST be declared by the session's definition rather than by the model.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-hook-is-declared-by-configuration | A decision hook MUST declare a deadline within the configured bounds.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-hook-is-declared-by-configuration | A decision hook MUST declare a failure outcome, which defaults to block.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-hook-is-declared-by-configuration | A decision hook MUST point at a destination registered with the tenant and referenced by name.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A block MUST carry a reason written for the model.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A block's reason MUST NOT repeat the content it rejects.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A decision MUST be exactly one of allow, flag, block, defer, or transform.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A defer MUST carry each deferring decider's reason as that decider gave it.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A defer MUST carry the principals allowed to resolve it, a deadline, and an outcome on timeout.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A defer MUST carry the subject as the whole pipeline left it, so that the resolver decides on what will run.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A redaction MUST identify the payload field it applies to.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A redaction marker MUST reveal nothing about the content it replaced.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A redaction span outside its field MUST be treated as the decider's failure.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#a-decision-is-one-of-a-closed-set | A transform MUST carry the patch to apply as redacted spans or set fields.
<!-- Allocation: H_S3#B_HOOK_IDEMPOTENT_RETRY | runtime -->
Source: spec/contracts/decision-hook.md#a-retry-reuses-the-decision-id | A decision request that fails in transport or with a server-side failure MAY be retried under the same decision id within the decider's deadline.
<!-- Allocation: H_S3#B_HOOK_IDEMPOTENT_RETRY | runtime -->
Source: spec/contracts/decision-hook.md#a-retry-reuses-the-decision-id | A decision request the destination refused, or answered with a body that is no verdict, MUST NOT be retried.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#each-decision-is-legal-only-at-stated-hooks | A defer MUST NOT be returned at the model-request, notification-arrival, or stop-request hooks.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#each-decision-is-legal-only-at-stated-hooks | A defer at the model-output hook MUST apply only to a tool-use block.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#each-decision-is-legal-only-at-stated-hooks | A transform MUST NOT be returned at the control-operation or stop-request hooks.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#each-decision-is-legal-only-at-stated-hooks | A transform that sets fields MUST set only fields in the closed set its hook offers.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request MUST NOT carry session context beyond that descriptor.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the compaction hook MUST carry the summary the compaction proposes and the cut point it covers.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the control-operation hook MUST carry the caller as its principal together with the operation's kind, target scope, and tier.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the model-request hook MUST carry the cause of the turn and the session's budget view.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the notification-arrival hook MUST carry a bounded activity descriptor: the session's phase, the work invested in the current step as tokens and time, the expected time to the next turn boundary, and the session's priority.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the notification-arrival hook MUST carry the event's kind, priority, recorded publisher and chain, access scope, and attachment references.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the tool-call hook MUST carry the digest of the spec the model saw and the call's side-effect and idempotency semantics.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request at the tool-result hook MUST state whether the result is an error and whether it is a call's result, a tool spec at load, or an outcome-unknown result.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#each-hook-adds-its-own-fields | A decision request for a subscribe or an unsubscribe MUST carry the principal, the tenant, the subscriber, the topic group, and the topic.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#latency-sensitive-hooks-admit-an-external-decider-only-as-a-defer | An external decider on the model-output, tool-call, or tool-result hook MUST run only as a defer.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#latency-sensitive-hooks-admit-an-external-decider-only-as-a-defer | An external decider on the stop-request hook MAY run synchronously.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#no-verdict-is-the-failure-outcome | A decider that gives no verdict within its deadline MUST be treated as having answered its declared failure outcome.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#no-verdict-is-the-failure-outcome | A decision returned at run time that is not legal at its hook MUST be treated as that decider's failure.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/contracts/decision-hook.md#no-verdict-is-the-failure-outcome | A response that is not a verdict of the closed set MUST be treated as having answered the failure outcome.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer MUST be resolved at most once.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer MUST be resolved only by an authenticated principal on its allowed list.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer configuration whose resolvers include no eligible principal MUST be refused at validation.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer past its deadline MUST take its timeout outcome.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer scoped to an approval call type MUST NOT list an agent or a model judge among its resolvers.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | A defer's timeout outcome MUST default to block.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | An approval defer that reaches its deadline with no eligible resolver MUST take block as its timeout outcome.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/contracts/decision-hook.md#only-a-listed-principal-resolves | The resolution of a defer MUST be exactly one of allow, flag, or block.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-carries-the-subject | A decision request MUST carry the session's usage.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-carries-the-subject | A decision request MUST carry the subject as the deciders earlier in the pipeline left it.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-carries-the-subject | A decision request MUST truncate content past the configured size bound with an explicit marker.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-carries-the-subject | A decision request for a configuration change MUST carry the configuration as it would be stored and the principal proposing it.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-carries-the-subject | A decision request on the model-output hook MAY include the tool results the output cites.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-itself | A decision request MUST carry a decision id that is the same when the same question is asked again.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-itself | A decision request MUST carry the contract version.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-itself | A decision request MUST carry the hook point and the call-type tags that apply.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-itself | A decision request MUST carry the name of the decider being asked.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-itself | A decision request MUST carry the tenant and the session, or the definition or tenant scope of an operation that has no session.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-the-actors | A decision request MUST carry the session's principal and on-behalf-of chain.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/contracts/decision-hook.md#the-request-identifies-the-actors | A decision request acting on another principal's event MUST also carry the requester's principal and chain.
<!-- Allocation: H_S1#B_HOOK_OPTIONAL_SIGNING | design -->
Source: spec/contracts/decision-hook.md#the-request-is-attributable | The platform MAY sign a decision request under its own identity so the destination can verify the question is the platform's.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/contracts/decision-hook.md#the-stop-hook-asks-whether-work-remains | A stop-admission answer MUST be allow or block.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/contracts/decision-hook.md#the-stop-hook-asks-whether-work-remains | A stop-admission block MUST carry a reason that states the open work that keeps the session running.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/contracts/decision-hook.md#the-stop-hook-asks-whether-work-remains | A stop-admission decider MUST allow a stop for a session whose definition's lifecycle intent, where one exists, is paused or retired.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/contracts/decision-hook.md#the-stop-hook-asks-whether-work-remains | A stop-admission request MUST carry the definition the session runs, whether the session is a root session or a child, and the count of consecutive blocked stops.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/contracts/decision-hook.md#the-stop-hook-asks-whether-work-remains | A stop-admission request MUST identify the session whose stop is proposed.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | A decider that fails at the notification-arrival hook MUST leave the event unapplied in the inbox and block the session with the reason.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | A failed decider dependency MUST be retried with backoff and an alert once per destination rather than once per session.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | A failure of a decider's dependency rather than of its decision MUST NOT count toward that attempt budget.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | A held event MUST be evaluated again on resume or on the session's next wake.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | A session blocked because a decider's dependency failed MUST retry with backoff when the dependency recovers, without an adapter's intervention.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-decider-or-its-dependency-fails | Each failed evaluation of a held event MUST count as one attempt toward the configured attempt budget for an event that keeps failing.
<!-- Allocation: H_S3#C_DECIDER_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#quarantine-and-blocks-are-events | A blocked state caused by a decider MUST identify the decider and state whether the decider answered a block or failed to answer.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | A blocked stop's reason MUST become the next turn's input.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | A session whose blocked stop would exceed the bound MUST block with the reason rather than take another turn.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | A session whose stop-admission decider failed MUST block with the reason and take no turn before its next inbox event or a resume.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | A stop request MUST be raised only when a turn ends with the provider's end-of-turn signal, makes no tool call, and leaves no unapplied inbox event.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | Consecutive blocked stops MUST be bounded, so that a session cannot be kept running indefinitely.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | The count of consecutive blocked stops MUST reset only on a cause the session did not originate.
<!-- Allocation: H_S6#B_HOOK_STOP | runtime -->
Source: spec/core/governance.md#a-blocked-stop-continues-the-session | The stop hook MUST decide whether a session may stop or has remaining work.
<!-- Allocation: H_S6#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A new or changed decider MUST run in shadow, recording without enforcing, before it may block.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A shadow decider MUST be evaluated even when an enforcing decider blocked first.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A shadow decider MUST see the request at the position it would hold in the pipeline, with the payload as the enforcing deciders before it left it.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A shadow decision MUST record whether it would have changed the outcome.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A shadow decision's record of whether it would have changed the outcome MUST be computed by evaluating the hook's outcome again with the shadow verdict in place.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A shadow evaluation MUST NOT delay or change the hook's outcome.
<!-- Allocation: H_S5#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | A substrate check MUST NOT run in shadow.
<!-- Allocation: H_S6#B_HOOK_SHADOW | runtime -->
Source: spec/core/governance.md#a-new-decider-runs-in-shadow-first | Promoting a decider from shadow to enforcing MUST carry its shadow evidence with the write that promotes it.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | A block MUST short-circuit the rest of its pipeline.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | A hook's outcome event MUST record the order in which the deciders ran.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | A pipeline MUST run lower-cost deterministic deciders before higher-cost ones.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | A session's configuration MUST give an ordered pipeline of deciders per hook.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | A substrate check MUST NOT be removable or reorderable by tenant or definition configuration.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | The effective pipeline at a hook MUST consist of the substrate's own checks, the tenant's governance deciders, and the definition's decider additions.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | Where the configured order and the cost order disagree, the cost order MUST prevail across cost classes.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-pipeline-runs-low-cost-deciders-first | Within one cost class, deciders MUST run in their declared order, the substrate's before the tenant's before the definition's.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-policy-decider-s-verdicts-map-onto-the-decisions | A deny from a policy decider MUST be a block unless every determining forbidding policy marks the deny as deferrable.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-policy-decider-s-verdicts-map-onto-the-decisions | A deny with no determining forbidding policy MUST be a block.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#a-policy-decider-s-verdicts-map-onto-the-decisions | An allow MUST become a flag when any determining permitting policy marks it as flagged.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A loop in a session's actions MUST be detected by a decider rather than by a function of an adapter.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A rollback MUST NOT remove a state-slot write.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A session's state slots together MUST be bounded apart from the transcript head's bound.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A state slot MUST be bounded per program by its catalog entry.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A state slot MUST be part of the session's durable state.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#a-program-decider-s-state-slots | A state slot MUST be read when an evaluation starts and written with the transition the hook gates.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | A block by a substrate check MUST NOT be escalated to a person.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | A retry after a block MUST be bounded by a budget.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | A retry after such a block MUST carry the reason without that attempt.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | A session whose retry budget is exhausted MUST block with the reason or escalate the block to an operator as a defer.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | Allowing an escalated block MUST be recorded as an override of the last attempt.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | An attempt that takes an effect MUST reset the count of consecutive blocked cycles.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | Consecutive blocked cycles per session MUST be bounded by a configured cap across turns.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | Every tool use in a blocked attempt MUST receive an error result before the next request.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | Spans a blocking decider marked MUST be redacted in every copy the platform stores or returns.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#a-retry-after-a-block-is-bounded | Where a provider binds reasoning content so that it cannot be edited, a secret found in it MUST block the output.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | A call in the never-automate set MUST NOT be deferred.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | A call in the never-automate set MUST be refused by a substrate check at the tool-call hook.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | A checker that includes a model MUST be able to block or flag a change.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/governance.md#agents-produce-evidence-and-never-approve | Weakening a property MUST be treated as a widening change.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#allow-and-flag | A flag MUST pass the subject on and record an event.
<!-- Allocation: H_S3#B_HOOK_PIPELINE | runtime -->
Source: spec/core/governance.md#allow-and-flag | An allow MUST pass the subject on unchanged.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A block on any part of a response MUST discard the rest of that response ungated and unstored.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A block's reason MUST pass the scrubbing decider, so that a block cannot put a secret back into context.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A blocked control operation MUST be rejected.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A blocked model output MUST be returned to the model with the block's reason rather than take effect.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A blocked tool call MUST become an error result to the model.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | A tool spec blocked at load MUST NOT be loaded.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | An event blocked at the notification-arrival hook MUST be applied as blocked, so that the inbox cursor advances past it.
<!-- Allocation: H_S6#B_HOOK_BLOCK_RETRY | runtime -->
Source: spec/core/governance.md#block | The model MUST NOT see content blocked at the notification-arrival hook.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A decider MUST be one of a pattern, a schema, a classifier, a sandboxed program, a declarative policy, a model judge, a session decider, or an external decider.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A decider MUST be treated as failed on an error, a passed deadline, exhausted time or memory bounds, a decision not legal at its hook, or an unreachable destination.
<!-- Allocation: H_S6#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A decider module or policy that fails validation MUST be refused when the configuration that references it is submitted, with the validator's diagnostics, rather than at the hook.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A default decider MUST run behind the decider interface rather than as policy code built into the core.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A judge, a session decider, or an external decider on a latency-sensitive hook MUST run only as a defer.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A program decider's module MUST import nothing beyond the decider interface.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A program instance MUST NOT be shared across tenants.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A sandboxed program decider MUST be bounded in time and memory.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A sandboxed program decider MUST be deterministic.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A sandboxed program decider MUST run with no access to the network or the filesystem, as a pure function of the decision request and its state slot.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A schema decider MUST validate the subject against its declared schema in process, allowing a subject that validates and denying one that does not with the validator's message as the reason.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A schema decider whose validator cannot run MUST count as the decider's declared failure handling.
<!-- Allocation: H_S6#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A schema that does not compile MUST refuse the configuration that carries it.
<!-- Allocation: H_S6#B_HOOK_SESSION_DECIDER | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A session decider MUST be an ephemeral session started to answer one decision request and ended when it answers.
<!-- Allocation: H_S6#B_HOOK_SESSION_DECIDER | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A session decider MUST be treated as a checker that includes a model.
<!-- Allocation: H_S6#B_HOOK_SESSION_DECIDER | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A session decider's answer MUST be one of the closed set of decisions.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | A tenant-supplied program decider MUST be evaluated in an instance that carries no state from any earlier evaluation.
<!-- Allocation: H_S5#B_HOOK_SANDBOX_SCHEMA | runtime -->
Source: spec/core/governance.md#deciders-are-one-of-a-closed-set-of-kinds | Every decider MUST have a deadline, configured with a default per decider kind.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#deciders-guard-inputs-as-well-as-outputs | A tool MUST be dispatched only by a session's own model output.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#deciders-guard-inputs-as-well-as-outputs | The tool-result and notification-arrival hooks MUST guard what enters a session.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#decisions-are-cached-by-content | A cached decision of a policy decider MUST be keyed by the versions of the policy set and of the grant snapshot it evaluated.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#decisions-are-cached-by-content | A decision served from the cache MUST NOT be counted as a decider call.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#decisions-are-cached-by-content | A decision served from the cache MUST be published as an event marked as cached.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#decisions-are-cached-by-content | A stateful or temporal decider's decision MUST NOT be cached.
<!-- Allocation: H_S5#B_HOOK_STATE_CACHE | runtime -->
Source: spec/core/governance.md#decisions-are-cached-by-content | A stateless decider's decision MUST be cacheable by the decider version and the input's content hash.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A defer MUST hold the deferred call rather than the session.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A deferred call's decision MUST arrive as an inbox event.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A deferred control operation that is allowed MUST be checked again against its target's current state and the current grants.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A deferred control operation that no longer applies when allowed MUST lapse with an event.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A deferred tool call that is allowed MUST pass the loaded-definition and revocation checks again before it is dispatched.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A deferred tool call that is allowed MUST pass the tool-call hook's policy deciders again against the current grants before it is dispatched.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A new defer past the cap MUST be a block.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A session with no other runnable work while a call is deferred MAY be evicted.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | A session with other work MUST continue while a call is deferred.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | Resolving a defer MUST be a control operation that passes the control-operation hook.
<!-- Allocation: H_S6#B_HOOK_DEFER | runtime -->
Source: spec/core/governance.md#defer | The number of open defers a session holds MUST be bounded by a configured cap.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/core/governance.md#each-decision-is-legal-only-at-some-hooks | A configuration that lists a decider whose declared decision set includes a decision not legal at its hook MUST be rejected at validation.
<!-- Allocation: H_S3#B_HOOK_DECISION_SHAPE | runtime -->
Source: spec/core/governance.md#each-decision-is-legal-only-at-some-hooks | A configuration that places a judge, a session decider, or an external decider synchronously on a latency-sensitive hook MUST be rejected at validation.
<!-- Allocation: H_S3#B_HOOK_ATOMIC_EVENTS | runtime -->
Source: spec/core/governance.md#every-decision-is-an-event | A fail-retry-pass episode, a shadow result, and an operator override MUST be recorded as training labels.
<!-- Allocation: H_S3#B_HOOK_ATOMIC_EVENTS | runtime -->
Source: spec/core/governance.md#every-decision-is-an-event | A hook's decisions, their decision events, and the transition they gate MUST commit in one transaction under the partition's fence.
<!-- Allocation: H_S3#B_HOOK_ATOMIC_EVENTS | runtime -->
Source: spec/core/governance.md#every-decision-is-an-event | A re-driven evaluation after a crash MUST carry the same decision id as the lost one.
<!-- Allocation: H_S3#B_HOOK_ATOMIC_EVENTS | runtime -->
Source: spec/core/governance.md#every-decision-is-an-event | Every decision MUST be published as an event.
<!-- Allocation: H_S3#B_HOOK_FAIL_CLOSED | runtime -->
Source: spec/core/governance.md#failure-defaults-to-closed | A classifier that is not required to block MAY run as a flag without blocking the turn.
<!-- Allocation: H_S3#B_HOOK_FAIL_CLOSED | runtime -->
Source: spec/core/governance.md#failure-defaults-to-closed | A decider's failure handling MUST default to blocking.
<!-- Allocation: H_S3#B_HOOK_FAIL_CLOSED | runtime -->
Source: spec/core/governance.md#failure-defaults-to-closed | A substrate check MUST NOT be configured to fail open.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#output-is-gated-before-it-takes-effect | A live attach that sees output before it is gated MUST mark that output as ungated.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#output-is-gated-before-it-takes-effect | A model's reasoning content MUST pass the structured-secret check.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#output-is-gated-before-it-takes-effect | Anything that acts on an external system MUST be a tool call gated before it runs.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#output-is-gated-before-it-takes-effect | Model output MUST NOT take effect before it passes its deciders.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-builds-the-decision-request | A decision request MUST NOT carry session context beyond that descriptor.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-builds-the-decision-request | A decision request on the notification-arrival hook MUST carry a bounded activity descriptor: the session's phase, the work invested in the current step as tokens and time, the expected time to the next turn boundary, and the session's priority.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-builds-the-decision-request | A decision request's configuration MUST state whether a truncation denies.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-builds-the-decision-request | At each hook the core MUST build a decision request carrying the principal and chain, the hook and call-type tags, the content, and the session's usage.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-builds-the-decision-request | The structured-secret check MUST read the full content regardless of the decision request's size bound.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-defines-a-fixed-set-of-hooks | The core MUST NOT add a hook point without a code change.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-defines-a-fixed-set-of-hooks | The core MUST define a fixed set of hook points and enforce only at them.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-defines-a-fixed-set-of-hooks | The hook points MUST be the model request, the model output, the tool call, the tool result, the notification arrival, the compaction, the stop request, and the control operation.
<!-- Allocation: H_S6#B_HOOK_REQUEST | runtime -->
Source: spec/core/governance.md#the-core-only-enforces | The core MUST enforce the decision a decider returns rather than form a decision of its own.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A decision event for a redaction MUST NOT record the content.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A redaction by a tenant or definition decider MUST apply to what the model is shown rather than to what a read grant holder reads.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A secret in a tool result MUST be removed by a transform before the result reaches the model.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A transform MUST proceed with a stated patch, redacting spans or setting fields.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A transform that sets the model MUST select only a model that accepts the transcript as it stands.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A transform that sets the model MUST select only a model the session's definition or the tenant allows.
<!-- Allocation: H_S6#B_HOOK_TRANSFORM_AND_OUTPUT | runtime -->
Source: spec/core/governance.md#transform | A transform's patch to a tool call's arguments MUST pass the tool's schema check before the call runs.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | A defer on an action reserved for a person MUST have block as its timeout outcome.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | A defer on an action reserved for a person MUST have no automated resolver.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | A substrate default MUST defer an action reserved for a person.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | Administration of a tenant's root key or a signing root MUST be an action reserved for a person.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | An action the tenant's policy reserves for a person MUST carry a call-type tag.
<!-- Allocation: H_S3#B_RUNTIME_RESERVED_AUTHORITY | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#actions-policy-reserves-for-people-defer | An agent MUST NOT hold a credential that can administer a key.
<!-- Allocation: H_S3#B_CHECKERS_ONLY | runtime -->
Source: spec/decisions/authority-widening/checkers-only.md#a-deferral-to-a-person-blocks-instead | A decision request MUST NOT be routed to a person at runtime.
<!-- Allocation: H_S3#B_CHECKERS_ONLY | runtime -->
Source: spec/decisions/authority-widening/checkers-only.md#a-deferral-to-a-person-blocks-instead | A hook whose policy would defer a decision to a person MUST block instead.
<!-- Allocation: H_S3#B_CHECKERS_ONLY | runtime -->
Source: spec/decisions/authority-widening/checkers-only.md#only-a-sound-checker-widens-authority | Authority MUST be widened only by a sound checker.
<!-- Allocation: H_S4#C_CHARTER_REFERENCE_AUDIT | runtime -->
Source: spec/core/governance.md#reference-checks-produce-audit-information | The platform MUST check each turn reply's charter references against the authoritative charter revision effective for that turn.

---

ID: L
Title: Session creation, supervision and end
Prerequisites: C, CFG, H, M, S, T, X
Deliverable: Root/child/keyed creation, supervision, stop/end/remove and recurring-ID tombstones. Interface boundary: Imperative root creation and reconfiguration, child/fork authority, keyed instancing, supervised lifecycle, ending/close and removal tombstones.
Acceptance: Racing first events create one instance; scope controls suppress new work; ending preserves transcripts and settles or closes outstanding work. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: C | inferred | Requires open-dispatch identity, result and close/recovery interfaces for integration.
Dependency: CFG | inferred | Requires validated definition revision and adoption interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
Dependency: M | inferred | Requires model interruption and committed-loop recovery interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
Dependency: T | inferred | Requires deadline, cancellation and occurrence interfaces for integration.
Dependency: X | inferred | Requires owned transcript ranges and freeze/thaw interfaces for integration.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | A call id MUST be unique across every incarnation of its session id.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | A new incarnation of a session id MUST start its lease generation and its tail position above the values its tombstone carries.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | A session MUST be identified by its session id together with an incarnation.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | An end that leaves calls open MUST leave a pending-close record from which a close and a reconcile are resent for each call while its outcome is unknown and its cutoff has not passed.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | An ended session whose id can recur MUST leave a tombstone carrying its last lease generation and tail position.
<!-- Allocation: L_S5#A_SESSION_INCARNATION_CLOSE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-recurring-id-has-incarnations | Ending an incarnation MUST close each call it leaves open.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#every-session-records-its-origin | A session's origin MUST identify the party responsible for its existence.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#every-session-records-its-origin | A tenant's principal MUST be able to create a root session.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#every-session-records-its-origin | A tenant's principal MUST be able to end a root session it owns.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#every-session-records-its-origin | Every session MUST record its origin as one of a caller, a definition, a keyed template, or a parent session.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#liveness-is-decided-by-a-mechanism | The owner of the list of root sessions MUST NOT need to poll the platform to learn a session's state.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#liveness-is-decided-by-a-mechanism | The platform MUST deliver a session's lifecycle events to the owner of the list of root sessions.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/core/sessions-and-partitions.md#liveness-is-decided-by-a-mechanism | Whether a session that should be running is running MUST be decided by a mechanism rather than by a person checking.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A child MUST end with its parent unless the call that started it specified otherwise.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A first event that would exceed the instance cap MUST be refused with a reason event.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A keyed instance MUST be owned by its template.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A keyed template MUST be either a definition or a template a caller registered.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A keyed template MUST carry an instance cap.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A parent MUST be able to list its children.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A parent MUST be able to stop its children.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A partition MUST create a keyed instance on the first event addressed to its key.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | A sub-session or a fork MUST be owned by its parent.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | An event addressed to a template that has no configuration in force, or that has been retired, MUST NOT create a session.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/core/sessions-and-partitions.md#platform-managed-sessions-are-supervised | An event addressed to a template that has no configuration in force, or that has been retired, MUST be dropped with a reason event.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#removing-a-session | A read of a removed session MUST answer not found.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#removing-a-session | A removal MUST take everything the durable store holds for the session out of it in one commit.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#removing-a-session | A removal of a session at rest MUST be accepted, whether the session is idle, blocked, or ended.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#removing-a-session | A removal of a session that has not ended MUST end the session before its record is removed, so that its transcript is frozen first.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#removing-a-session | A tenant's principal MUST be able to remove a session it owns.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#starting-and-ending-are-low-cost | A publish or send to a session that has ended MUST write nothing.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#starting-and-ending-are-low-cost | A session that has ended MUST refuse every operation except its removal, with a stable code.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#starting-and-ending-are-low-cost | Ending a session MUST freeze its transcript into the blob store.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#starting-and-ending-are-low-cost | Ending a session MUST publish its final observation record.
<!-- Allocation: L_S5#A_SESSION_END_REMOVE | runtime -->
Source: spec/core/sessions-and-partitions.md#starting-and-ending-are-low-cost | Ending a session MUST release its partition resources.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-is-its-own-principal | A fork MUST inherit none of its parent's timers.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-starts-from-its-parent-s-transcript | A fork MUST diverge from the fork point without modifying its parent.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-starts-from-its-parent-s-transcript | A fork MUST pin its parent's frozen blobs and copy only the unfrozen head.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-starts-from-its-parent-s-transcript | A fork MUST take its parent's transcript up to the fork point and its parent's definition narrowed by the call.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-starts-from-its-parent-s-transcript | A fork's first request MUST follow its parent's route, so that forking costs a cache read rather than a rewrite.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-fork-starts-from-its-parent-s-transcript | A fork's grants and budget MUST be its parent's, narrowed.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A child's depth and count of sub-sessions MUST be bounded by its parent's remaining allowance.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A child's model MUST be one its parent's configuration allows.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A parent's deciders MUST apply to its child in addition to the child's own.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A request to create or reconfigure a child beyond its parent's authority MUST be refused at the call with the reason.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A sub-session MUST NOT hold authority its parent lacks.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-is-never-wider-than-its-parent | A sub-session MUST run its parent's definition narrowed by the call, or another of the tenant's definitions as a keyed instance with the parent as requester.
<!-- Allocation: L_S5#C_SUBSESSION_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-reports-to-its-parent | A child's end MUST be delivered to its parent's inbox in the same commit that ends the child.
<!-- Allocation: L_S5#C_SUBSESSION_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-reports-to-its-parent | A sub-session's final output MUST return as the result of the call that started it.
<!-- Allocation: L_S5#C_SUBSESSION_RESULTS | runtime -->
Source: spec/core/tools-and-dispatch.md#a-sub-session-reports-to-its-parent | A sub-session's lifecycle events MUST arrive in its parent's inbox.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | A principal with a grant MUST be able to fork a session at any step of its transcript.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | A tool call on an experiment fork MUST pass the tool-call hook before it reaches the interceptor.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | An experiment fork MUST NOT act on an external system.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | An experiment fork MUST be discarded when its experiment ends.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | An experiment fork MUST route every tool call to the interceptor its experiment designates rather than to the tool's server.
<!-- Allocation: L_S5#C_EXPERIMENT_FORK | runtime -->
Source: spec/core/tools-and-dispatch.md#an-experiment-fork-is-intercepted | An experiment fork's transcript MUST be readable as a tail by the experiment's principal.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#fork-queries-are-discarded | A fork query MUST leave the original session's state unchanged.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#fork-queries-are-discarded | A fork started to answer a question MUST be discarded once it answers.
<!-- Allocation: L_S5#C_FORK_STATE_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#fork-queries-are-discarded | A fork started to answer a question about its parent MUST run with model-only tools.
<!-- Allocation: L_S5#C_SUBSESSION_AUTHORITY | runtime -->
Source: spec/core/tools-and-dispatch.md#recurring-work-uses-a-new-sub-session | The sub-session tool SHOULD direct recurring work, and work that needs only a short instruction rather than the parent's transcript, to a new sub-session rather than to a fork.
<!-- Allocation: L_S3#A_IMPERATIVE_EXCLUSIVITY | runtime -->
Source: spec/decisions/session-management/imperative-only.md#the-platform-creates-no-root-session-on-its-own | A keyed instance MUST be created only from a template a caller registered.
<!-- Allocation: L_S3#A_IMPERATIVE_EXCLUSIVITY | runtime -->
Source: spec/decisions/session-management/imperative-only.md#the-platform-creates-no-root-session-on-its-own | The platform MUST NOT create a root session except at a caller's request.
<!-- Allocation: L_S3#A_IMPERATIVE_EXCLUSIVITY | runtime -->
Source: spec/decisions/session-management/imperative-only.md#the-platform-creates-no-root-session-on-its-own | The platform MUST NOT keep a desired set of root sessions.
<!-- Allocation: L_S3#A_IMPERATIVE_EXCLUSIVITY | runtime -->
Source: spec/decisions/session-management/imperative-only.md#the-platform-creates-no-root-session-on-its-own | The platform MUST NOT read agent configuration from any source other than a caller's request.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#a-caller-holds-a-grant | A caller MUST hold a grant covering the operation to create, end, or reconfigure a root session or to register a keyed template.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#a-refusal-over-a-cap-states-the-cap | A request refused because it would exceed the root-session cap MUST carry a status that states the cap.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#a-refusal-over-a-cap-states-the-cap | The number of root sessions a tenant owns MUST be bounded by a configured cap.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/decisions/session-management/imperative.md#a-template-is-a-configuration-plus-a-key-scheme | A keyed template MUST consist of a configuration and a key scheme.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/decisions/session-management/imperative.md#a-template-is-a-configuration-plus-a-key-scheme | A registered template MUST be owned by the principal that registered it.
<!-- Allocation: L_S5#A_SESSION_CHILDREN_TEMPLATES | runtime -->
Source: spec/decisions/session-management/imperative.md#a-template-is-a-configuration-plus-a-key-scheme | The API MUST accept registration of a keyed template.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#each-operation-passes-the-control-operation-hook | Creating a root session, ending it, reconfiguring it, and registering a keyed template MUST each be a control operation.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#lifecycle-events-reach-the-owner | A root session's configuration MUST identify the event hook destinations that receive its lifecycle events.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#lifecycle-events-reach-the-owner | The platform MUST deliver a root session's lifecycle events to its owner through event hooks.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-api-accepts-creation-with-configuration | A root session created through the API MUST be owned by the caller's principal.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-api-accepts-creation-with-configuration | A root session created through the API MUST record a caller as its origin.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-api-accepts-creation-with-configuration | The API MUST accept creation of a root session together with its configuration.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-api-accepts-ending | The API MUST accept ending a root session.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-owner-decides-whether-a-root-session-exists | The owner of a root session MUST be the party responsible for deciding whether it should exist.
<!-- Allocation: L_S5#A_SESSION_ROOT_OWNERSHIP | runtime -->
Source: spec/decisions/session-management/imperative.md#the-owner-decides-whether-a-root-session-exists | The set of root sessions MUST be reconciled by the owning system rather than by a person.
<!-- Allocation: L_S5#OPTIONAL_IDLE_END | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#retention-is-per-tenant | A tenant MAY configure an idle duration after which a session ends.

---

ID: M
Title: Direct model and context loop
Prerequisites: C, H, O, S, X
Deliverable: Owned model/tool loop, gated streaming, interruption, compaction and prompt-cache handling. Interface boundary: Provider-neutral direct turn loop, streaming gate/interrupt boundary, scheduling/admission, lawful capacity routing, compaction and prompt-cache policy.
Acceptance: Real provider failures and interrupts preserve committed progress; no output takes effect before gating; context and usage remain bounded and attributable. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: C | inferred | Requires open-dispatch identity, result and close/recovery interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
Dependency: O | inferred | Requires stable work identities, usage append and audit export interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
Dependency: X | inferred | Requires owned transcript ranges and freeze/thaw interfaces for integration.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#interrupting-a-turn-cancels-only-the-model | A running side-effecting tool MUST NOT be cancelled by a notification.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#interrupting-a-turn-cancels-only-the-model | The result of a tool that was running when a notification arrived MUST be recorded before the notification is delivered.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-session-decides-whether-a-notification-interrupts | A notification whose priority does not exceed the threshold MUST wait for the next turn boundary.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-session-decides-whether-a-notification-interrupts | A notification whose priority exceeds the session's current interrupt threshold MUST interrupt the work in progress.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | A session MUST be able to raise or lower its own interrupt threshold through a native tool.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | A transform at the notification-arrival hook MAY set whether the notification interrupts now or waits for the turn boundary.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | An idle session MUST wake for any notification.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | The computed interrupt threshold MUST decide delivery unless a transform set the delivery field.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | The threshold function MUST be configuration.
<!-- Allocation: M_S4#A_NOTIFICATION_INTERRUPT | runtime -->
Source: spec/core/events-and-subscriptions.md#the-threshold-is-dynamic-and-configured | When no notification-arrival decider is configured, the partition owner MUST compute the interrupt threshold from the session's state without building a decision request.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-context-window-exceeded | A model attempt that exceeds the context window MUST be classified as such rather than retried unchanged.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/core/failure-handling.md#a-failure-mid-stream | A retry after a reply that failed mid-stream MUST be a new attempt with its own attempt id.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/core/failure-handling.md#a-policy-block | A request blocked by policy MUST NOT be resent unchanged.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/core/failure-handling.md#a-policy-block | A session whose request was blocked by policy MUST take the handling the session's policy decides.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-refusal-a-malformed-reply-or-a-truncated-reply | A model refusal or a provider's content filter MUST be handled as a policy block.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-refusal-a-malformed-reply-or-a-truncated-reply | A model reply cut at the output cap MUST be continued by a further turn from the point of truncation.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-refusal-a-malformed-reply-or-a-truncated-reply | A model reply that fails a declared output format MUST be retried with the specific validation error, within the configured attempts.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-throttled-or-unavailable-model | A model attempt that is throttled, overloaded, timed out, or failed internally MUST be retried with backoff and jitter within the turn's attempt budget.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-throttled-or-unavailable-model | A retry of a throttled model attempt MUST honor the provider's retry-after when one is given.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-throttled-or-unavailable-model | A session whose turn exhausts its attempt budget against a model MUST end blocked with the reason.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#a-throttled-or-unavailable-model | The platform MUST keep a circuit breaker per model and endpoint.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#an-invalid-request | A model attempt rejected as invalid MUST NOT be retried.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#an-invalid-request | A session whose request was rejected as invalid MUST end blocked, with an event carrying the reason.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/core/failure-handling.md#every-blocked-or-quarantined-state-has-an-exit | A turn that the budget check denies MUST wait, with its session idle rather than blocked, while the budget does not allow it.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#every-side-effect-can-be-redone | A model attempt's idempotency key MUST be its attempt id.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/core/failure-handling.md#every-unit-of-work-has-a-deadline | Every model turn MUST have a deadline.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#a-charter-change-schedules-a-compaction | A change to a session's prompt or policy MUST schedule a low-priority compaction that merges the change into the system prompt.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#a-charter-change-schedules-a-compaction | A charter change arriving while a compaction is already due MUST be included in that compaction.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#a-charter-change-schedules-a-compaction | A compaction that follows a charter change MUST write its summary under the new charter.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#a-charter-change-schedules-a-compaction | Charter changes MUST be debounced, so that a burst of them costs one compaction.
<!-- Allocation: M_S4#C_COMPACTION_FAILURES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-failures-are-bounded | A compaction a turn depended on that exhausts its attempts MUST block the session with the reason.
<!-- Allocation: M_S4#C_COMPACTION_FAILURES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-failures-are-bounded | A compaction requested on its own that exhausts its attempts MUST leave the session idle with its transcript unchanged.
<!-- Allocation: M_S4#C_COMPACTION_FAILURES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-failures-are-bounded | A compaction that fails MUST be retried within the configured attempts.
<!-- Allocation: M_S4#C_COMPACTION_FAILURES | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-failures-are-bounded | A compaction the provider answers with no summary MUST leave the transcript unchanged.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A compaction MAY run inline within a step's own request where the provider compacts on a request-side threshold.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A compaction MUST cover the transcript up to a cut point.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A compaction a provider returns inside a reply MUST be applied as a compaction, with the messages it covers archived, the transcript renumbered, and the compaction counted.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A compaction result MUST be swapped in only at a turn boundary.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A compaction's priority MUST rise as the context nears the hard threshold, so that a continuous stream of events cannot postpone it indefinitely.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A session MUST ask for compaction at its soft threshold through whichever path the provider offers.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A session MUST continue to process turns while its compaction runs.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | A turn that arrives during a compaction MUST run on the uncompacted context.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | An event that exceeds the session's interrupt threshold MUST preempt a running compaction, which is then rescheduled.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | Cancelling a compaction MUST NOT change the session's durable state.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | Cancelling a compaction MUST take effect by the next turn boundary.
<!-- Allocation: M_S4#C_COMPACTION_CONCURRENCY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#compaction-runs-beside-the-turns | The answer carried beside such a compaction MUST be kept as the step's answer rather than the step run again.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#every-cache-setting-is-measured | Every cache setting MUST be configuration, so that a change to it rolls out as a definition version.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#method | A compaction MAY run in the batch latency class.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#method | A compaction MUST count against the session's budget.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#method | A compaction MUST use the provider's native compaction where the provider offers one.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#method | The platform's own summarizer MUST be used only as a fallback.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-keep-set-stays-verbatim | A compaction MUST keep the keep set verbatim: plans, open items, unresolved defects, constraints, decisions, and identifiers the work depends on.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-keep-set-stays-verbatim | A compaction MUST keep the recent tail verbatim.
<!-- Allocation: M_S5#PENDING_TIMER_COMPACTION | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-keep-set-stays-verbatim | The keep set MUST include the session's pending timers with their notes.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-platform-avoids-full-rewrites | A low-priority wake MAY be delayed and merged into the session's next wake, where its deadline allows, so that it runs while the cache is warm.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-platform-avoids-full-rewrites | The platform MUST compact before a long idle, so that the rewrite on the next wake is small.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-platform-manages-the-lifetime | The platform MUST choose a session's cache lifetime per session, taking a longer lifetime where the model endpoint offers one and the session's wake history shows that the longer lifetime reduces cost.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-platform-manages-the-lifetime | The platform MUST keep a prefix warm across an expected short gap with a refresh read before it expires, when the next wake is likely inside the window.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-platform-manages-the-lifetime | The platform MUST let a prefix expire when the next wake is not expected inside the window.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-policy-comes-from-the-definition | A session's cache policy MUST come from its definition.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#thresholds-are-per-session-and-below-the-context-window | A session's definition MUST set a hard threshold at which compaction becomes a precondition of the next turn.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#thresholds-are-per-session-and-below-the-context-window | A session's definition MUST set a soft threshold at which a background compaction starts.
<!-- Allocation: M_S4#C_COMPACTION_THRESHOLDS_METHOD | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#thresholds-are-per-session-and-below-the-context-window | Both thresholds MUST be below the model's context window.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-model-switch-never-fails-a-turn | A switch of model mid-session MUST NOT fail a turn because of reasoning produced by another model.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-model-switch-never-fails-a-turn | A tool call made under one provider and answered under another MUST render as a valid call-and-result pair under the provider that renders the request.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-request-is-a-function-of-the-log-and-policy | A model request MUST be a pure function of the session's durable log and its policy.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-request-is-a-function-of-the-log-and-policy | A rendered segment MUST depend only on its content, the provider's wire format, and a render version, so that it is cached by content and shared across sessions.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-source-is-an-account-a-region-and-a-model-endpoint | A capacity source MUST be an account, a region, and a model endpoint, reached through a role.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-source-is-an-account-a-region-and-a-model-endpoint | A scheduler MUST route across capacity sources by spare capacity and cost.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A capacity source MUST come from the tenant's configuration rather than from an agent definition.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A node's permission to call a model endpoint MUST be limited to the models and endpoints its capacity sources list.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A scheduler MUST route a session's model call only to a region the tenant's policy permits.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A tenant-supplied role MUST be used only if it is on the tenant's trusted list.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A transform MUST NOT set the capacity source that serves a model.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#a-tenant-s-own-capacity-comes-from-tenant-configuration | A turn MUST NOT start for a tenant whose configuration supplies no capacity source.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#admission-adapts-to-capacity | A scheduler MUST admit against the model endpoint's known request and token budgets rather than discover them by throttling.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#admission-adapts-to-capacity | A scheduler MUST admit waiting turns per model and endpoint in priority order, breaking ties by deadline.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#admission-adapts-to-capacity | A throttle MUST narrow the admission window rather than fail a session that is inside its deadline.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#cache-boundaries-coincide | A breakpoint that no block can carry MUST be dropped and reported rather than fail the turn.
<!-- Allocation: M_S5#C_PROMPT_CACHE_POLICY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#cache-boundaries-coincide | A cache breakpoint MUST be placed at a segment boundary, so that the client's render cache and the provider's prompt cache invalidate the same suffix.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#changes-to-the-prefix-preserve-the-cache | A change to the cached prefix MUST be made through the provider's cache-preserving path or at a compaction.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#changes-to-the-prefix-preserve-the-cache | The system prompt MUST NOT be edited in place outside a compaction.
<!-- Allocation: M_S4#C_MODEL_TRANSPORT_FACTS | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#connections-are-shared-and-bounded | The transport MUST enforce a provider's per-connection concurrency limit before sending rather than discover it by refusal.
<!-- Allocation: M_S4#C_MODEL_TRANSPORT_FACTS | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#connections-are-shared-and-bounded | The transport MUST enforce the configured request body cap before a body is sent.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#failures-are-classified-structurally | A context-window overflow MUST be classified as such, so that the turn loop compacts rather than retries.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#failures-are-classified-structurally | A provider failure MUST be classified from the provider's structured fields rather than by matching message text, except where a provider offers no structured field for the condition.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#fair-share | A scheduler MUST weight admission per tenant and, within a tenant, by priority, from configuration.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#fair-share | Budgets MUST be applied at the model-request hook.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#no-call-is-made-directly-to-a-model-endpoint | A session MUST NOT call a model endpoint directly.
<!-- Allocation: M_S4#C_MODEL_ADMISSION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#no-call-is-made-directly-to-a-model-endpoint | Every model call MUST pass through a scheduler on the partition owner's node.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#one-provider-neutral-transcript | A provider-bound payload MUST be carried opaquely, tagged with its provider, and interpreted only by that provider's adapter.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#one-provider-neutral-transcript | A session's transcript MUST be held in one provider-neutral form from which each provider's request is rendered.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#one-provider-neutral-transcript | Provider state such as reasoning or a compaction item MUST be round-tripped unmodified.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-follows-the-work | A definition's base priority MUST be bounded by the upper bound of the binding or grant under which the definition arrived.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-follows-the-work | A session's effective priority MUST be the highest of its base priority, the priority of the work it is handling, and any priority inherited from a session waiting on it.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-follows-the-work | A session's effective priority MUST return to its base priority when the work that raised it is done.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-follows-the-work | Raising a session's priority MUST be a granted capability checked at the control-operation hook.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-orders-everything-that-queues | Eviction, when a partition needs memory, MUST remove the lowest-priority idle sessions first.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-orders-everything-that-queues | Model admission MUST be ordered by priority.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-orders-everything-that-queues | The deciders' compute pool MUST serve by priority.
<!-- Allocation: M_S4#C_PRIORITY_PROPAGATION | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#priority-orders-everything-that-queues | The order in which a partition processes its dirty sessions MUST be by priority.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#quota-is-a-cluster-resource | A model call MUST NOT wait on a quota-coordination call.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#quota-is-a-cluster-resource | A share of quota MUST be reservable for a priority class, so that urgent work is admitted while background work saturates the rest.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#quota-is-a-cluster-resource | Quota shares MUST rebalance by demand.
<!-- Allocation: M_S4#C_MODEL_RENDERING | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#reasoning-binding-is-explicit | Where a provider binds reasoning to the history it was produced in, the platform MUST set the binding policy explicitly rather than rely on the provider's default.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#retention-terms-are-honored | A scheduler MUST route a session only to a model whose data-retention terms the tenant's policy permits.
<!-- Allocation: M_S4#C_MODEL_CAPACITY_TRUST | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#routes-are-sticky | A session's route to a capacity source MUST be sticky, so that its cached prefix stays warm.
<!-- Allocation: M_S4#C_MODEL_TRANSPORT_FACTS | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#the-platform-keeps-each-model-s-facts | The platform MUST keep, for every model it routes to, the model's context window, its maximum output, and whether it supports tool use, streaming, reasoning, prompt caching, and image input.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#three-classes | A batch request's result MUST arrive as an inbox event, so that its session can be evicted while it waits.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#three-classes | Compaction, distillation, and retrospectives MUST default to the background or batch class.
<!-- Allocation: M_S5#C_QUOTA_LATENCY_CLASSES | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#three-classes | The platform MUST offer interactive, background, and batch latency classes.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/decisions/execution/direct.md#a-context-window-exceeded-compacts-and-retries | A model attempt that exceeds the context window MUST cause a compaction followed by a retry of the turn.
<!-- Allocation: M_S4#C_MODEL_RETRY_CLASSIFICATION | runtime -->
Source: spec/decisions/execution/direct.md#a-context-window-exceeded-compacts-and-retries | A session whose compaction cannot bring the prompt under the context window MUST end blocked with the reason.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/decisions/execution/direct.md#a-failure-mid-stream-discards-the-reply | A model reply that fails mid-stream MUST be discarded in full.
<!-- Allocation: M_S5#C_CHARTER_PREFIX_UPDATES | runtime -->
Source: spec/decisions/execution/direct.md#a-prefix-change-is-a-mid-conversation-instruction | A prompt or policy change MUST be applied as a mid-conversation instruction.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/decisions/execution/direct.md#a-retry-carries-the-blocked-attempt | A retry after a blocked output MUST carry the blocked attempt up to the refused block, followed by a platform-authored message with the block's reason.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/decisions/execution/direct.md#interrupting-a-turn-cancels-the-stream | Interrupting a model turn MUST cancel the stream and discard the partial output.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/decisions/execution/direct.md#output-is-gated-per-block | Model output MUST be gated per completed block.
<!-- Allocation: M_S4#C_MODEL_STREAM_GATES | runtime -->
Source: spec/decisions/execution/direct.md#the-threshold-is-computed-within-a-step | The interrupt threshold MUST be computed from the session's state, the work already done in the current step, the expected remaining duration of the step, and the session's own priority.
<!-- Allocation: M_S4#C_CHARTER_REPLY_METADATA | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | Every turn reply MUST carry structured charter-reference metadata.
<!-- Allocation: M_S4#C_CHARTER_REPLY_METADATA | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | A turn reply's charter-reference metadata MUST identify the authoritative charter revision effective at that turn's boundary.
<!-- Allocation: M_S4#C_CHARTER_REPLY_METADATA | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | A turn reply's charter-reference metadata MUST identify the specific charter clauses governing its response or actions.
<!-- Allocation: M_S4#CORE_CONFIGURED_EXECUTION | runtime -->
Source: constitution.md#p1-the-core-executes-and-decides-no-policy-that-configuration-can-supply | The core MUST execute sessions, deliver events, call models, run tools, and persist state without deciding any policy a tenant could supply as configuration.

---

ID: O
Title: Usage and observation
Prerequisites: S
Deliverable: Per-work records, causal attribution, one usage stream and independently retained audit/usage export. Interface boundary: Stable work identity and once-only usage append, causal/content-free schema, bounded metric dimensions, observation pointers and independent export retention.
Acceptance: Actual model/tool/decider sites emit once per required identity; budgets and metering agree; export resumes without loss. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
<!-- Allocation: O_S6#C_STATE_EVENTS_METRICS | runtime -->
Source: constitution.md#iii-everything-is-observable | A metric MUST be split only by dimensions drawn from closed sets.
<!-- Allocation: O_S6#C_STATE_EVENTS_METRICS | runtime -->
Source: constitution.md#iii-everything-is-observable | An identifier of an open set, such as a tenant, a session, a principal, or a tool name, MUST be carried on events as a field rather than as a metric dimension.
<!-- Allocation: O_S6#C_STATE_EVENTS_METRICS | runtime -->
Source: constitution.md#iii-everything-is-observable | Every state change the platform makes MUST be published as a specific event.
<!-- Allocation: O_S5#C_IMPROVEMENT_FEEDBACK | runtime -->
Source: constitution.md#p7-the-system-improves-itself-through-its-own-primitives | Every output the platform produces about its own behavior, such as a usage record, an observation record, or a decision episode, MUST be data that can feed back as configuration through the same change control as any other configuration.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-model-attempt-s-record-carries-its-cause-and-outcome | A model attempt's record MUST carry the cache reads and cache writes the attempt incurred.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-model-attempt-s-record-carries-its-cause-and-outcome | A model attempt's record MUST carry the prompt size.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-model-attempt-s-record-carries-its-cause-and-outcome | A model attempt's record MUST identify the model and the capacity source that served it.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-model-attempt-s-record-carries-its-cause-and-outcome | A model attempt's record MUST state the turn's outcome as no action, or the actions it took.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-model-attempt-s-record-carries-its-cause-and-outcome | A model attempt's record MUST state what caused the turn: an event, a timer, an operator message, or a stop-admission continuation.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-its-tenant-and-actors | A usage record MUST carry its tenant.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-its-tenant-and-actors | A usage record MUST carry the attribution tags the definition configured, such as a project or a task.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-its-tenant-and-actors | A usage record MUST carry the digest of the loaded definition the session ran under.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-its-tenant-and-actors | A usage record MUST carry the session's principal chain.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-its-tenant-and-actors | A usage record MUST identify the session and its parent session, if any.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/contracts/usage-record.md#a-record-carries-no-content | A usage record MUST NOT carry transcript content.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | A decision served from the decision cache MUST NOT produce a usage record, because no decider was called.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | A model attempt interrupted before it completed MUST have its usage record written by the next owner, marked interrupted, with its token counts recorded as unknown.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | A shadow decider call MUST have its own usage record marked as shadow.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | Exactly one usage record MUST exist for every decider call.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | Exactly one usage record MUST exist for every model attempt.
<!-- Allocation: O_S6#C_USAGE_IDEMPOTENCE | runtime -->
Source: spec/contracts/usage-record.md#every-attempt-is-recorded-once | Exactly one usage record MUST exist for every tool call.
<!-- Allocation: O_S5#C_USAGE_EXPORT | runtime -->
Source: spec/contracts/usage-record.md#the-stream-has-its-own-retention | A partition MUST deliver its usage records to their sink within the configured interval.
<!-- Allocation: O_S5#C_USAGE_EXPORT | runtime -->
Source: spec/contracts/usage-record.md#the-stream-has-its-own-retention | Usage records MUST have a retention period of their own, separate from session data.
<!-- Allocation: O_S5#A_ALARM_OPERABILITY | runtime -->
Source: spec/core/durability-and-deployment.md#alarms-derive-from-the-platform-s-own-events | A node MUST export its resident memory and its allocator's gauges in every stage.
<!-- Allocation: O_S5#A_ALARM_OPERABILITY | runtime -->
Source: spec/core/durability-and-deployment.md#alarms-derive-from-the-platform-s-own-events | Alarms and dashboards MUST derive from the platform's own metric definitions.
<!-- Allocation: O_S5#A_ALARM_OPERABILITY | runtime -->
Source: spec/core/durability-and-deployment.md#alarms-derive-from-the-platform-s-own-events | An alarm on a gauge that must stay at a value MUST treat missing data as breaching.
<!-- Allocation: O_S5#A_ALARM_OPERABILITY | runtime -->
Source: spec/core/durability-and-deployment.md#alarms-derive-from-the-platform-s-own-events | Every alarm MUST have a runbook.
<!-- Allocation: O_S5#A_ALARM_OPERABILITY | runtime -->
Source: spec/core/durability-and-deployment.md#alarms-derive-from-the-platform-s-own-events | Every alarm MUST have fired once, in a drill or a synthetic test, before its stage carries production work.
<!-- Allocation: O_S5#A_SUPERVISOR_ACTIVITY | runtime -->
Source: spec/core/events-and-subscriptions.md#a-supervisor-session-observes-another-session | A session holding a read grant on another session MUST be able to receive that session's gated activity as inbox events as the activity occurs.
<!-- Allocation: O_S5#A_SUPERVISOR_ACTIVITY | runtime -->
Source: spec/core/events-and-subscriptions.md#a-supervisor-session-observes-another-session | A supervisor session MUST NOT slow the session it observes.
<!-- Allocation: O_S5#A_SUPERVISOR_ACTIVITY | runtime -->
Source: spec/core/events-and-subscriptions.md#a-supervisor-session-observes-another-session | A supervisor session MUST see only output that has passed the observed session's deciders.
<!-- Allocation: O_S5#B_AUTH_AUDIT | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-decision-and-credential-is-audited | An audit event MUST NOT hold content.
<!-- Allocation: O_S5#B_AUTH_AUDIT | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-decision-and-credential-is-audited | An audit event MUST hold ids, principals, and decisions.
<!-- Allocation: O_S5#B_AUTH_AUDIT | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-decision-and-credential-is-audited | An authentication event MUST carry the verifier's request id when a verification ran, so that a refusal or a failure can be followed into the verifier's own records.
<!-- Allocation: O_S5#B_AUTH_AUDIT | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-decision-and-credential-is-audited | Audit events MUST be kept apart from session data, with their own retention, so that destroying a tenant's keys deletes its content but not the record of each action and its principal.
<!-- Allocation: O_S5#B_AUTH_AUDIT | runtime -->
Source: spec/core/identity-delegation-and-secrets.md#every-decision-and-credential-is-audited | Every authorization decision MUST be published as an event that identifies the grant or policy that decided it.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | A label MUST reference content rather than copy it.
<!-- Allocation: O_S6#ACTUAL_OBSERVATION_TRIGGERS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | A session MUST publish an observation record to a queue when it crosses a size threshold, and when it ends, compacts, or is asked to.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An episode MUST be recorded as an observation record that points at the transcript range of its attempts rather than copying them.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An episode's event MUST NOT carry content.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An episode's event MUST carry the episode's id and its range reference.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An observation record MUST carry the session and identity, the reason, the transcript range, the size in bytes and tokens, the model and configuration version, and timestamps.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An observation record MUST point at the same frozen blobs the transcript ranges refer to.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#a-session-publishes-observation-records | An observation record MUST point at the transcript rather than copy it.
<!-- Allocation: O_S6#C_STATE_EVENTS_METRICS | runtime -->
Source: spec/core/observation-and-usage.md#dependency-measurements-are-published | A dependency client's own measurements MUST be published through the node's metrics rather than dropped.
<!-- Allocation: O_S5#C_IMPROVEMENT_FEEDBACK | runtime -->
Source: spec/core/observation-and-usage.md#improvement-runs-through-the-platform-s-own-primitives | A proposed change MAY be evaluated on experiment forks of recorded sessions before it runs on a cohort.
<!-- Allocation: O_S5#C_IMPROVEMENT_FEEDBACK | runtime -->
Source: spec/core/observation-and-usage.md#improvement-runs-through-the-platform-s-own-primitives | An observer MUST propose a change as a new definition version through the change control that governs the definition rather than by writing configuration directly.
<!-- Allocation: O_S5#C_IMPROVEMENT_FEEDBACK | runtime -->
Source: spec/core/observation-and-usage.md#improvement-runs-through-the-platform-s-own-primitives | The signals by which a change is evaluated MUST come from the usage and observation streams.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A consumer whose observed range is no longer readable MUST record the range as unreadable rather than substitute content.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A copy of observed content held by a consumer MUST become unreadable within the configured bound when the key protecting the source content is destroyed.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A grant to read episodes MUST be scoped to episode records in the reader's tenant rather than to transcripts in general.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A label derived from observed content, such as a verdict, a reason, or a decider id, MAY outlive the content.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A principal reading a range an observation record points at MUST hold a read grant on the session.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | A reader MUST pass the access-scope check for the range it reads.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | An episode kept longer than its transcript MUST be transferred to a tenant-owned copy before the key protecting its range is destroyed.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | An observation record's pin MUST cover only the transcript ranges its records cite, never reasoning content, a stored tool output, a frozen session, or an audit copy.
<!-- Allocation: O_S5#C_OBSERVATION_ACCESS | runtime -->
Source: spec/core/observation-and-usage.md#reading-an-observed-range-is-granted-and-audited | Every read of an observed range MUST be audited.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/observation-and-usage.md#triggers-and-queues-are-configuration | The triggers for an observation record and its target queue MUST be configuration.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/core/observation-and-usage.md#versions-compare-on-one-stream | A model attempt's record MUST carry the definition digest the session ran, so that versions compare on the same stream.
<!-- Allocation: O_S5#C_USAGE_EXPORT | runtime -->
Source: spec/core/observation-and-usage.md#versions-compare-on-one-stream | Budget deciders MUST read the usage stream.
<!-- Allocation: O_S5#C_USAGE_EXPORT | runtime -->
Source: spec/core/observation-and-usage.md#versions-compare-on-one-stream | Metering MUST read the same usage stream rather than a separate one.
<!-- Allocation: O_S6#C_TOOL_OBSERVABILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-is-observable | A tool server the operators run MUST carry the call's trace id on every event it publishes for the call.
<!-- Allocation: O_S6#C_TOOL_OBSERVABILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-is-observable | A tool server the operators run MUST continue the caller's trace.
<!-- Allocation: O_S6#C_TOOL_OBSERVABILITY | runtime -->
Source: spec/core/tools-and-dispatch.md#dispatch-is-observable | Tool metrics MUST split only by executor kind and outcome.
<!-- Allocation: O_S4#C_USAGE_FIELDS | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#every-cache-setting-is-measured | Every usage record MUST carry the cache reads and writes of its attempt, so that the policy is measured per session and per definition.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#every-turn-publishes-its-outcome | A turn abandoned by its caller MUST be reported with the stage it reached.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#every-turn-publishes-its-outcome | Every stage of a turn MUST be an event at a level matching its frequency.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#every-turn-publishes-its-outcome | Every turn MUST publish one outcome event carrying its request id, model, stop or failure class, latencies, and token totals.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#metrics-split-by-closed-sets | Every token count MUST be split by model.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#metrics-split-by-closed-sets | Turn metrics MUST split only by model, wire format, endpoint, signing mode, error class, stage, stop reason, segment kind, and cache lifetime.
<!-- Allocation: O_S6#C_MODEL_OBSERVABILITY | runtime -->
Source: spec/decisions/execution/direct-model-turns-and-scheduling.md#scheduling-is-observable | A scheduler MUST publish queue depth, wait time, admitted concurrency, and throttles per model and model endpoint.
<!-- Allocation: O_S5#C_OBSERVATION_RECORDS | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | A record that cites a turn reply for audit inspection MUST preserve its association with that reply's charter-reference metadata.
<!-- Allocation: O_S5#C_CHARTER_AUDIT_RECORDS | runtime -->
Source: spec/core/governance.md#reference-checks-produce-audit-information | The platform MUST record invalid or mismatched charter references with the reply they came from for audit inspection.
<!-- Allocation: O_S6#OBSERVATION_MEMORY_WRITER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-memory-belongs-to-an-agent-identity | A memory MUST be recordable by a mechanism that observes the session rather than only by a tool call the agent makes.
<!-- Allocation: O_S6#OBSERVATION_MEMORY_WRITER | runtime -->
Source: spec/core/sessions-and-partitions.md#a-memory-belongs-to-an-agent-identity | A memory recorded by observation MUST carry the access scope of the transcript range it was derived from.

---

ID: R
Title: Receiver receipts and reconciliation
Prerequisites: AUTH, DA
Deliverable: Request equality, receipt lookup/admission, settlement, close and reconcile. Interface boundary: Receiver admission plus atomic receipt/effect, request equality, pending/final states, retention, reconcile/close and monotonic fence semantics.
Acceptance: Actual receiver effects and receipts retain equality, dedupe, generation and cutoff rules across retry and recovery. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: AUTH | inferred | Requires verified caller, assertion and authenticated transport interfaces for integration.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-answers-reconcile | A repeat of a call and a reconcile of it MUST NOT disagree.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-answers-reconcile | A service with a reconcile binding MUST answer, for a call id, one of never admitted, still running, interrupted, settled, or closed, with the outcome where there is one.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A call whose id has a receipt under a different request digest MUST be refused with a final error and receive nothing from the receipt.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A close MUST be idempotent.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A close of a call in flight MUST end the call.
<!-- Allocation: R_S6#C_RECEIVER_AUTH_REFUSALS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A close or a reconcile MUST compare its target call id with the assertion's call claim before it reads or writes anything.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A receipt MUST be settled at most once.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A receipt for a call whose effect takes place inside the service's store MUST commit with the effect.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A receipt for a call whose effect takes place outside the service's store MUST be admitted before the effect and settled after it.
<!-- Allocation: R_S6#C_RECEIVER_AUTH_REFUSALS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A refusal decided before admission MUST record nothing and leave the fence unchanged.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A refusal the effect itself returns after admission MUST be settled as the call's outcome and replayed on retry.
<!-- Allocation: R_S6#C_RECEIVER_AUTH_REFUSALS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service MUST establish the caller of every request, the handshake included, before any method runs.
<!-- Allocation: R_S3#C_RECEIVER_RETENTION | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service MUST keep every receipt for at least the configured call deadline upper bound plus the configured outcome-unknown window after it writes the receipt, measured on its own clock.
<!-- Allocation: R_S6#C_RECEIVER_AUTH_REFUSALS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service MUST refuse a call whose caller lacks the tool's declared scope with a refusal that identifies the scope and is distinct from an authentication failure.
<!-- Allocation: R_S3#C_RECEIVER_RETENTION | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service MUST refuse to start with a receipt retention below that sum.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service that receives a call id it holds a receipt for under the same request digest MUST return the recorded outcome without running the call again.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service that receives a close for a call id it holds no receipt for MUST write a closed receipt, so that a later admission of that call id is refused.
<!-- Allocation: R_S3#C_RECEIVER_CLOSE_RECONCILE | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service the platform's operators run MUST answer a close for a call id with the call's final state: closed, in flight, or settled.
<!-- Allocation: R_S3#C_RECEIVER_ATOMIC_RECEIPTS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | A service the platform's operators run MUST keep a receipt keyed by tenant, session id, and call id.
<!-- Allocation: R_S6#C_RECEIVER_AUTH_REFUSALS | runtime -->
Source: spec/contracts/tool-call.md#a-service-dedupes-by-call-id | An operation MUST decide every refusal before its first write.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A call that has a receipt MUST be answered from the receipt whatever its lease generation.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A remote target outside the operators' control MAY be guarded only by the call id.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A service MUST NOT fence a reconcile.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A service MUST apply the fence check to a request it fences but records no receipt for.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A service MUST fence a close by lease generation as it fences an admission.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A service that runs as more than one instance MUST fence by the highest lease generation any of its instances has recorded for the session.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | A service the platform's operators run MUST refuse a call that has no receipt and whose lease generation is older than the highest it has recorded for that session.
<!-- Allocation: R_S3#C_RECEIVER_FENCE | runtime -->
Source: spec/contracts/tool-call.md#a-service-fences-a-superseded-owner | The refusal of a superseded owner MUST be distinct from any other error.
<!-- Allocation: R_S3#A_CALL_GUARDS | runtime -->
Source: spec/core/shared-framework.md#caller-authentication-and-call-guards | The call guards MUST fence by lease generation.
<!-- Allocation: R_S3#A_CALL_GUARDS | runtime -->
Source: spec/core/shared-framework.md#caller-authentication-and-call-guards | The framework MUST provide call guards that dedupe by call id.

---

ID: S
Title: Session transitions and event recovery
Prerequisites: CO, D, DA, SF, SIM
Deliverable: Ordered inbox/state/cursor commits, attempts, quarantine and retry/skip controls. Interface boundary: Opaque ordered inbox apply, session/cursor/state atomicity, dedupe, ambiguous commit recovery, lease/authority fences, attempts, quarantine and wakeups.
Acceptance: Exactly-once state effects and order survive real recovery; lease changes reject stale completions; every blocked state has an exit. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: CO | inferred | Requires ownership generation, assignment and routing interfaces for integration.
Dependency: D | inferred | Requires immutable loaded-definition and grant-check interfaces for integration.
Dependency: DA | inferred | Requires real durable transactions and recovery interfaces for integration.
Dependency: SF | inferred | Requires sealing, key lifecycle and compatible durable codecs for integration.
Dependency: SIM | inferred | Requires explicit time, transaction and injectable-fault interfaces for integration.
<!-- Allocation: S_S6#C_NOTIFICATION_EXECUTION | runtime -->
Source: constitution.md#ii-event-driven-never-polling | A component MUST NOT poll a dependency for a change that the dependency can notify it of.
<!-- Allocation: S_S6#C_NOTIFICATION_EXECUTION | runtime -->
Source: constitution.md#ii-event-driven-never-polling | A session MUST be woken by an event, a timer, or a deadline addressed to it, rather than by a periodic check for work.
<!-- Allocation: S_S3#C_DISPOSABLE_MEMORY | runtime -->
Source: constitution.md#p5-durable-state-disposable-memory | Every part of a session's state MUST be kept in the durable store.
<!-- Allocation: S_S3#C_DISPOSABLE_MEMORY | runtime -->
Source: constitution.md#p5-durable-state-disposable-memory | Everything a node holds in memory MUST be a cache that can be dropped and rebuilt from the durable store at any moment.
<!-- Allocation: S_S3#C_FAILURE_TABLE_IDENTITY | runtime -->
Source: constitution.md#p6-one-handling-per-failure-every-side-effect-redoable | Every failure class MUST have exactly one defined handling and one defined state the session ends in.
<!-- Allocation: S_S3#C_FAILURE_TABLE_IDENTITY | runtime -->
Source: constitution.md#p6-one-handling-per-failure-every-side-effect-redoable | Every side effect MUST carry an idempotency key so that redoing it is safe.
<!-- Allocation: S_S3#A_EVENT_DEDUPE | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-dedupe-id | A publish or send id MUST be deduplicated per publisher principal and destination, so that the same id reused toward another destination is a fresh event.
<!-- Allocation: S_S3#A_EVENT_DEDUPE | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-dedupe-id | A publisher that publishes from an ordered log SHOULD derive each event's id from its position in the log, so that a retry is a duplicate by construction.
<!-- Allocation: S_S3#A_EVENT_DEDUPE | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-dedupe-id | Every event MUST carry an id that dedupes retries.
<!-- Allocation: S_S3#A_EVENT_DEDUPE | runtime -->
Source: spec/contracts/event-envelope.md#every-event-has-a-dedupe-id | The platform MUST apply an event with an id it has already applied for the same destination at most once.
<!-- Allocation: S_S6#A_SESSION_ATTEMPT_REDRIVE | runtime -->
Source: spec/core/durability-and-deployment.md#work-resumes-from-committed-state | An interrupted model attempt MUST be run again as a new attempt with an attempt id of its own.
<!-- Allocation: S_S6#A_SESSION_ATTEMPT_REDRIVE | runtime -->
Source: spec/core/durability-and-deployment.md#work-resumes-from-committed-state | An interrupted tool call MUST follow its semantics on recovery.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/events-and-subscriptions.md#ordering-and-state-are-kept-in-one-store | The platform MUST NOT rely on an external queue for per-session ordering or for exactly-once application.
<!-- Allocation: S_S3#C_STORE_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-store-conflict-or-outage | A durable-store error met while checking the partition's fence MUST be handled as a store failure to retry rather than as a superseded fence.
<!-- Allocation: S_S3#C_STORE_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-store-conflict-or-outage | A durable-store transaction that conflicts or fails MUST be retried with backoff.
<!-- Allocation: S_S3#C_STORE_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#a-store-conflict-or-outage | A failed store transaction MUST leave the session's state unchanged.
<!-- Allocation: S_S6#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | A configuration change applied to a session quarantined for an event that keeps failing MUST reset the event's attempt count and re-drive the event.
<!-- Allocation: S_S6#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | A pause, stop, or quarantine imposed by an emergency control MUST NOT be lifted by a configuration change.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | A resume of a session quarantined for an event that keeps failing MUST either retry the event with a fresh attempt count or skip it with an audit event.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | A session whose event was quarantined MUST end blocked with the reason.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | An event that fails the configured number of attempts MUST be quarantined so that it cannot stall its partition.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#an-event-that-fails-repeatedly | An owner that re-drives a session's first unapplied event MUST commit the attempt count for that event before it runs the attempt.
<!-- Allocation: S_S3#C_STORE_FAILURE_RECOVERY | runtime -->
Source: spec/core/failure-handling.md#an-owner-that-crashes-or-moves | The new owner of a partition MUST resume each session from its committed cursors.
<!-- Allocation: S_S6#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#every-blocked-or-quarantined-state-has-an-exit | Alarming an operator MUST NOT be the only handling of any failure class.
<!-- Allocation: S_S6#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#every-blocked-or-quarantined-state-has-an-exit | Every blocked or quarantined state MUST have an exit transition that an adapter can trigger through an inbox event or the resume control.
<!-- Allocation: S_S3#C_FAILURE_TABLE_IDENTITY | runtime -->
Source: spec/core/failure-handling.md#every-retry-has-a-budget | Every retry MUST be bounded by a budget.
<!-- Allocation: S_S3#C_FAILURE_TABLE_IDENTITY | runtime -->
Source: spec/core/failure-handling.md#every-retry-has-a-budget | Exhausting a retry budget MUST end in a defined blocked state that carries the reason.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#quarantine-and-blocks-are-events | Every quarantine MUST be published as an event with its reason.
<!-- Allocation: S_S3#C_EVENT_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#quarantine-and-blocks-are-events | Every transition to a blocked state MUST be published as an event with its reason.
<!-- Allocation: S_S6#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A frozen range whose integrity check fails when it is loaded MUST be treated as state that cannot be decoded.
<!-- Allocation: S_S6#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A quarantined frozen session MUST NOT be loaded again before a resume, so that one failed blob raises one event.
<!-- Allocation: S_S3#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A quarantined session MUST NOT stop its partition from serving its other sessions.
<!-- Allocation: S_S3#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A quarantined session's state MUST NOT be overwritten.
<!-- Allocation: S_S6#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A queued message or a timer fire whose stored form cannot be decoded or unsealed MUST be set aside with an event that carries the message's and the reader's format identities rather than stall its queue.
<!-- Allocation: S_S3#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A resume of a session quarantined for undecodable state MUST retry the decode and lift the quarantine only if the decode succeeds.
<!-- Allocation: S_S3#C_DECODE_QUARANTINE | runtime -->
Source: spec/core/failure-handling.md#state-that-cannot-be-decoded | A session whose state cannot be decoded MUST be quarantined.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-commit-conflicts-only-on-the-fence | A partition's commit of state, cursors, and trims MUST be rejected for a conflict only when the partition's fence has changed.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-reads-at-the-woken-version | A partition MUST NOT read its doorbell or inboxes with a stale read that can miss the append that woke it.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-reads-at-the-woken-version | A partition MUST read its doorbell and its dirty inboxes in one read-only transaction pinned at the version its watch woke for.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-sleeps-on-its-doorbell-and-its-deadlines | A doorbell watch MUST be kept alive or renewed within the transport's idle bound.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-sleeps-on-its-doorbell-and-its-deadlines | A partition MUST read the wall clock again on every wake.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-sleeps-on-its-doorbell-and-its-deadlines | A partition MUST sleep on the earlier of a watch on its doorbell and its next deadline.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/sessions-and-partitions.md#a-partition-sleeps-on-its-doorbell-and-its-deadlines | A watch on a newly created partition's doorbell MUST block until the first append rather than return at once because nothing has been written.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-id-encodes-no-location | A keyed instance's session id MUST be derived deterministically, beginning with the tenant, from its template, its instancing form, and its publisher-scoped instance key.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-id-encodes-no-location | A session id MUST NOT encode where the session runs.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-id-encodes-no-location | A session id MUST be unique across tenants.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-id-encodes-no-location | The instance key of a template keyed by a publisher MUST be the pair of the publisher's authenticated principal and the key.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A commit whose outcome is unknown MUST be resolved by re-reading the committed state under the fence before any redo.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A partition MUST apply each session's inbox in order.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A partition MUST commit a session's new state and its cursor in one transaction.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A publisher MUST append an event to the session's inbox and the session id to the partition's doorbell in one transaction.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A session MUST NOT compare topic positions across lineage epochs.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A session MUST skip a topic delivery whose position is at or below the last position it applied or dropped from that topic within the same lineage epoch.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | A slow session MUST delay only itself.
<!-- Allocation: S_S3#A_SESSION_ATOMIC_APPLY | runtime -->
Source: spec/core/sessions-and-partitions.md#each-event-applies-exactly-once-in-order | Each inbox event MUST apply exactly once relative to the session's state.
<!-- Allocation: S_S6#A_SESSION_ATTEMPT_REDRIVE | runtime -->
Source: spec/core/sessions-and-partitions.md#long-work-commits-as-state-transitions | A crash during a step MUST leave the durable state at the step in progress.
<!-- Allocation: S_S6#A_SESSION_ATTEMPT_REDRIVE | runtime -->
Source: spec/core/sessions-and-partitions.md#long-work-commits-as-state-transitions | A model turn or tool run MUST be a task whose result the partition commits as a state transition.
<!-- Allocation: S_S6#A_SESSION_ATTEMPT_REDRIVE | runtime -->
Source: spec/core/sessions-and-partitions.md#long-work-commits-as-state-transitions | The next owner MUST re-drive a step left in progress.
<!-- Allocation: S_S6#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | A close sent for a call abandoned at a session's end MUST carry the end's advanced lease generation.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | A commit by a superseded owner MUST fail.
<!-- Allocation: S_S6#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | A new owner MUST commit a session's lease-generation advance before it issues any recovery dispatch, reconcile, or close for that session.
<!-- Allocation: S_S6#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | A session's lease generation MUST advance, in the same commit as the change, on every change of the party that may act for the session: an owner move, a restore, a pause, a quarantine, a stop or end, and a resume.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | A wake addressed to a partition generation newer than the resident owner's MUST stop the resident owner and be handled by an owner at the new generation, so that the wake is not lost.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | Correctness MUST NOT depend on a superseded owner detecting that it was superseded.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | Every partition commit MUST check the partition's generation.
<!-- Allocation: S_S6#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#ownership-is-fenced-by-generation | The acknowledgement of a session-scoped resume MUST follow the session's commit and carry the session's new lease generation.
<!-- Allocation: S_S3#A_DOORBELL_WATERMARK | runtime -->
Source: spec/core/sessions-and-partitions.md#the-doorbell-advances-to-a-watermark | A doorbell entry MUST be trimmed no earlier than the commit that records the entry's first-read effects.
<!-- Allocation: S_S3#A_DOORBELL_WATERMARK | runtime -->
Source: spec/core/sessions-and-partitions.md#the-doorbell-advances-to-a-watermark | Recovery MUST re-read the doorbell from the watermark.
<!-- Allocation: S_S3#A_DOORBELL_WATERMARK | runtime -->
Source: spec/core/sessions-and-partitions.md#the-doorbell-advances-to-a-watermark | The doorbell cursor MUST advance to the position below which every session listed in the doorbell has applied every event.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-partition-owner-is-the-single-writer | A partition MUST have exactly one owner at a time.
<!-- Allocation: S_S3#A_SINGLE_WRITER_FENCE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-partition-owner-is-the-single-writer | Every write to a session's state MUST be made by the owner of the session's partition.
<!-- Allocation: S_S3#A_SESSION_WATCH_WAKE | runtime -->
Source: spec/core/shared-framework.md#reads-at-the-woken-version | The framework MUST provide a read pinned at the version a watch woke for, so that the reader always sees the append that woke it.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/shared-framework.md#self-describing-ids | A decoder MUST report an unregistered code as unregistered rather than guess.
<!-- Allocation: S_S3#A_SESSION_LOCATION_IDS | runtime -->
Source: spec/core/shared-framework.md#self-describing-ids | A parser MUST accept only an id's canonical form and refuse every other spelling.
<!-- Allocation: S_S6#C_NOTIFICATION_EXECUTION | runtime -->
Source: spec/core/tools-and-dispatch.md#mechanisms-are-not-tools | The platform MUST NOT require a model turn to report status, drain an inbox, re-arm a loop, send a heartbeat, or request a compaction.

---

ID: SF
Title: Content sealing and compatible stored forms
Prerequisites: none
Deliverable: Client sealing, key lifecycle and evolvable durable codecs. Interface boundary: Trusted-input key selection, resource envelope sealing, tagged bounded codecs, unknown-field handling, durable compatibility and destruction across copies. The v1 Backup Key Retention exception in implementation.md applies to retained recovery copies; original backup-erasure traceability remains visible without a conformance claim.
Acceptance: Stored tenant content is sealed; old records and unknown tags round-trip; key destruction and trust boundaries are exercised. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
<!-- Allocation: SF_S2#C_TENANT_KEYS | runtime -->
Source: constitution.md#the-tenant-is-the-boundary | Data MUST NOT be written under a root key shared between tenants.
<!-- Allocation: SF_S6#C_ROOT_KEY_DESTRUCTION | runtime -->
Source: constitution.md#the-tenant-is-the-boundary | Destroying a tenant's root key MUST make the tenant's data unreadable everywhere, backups included.
<!-- Allocation: SF_S2#C_TENANT_KEYS | runtime -->
Source: constitution.md#the-tenant-is-the-boundary | Every tenant's keys MUST derive from a root key dedicated to that tenant, whether the tenant's own or one the service holds for it.
<!-- Allocation: SF_S2#C_TENANT_KEYS | runtime -->
Source: constitution.md#v-secure-by-default | A key MUST be chosen only by a trusted input, never by a value read from a store or a payload.
<!-- Allocation: SF_S2#C_TENANT_KEYS | runtime -->
Source: constitution.md#v-secure-by-default | Every durable record of a tenant's content MUST be encrypted at rest.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#a-decoder-reports-every-corrupt-value | A decoder MUST NOT overwrite a value it could not decode.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#a-decoder-reports-every-corrupt-value | A decoder MUST allocate no more than the remaining input can hold.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#a-decoder-reports-every-corrupt-value | A decoder MUST distinguish an absent value, a valid value, and a corrupt value.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#a-decoder-reports-every-corrupt-value | A plaintext value found where a sealed value is due MUST be treated as undecodable rather than read.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys | A backup or point-in-time copy of any store MUST NOT retain key material that still opens after the owning key is destroyed.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys | A long-lived resource's data key MUST be re-wrappable under a new epoch's branch key without a call to the key service.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#a-resource-carries-its-own-wrapped-keys | A stored resource MUST carry its own wrapped keys, so that any node can read it with at most one call to the key service.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#additive-evolution-of-this-contract | A change to this contract MUST be additive with respect to already-written data, or else carry an explicit version increment.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#additive-evolution-of-this-contract | A change to this contract that is not additive with respect to already-written data MUST carry a stated migration path.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | A ciphertext copied to another owner or range MUST fail to decrypt.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | A segment of a blob MUST have authenticated data that binds its owner and its position within the blob.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | A session's stored state MUST carry a header that decodes independently of its body and carries the format identity, the session's lease generation, and its owner identity.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | An inbox entry's authenticated data MUST bind it to its session id and its event id.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | Every envelope MUST carry authenticated data that binds it to its tenant, its owner, and its range.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#authenticated-data-binds-the-envelope | The state header MUST be bound to the body as authenticated data.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A change to a field's bound MUST be a format change.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A decoder MUST preserve a field whose tag it does not recognize through a read and a rewrite.
<!-- Allocation: SF_S2#A_STORED_DEFAULT_UNKNOWN_VARIANT | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A field added to a durable format MUST carry the value a reader that lacks the field falls back to.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A length that is not the minimal encoding of its value MUST be rejected, so that every value has exactly one encoding.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A re-seal of a record under a new binding MUST commit under the record's partition fence, so that the owner's own commits cannot interleave with it.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A record MUST remain readable under the stored format, the encryption domain, and the key generation it was sealed with after any of them changes.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A record at rest MUST remain readable by every later build without being rewritten.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A stored envelope's format identity MUST be independent of the shape of the value it carries, so that a change to the carried value invalidates no stored envelope.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A tag given to a field or a variant MUST NOT be reused for another meaning after the field or variant is removed.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A value that exceeds its bound MUST be rejected as an error rather than truncated.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | A variant MUST be removed only where every record holding it can be held as the fallback.
<!-- Allocation: SF_S2#A_STORED_DEFAULT_UNKNOWN_VARIANT | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | An enumeration in a durable format MUST declare a fallback variant that a reader reads an unknown variant as, or declare that it has none.
<!-- Allocation: SF_S2#A_STORED_DEFAULT_UNKNOWN_VARIANT | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | An enumeration whose variants cannot be held safely MUST declare no fallback, so that an unknown variant fails the decode and the value is held unchanged.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | Every component of a durable format that may gain a field MUST itself be tagged, including the payload of a variant and the stored form of a resource's wrapped keys.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | Every evolvable durable format MUST identify each field by a tag.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | The hash that maps a session id to its partition MUST be part of the durable format, so that changing it is a migration.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/contracts/stored-form.md#every-durable-format-is-self-describing | Two consecutive forms of a durable format MUST both be readable during a rollout.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#keys-derive-from-the-tenant-s-root | A branch key MUST be derived per tenant per epoch under a service-held root, or per resource under a tenant-held root.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#keys-derive-from-the-tenant-s-root | A data key MUST be random per resource.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#keys-derive-from-the-tenant-s-root | An event appended to a session's inbox by a principal other than the session MUST be encrypted under a key the publisher can use without holding the session's data key.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/contracts/stored-form.md#keys-derive-from-the-tenant-s-root | Every stored value of a tenant's content MUST be encrypted under a data key derived from that tenant's root key.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/contracts/stored-form.md#scheduling-fields-are-readable-without-keys | A tenant's content MUST be sealed by the writing client before it is written, so that the durable store never holds it in plaintext.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/contracts/stored-form.md#scheduling-fields-are-readable-without-keys | A timer's name, note, and comparison values MUST be encrypted under the session's keys.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/contracts/stored-form.md#scheduling-fields-are-readable-without-keys | A timer's timing fields MUST be stored unencrypted, so that a partition schedules and recovers without the session's keys.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/contracts/stored-form.md#scheduling-fields-are-readable-without-keys | The values stored unencrypted MUST be only the platform's own operational metadata that this contract enumerates.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#the-tenant-is-in-the-encryption-context | Every envelope's encryption context MUST carry the tenant.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#values-and-blobs-use-authenticated-encryption | A stored value MUST be encrypted under an authenticated mode.
<!-- Allocation: SF_S2#A_ENVELOPE_BINDING | runtime -->
Source: spec/contracts/stored-form.md#values-and-blobs-use-authenticated-encryption | An immutable blob MUST be encrypted under a deterministic authenticated mode, so that a retried write produces the same bytes under the same name.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/encryption-and-blobs.md#a-long-lived-session-re-wraps-per-epoch | A session that outlives an epoch MUST re-wrap its data key under the new epoch's branch key without a call to the key service.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | A copy of key material MUST NOT be kept in a store whose recovery window would leave it readable after its key is destroyed.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | A read of a range that is no longer readable MUST answer not available rather than partial content.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | A session's data key MUST be wrapped only under a key whose every stored copy is destroyed with the session's owner, never under the tenant's branch key alone.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | A transfer of a blob to another owner MUST re-seal it under the new owner's key.
<!-- Allocation: SF_S6#ACTUAL_KEY_DELETION_CONSUMERS | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | Deleting a session MUST destroy, in the same deletion, the keys of every copy of its transcript ranges made for episodes.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | Deletion MUST come from destroying keys.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | Destroying a key MUST be followed by the destruction of every plaintext copy a host holds, within the configured bound.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | Destroying a key MUST take effect in the blob store, caches, and backups.
<!-- Allocation: SF_S5#A_KEY_DELETION | runtime -->
Source: spec/core/encryption-and-blobs.md#deletion-is-key-destruction | The node cache MUST hold only ciphertext, so that destroying a key revokes cached copies within the decrypt cache's window.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A node MUST be able to read a session with at most one call to the key service.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A plaintext key MUST be held only in a per-node cache whose lifetime is the revocation window.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A plaintext value found where a sealed value is due MUST be treated as a corrupt value rather than read.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A re-seal of a record MUST commit under the record's partition fence, so that the owner's own commits cannot interleave with it.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A stage MUST hold a tenant's production data only inside an isolation boundary in which the platform's own deployment pipeline is the only principal with administrative authority over the whole boundary.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A stage that holds production data MUST store every value and blob under the envelope.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A stage whose stores do not apply the envelope MUST refuse a production tenant at start and on every call.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A stage whose stores take one key MUST serve exactly one tenant.
<!-- Allocation: SF_S2#A_STORED_CODEC | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | A stored record that has not been re-sealed MUST stay readable under the format version, the key hierarchy, and the key generation it was sealed with after any of them changes.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | Content stored without the envelope MUST be discarded with its stage rather than migrated.
<!-- Allocation: SF_S6#ACTUAL_KEY_DELETION_CONSUMERS | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | Ending or deleting a session MUST make its unapplied inbox entries unreadable.
<!-- Allocation: SF_S2#A_SEALED_CONTENT_METADATA | runtime -->
Source: spec/core/encryption-and-blobs.md#sessions-are-encrypted-under-an-envelope | Every session's values and blobs MUST be encrypted under the envelope the stored-form contract pins.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-comes-from-a-trusted-list | The label MUST be checked against the node's trusted list of permitted keys.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-comes-from-a-trusted-list | The root key for a session MUST be identified in the session's configuration by a label.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-service-stays-off-the-critical-path | A key-service call that must be waited on MUST be hedged and bounded by a short deadline.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-service-stays-off-the-critical-path | A session's keys MUST resolve when its doorbell wakes it, before a turn needs them.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-service-stays-off-the-critical-path | Resumes after a failover MUST be paced, so that a burst of session resumes does not exceed the key service's request quota.
<!-- Allocation: SF_S3#A_KEY_TRUST_REFRESH | runtime -->
Source: spec/core/encryption-and-blobs.md#the-key-service-stays-off-the-critical-path | The decrypt cache MUST refresh a key ahead of its expiry, so that a revocation still takes effect within the window.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | A plaintext data key MUST NOT be stored at rest.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST provide a deterministic mode for immutable blobs.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST provide a key per owner whose destruction makes that owner's data unreadable.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST provide a key-service client built for the key service's limits.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST realize the key hierarchy the stored-form contract pins.
<!-- Allocation: SF_S6#OFF_HOST_KEY_RELEASE | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST release exactly one owner's data key to an authorized off-host caller without exposing another's.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The envelope-encryption component MUST support a root dedicated to each tenant, whether the tenant's own or one the service holds for it.
<!-- Allocation: SF_S2#A_WRAPPED_KEY_HIERARCHY | runtime -->
Source: spec/core/shared-framework.md#envelope-encryption | The framework MUST provide one envelope-encryption implementation, generic over the resource and its authenticated data.
<!-- Allocation: SF_S1#A_STORED_EVOLUTION | governance -->
Source: spec/core/shared-framework.md#self-describing-ids | Codes MUST come from one append-only registry in which a code never changes and is never reused.
<!-- Allocation: SF_S2#B_PORTABLE_ID_CONTRACT | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#ids-are-portable | A durable id MUST NOT be reissued to a different entity, across a restore included.
<!-- Allocation: SF_S2#B_PORTABLE_ID_CONTRACT | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#ids-are-portable | A durable id MUST NOT encode where it runs.
<!-- Allocation: SF_S2#B_PORTABLE_ID_CONTRACT | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#ids-are-portable | A durable id MUST be unique across tenants.
<!-- Allocation: SF_S2#B_PORTABLE_ID_CONTRACT | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#ids-are-portable | A durable id MUST carry the kind of entity it identifies and the version of its format.
<!-- Allocation: SF_S2#B_PORTABLE_ID_CONTRACT | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#ids-are-portable | A durable id MUST have exactly one canonical text form.
<!-- Allocation: SF_S6#B_TENANT_ROOT_ADMIN | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#root-key-administration-is-separated | A role that administers a key MUST NOT be able to use it.
<!-- Allocation: SF_S6#B_TENANT_ROOT_ADMIN | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#root-key-administration-is-separated | A tenant's root key MUST grant nothing to a principal that its own policy does not identify.
<!-- Allocation: SF_S6#B_TENANT_ROOT_ADMIN | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#root-key-administration-is-separated | Any attempt to disable a tenant's root key or to schedule its destruction MUST raise an alarm.
<!-- Allocation: SF_S6#B_TENANT_ROOT_ADMIN | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#root-key-administration-is-separated | Destruction of a root key MUST take effect only after a configured waiting window.
<!-- Allocation: SF_S6#B_TENANT_ROOT_ADMIN | runtime -->
Source: spec/core/tenancy-and-confidentiality.md#root-key-administration-is-separated | The identity that deploys the platform's infrastructure MUST NOT be able to use or administer a tenant's root key.

---

ID: SIM
Title: Deterministic environment and store model
Prerequisites: none
Deliverable: Explicit time, snapshots, transactions and injectable dependency faults. Interface boundary: Explicit monotonic time and per-node wall time, entropy, deterministic ordering, sleep/deadline cancellation, chosen faults and replay inputs. Simulation uses Bach and Bolero. Simulator contracts are qualified against the real dependencies through the specified probes.
Acceptance: Generated histories agree with independent models and replay; the logical store fault contract is executable. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: constitution.md#iv-deterministic-by-construction | A component MUST NOT iterate a collection in an order that varies between runs of the same inputs.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: constitution.md#iv-deterministic-by-construction | A component MUST take its entropy from the simulation's entropy source rather than from an ambient source of randomness.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: constitution.md#iv-deterministic-by-construction | A simulator MUST be verified by a contract test that runs the same expectations against the simulator and against the real dependency.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: constitution.md#iv-deterministic-by-construction | Every component MUST run under the deterministic simulation framework with every external dependency replaced by a simulator.
<!-- Allocation: SIM_S6#C_SIM_FAILURE_CONTRACT | runtime -->
Source: constitution.md#iv-deterministic-by-construction | Every external dependency MUST have a simulator that injects the failures the real dependency exhibits.
<!-- Allocation: SIM_S6#C_REMOTE_DEADLINES | runtime -->
Source: spec/core/failure-handling.md#every-unit-of-work-has-a-deadline | A deadline enforced by a remote host MUST NOT end the work before the deadline in the issuer's time.
<!-- Allocation: SIM_S6#C_REMOTE_DEADLINES | runtime -->
Source: spec/core/failure-handling.md#every-unit-of-work-has-a-deadline | A deadline enforced by a remote host MUST end the work within the configured bound after the deadline.
<!-- Allocation: SIM_S2#A_ENV_DEADLINES | runtime -->
Source: spec/core/shared-framework.md#deadlines-on-the-environment | The framework MUST provide a deadline queue with insert, cancel, and next-deadline.
<!-- Allocation: SIM_S2#A_ENV_DEADLINES | runtime -->
Source: spec/core/shared-framework.md#deadlines-on-the-environment | The framework MUST provide a sleep to an absolute instant.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: spec/core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator | A contract test MUST fail when the simulator diverges from the real dependency.
<!-- Allocation: SIM_S6#C_SIM_FAILURE_CONTRACT | runtime -->
Source: spec/core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator | Each dependency's simulator MUST inject latency and its tail, throttles, errors, timeouts, dropped connections, duplicates, reordering, and partial failures.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: spec/core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator | Each simulator MUST have a contract test that runs the same expectations against the simulator and, through a probe, against the real dependency.
<!-- Allocation: SIM_S6#C_SIM_FAILURE_CONTRACT | runtime -->
Source: spec/core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator | The durable store's simulator MUST be able to fail one chosen transaction on demand, so that a scenario exercises the retry of a fence check.
<!-- Allocation: SIM_S6#C_ALL_COMPONENT_SIM_PARITY | runtime -->
Source: spec/core/simulation-and-conformance.md#everything-runs-under-simulation | A component MUST NOT depend on a wall clock, a thread schedule, or a collection order that varies between runs of the same inputs.
<!-- Allocation: SIM_S2#C_SIM_DETERMINISM_CONTRACT | runtime -->
Source: spec/core/simulation-and-conformance.md#time-is-a-dependency | A clock-fault scenario MUST be expressible, so that the timer discipline is tested.
<!-- Allocation: SIM_S2#C_SIM_DETERMINISM_CONTRACT | runtime -->
Source: spec/core/simulation-and-conformance.md#time-is-a-dependency | The simulation MUST provide a per-node wall clock whose offsets, drift, and steps can be injected.

---

ID: SR
Title: Persistent stage resources
Prerequisites: none
Deliverable: Stage-owned blob, key, signing, restore, stable-name and access-log resources. Interface boundary: Cluster-independent resource identity/lifetime, one-tenant bootstrap, node isolation, peer trust, readiness and declared replacement/failure boundary.
Acceptance: Resource lifecycle tests show cluster replacement cannot remove required stage state; missing resources block cluster creation. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-deployment-completes-on-readiness | A deployment MUST count a node as deployed only after the node's readiness check passes.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-deployment-completes-on-readiness | A node whose readiness check does not pass within the configured bound MUST fail its deployment rather than complete it.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A change in a node's reported reachability of a dependency MUST be published as an event.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A cluster MUST be reported ready only when every partition is assigned and a canary session has completed a round trip within the configured bound.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A node MUST report its own health to the deployment control plane rather than rely on a check that a crash-looping process passes.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A node MUST withdraw readiness before it drains.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A node's readiness MUST reflect the node's own ability to reach each dependency a request needs, so that traffic is routed away from that node alone.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | A reachability probe MUST exercise the dependency's request path rather than only open a connection, so that an authorization or credential failure is detected.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | The deployment control plane MUST treat a cluster whose health reports have stopped for longer than the configured bound as unhealthy.
<!-- Allocation: SR_S5#A_NODE_READINESS | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-reports-its-own-health | Traffic MUST NOT be moved to a cluster that is not reported ready and healthy.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | A change to a platform node outside the deployment control plane MUST be prevented by permission rather than detected after the fact.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | A component MUST bind no network listener beyond those its design declares by port.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | A node image MUST NOT include an agent that accepts configuration or commands from outside the deployment control plane.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | A platform node MUST accept no command and no interactive session from any principal.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | A platform node MUST run only the platform's own process.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | An administrative listener MUST bind only the local interface unless a declared peer calls it.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | Code a tenant supplies MUST NOT be able to reach the host's metadata endpoint or the node's own credentials.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-runs-only-the-platform-s-process | The platform's process MUST run unprivileged and isolated from the host's own process, network, and filesystem namespaces.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#a-node-starts-only-on-a-checked-configuration | A node MUST refuse to start on a configuration that fails its schema check, carries a field it does not know, selects an unauthenticated transport, or relies on a default for a network-facing setting.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#least-privilege-from-the-first-release | A component MUST hold only the permissions that its specified behavior needs.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/core/durability-and-deployment.md#stage-resources-outlive-every-cluster | A blob's storage location, an owner id, and a published name MUST NOT encode the cluster, node, or partition that wrote them.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/core/durability-and-deployment.md#stage-resources-outlive-every-cluster | A cluster MUST NOT be created in a stage that lacks any resource its sessions depend on.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/core/durability-and-deployment.md#stage-resources-outlive-every-cluster | The blob store, the tenant root keys, the signing roots, the restore source, and the frontend's stable name MUST survive the replacement or loss of any cluster.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/core/durability-and-deployment.md#stage-resources-outlive-every-cluster | The frontend's access log MUST survive the deletion of any deployment that declares it.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type | A platform cluster MUST NOT share a cluster with another workload, so that another workload cannot deprive sessions of resources.
<!-- Allocation: SR_S5#A_NODE_BOUNDARY | runtime -->
Source: spec/core/durability-and-deployment.md#the-platform-deploys-as-its-own-cluster-type | What a node runs MUST be pinned by digest.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/decisions/tenancy-scope/one-tenant.md#the-first-tenant-is-bootstrapped | The first tenant MUST be bootstrapped with a stage's first cluster rather than configured.
<!-- Allocation: SR_S2#A_STAGE_RESOURCES | runtime -->
Source: spec/decisions/tenancy-scope/one-tenant.md#the-first-tenant-is-bootstrapped | The platform MUST NOT provide a path to register a tenant.

---

ID: T
Title: Timer schedules and delivery
Prerequisites: S, SIM
Deliverable: One-shot and recurring schedules, deadlines, occurrence identity and atomic delivery. Interface boundary: Owner-clock schedules and occurrence identity, injected clock faults, deadline reconstruction, idempotent edits, guards, holds, budgets and exactly-once fenced fire.
Acceptance: Actual timer recovery honors owner-clock discipline, missed occurrences, bounds and lifecycle suppression without replay. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
Dependency: SIM | inferred | Requires explicit time, transaction and injectable-fault interfaces for integration.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire MUST append an event to the session's inbox, append the session to the partition's doorbell, and advance the timer, in one commit under the partition's fence.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire MUST carry its timer's priority, so that it interrupts or waits like any notification.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire held by the wake budget or by a hold MUST NOT be recorded as a skip.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire whose timer was cancelled, re-armed, or replaced before the session applies it MUST be dropped.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire's id MUST identify the timer, the setting of the timer that produced it, and the occurrence.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A fire's priority MUST be the session's base priority unless the timer asks for a lower one.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A later passing occurrence MUST merge into the waiting fire's count.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A partition MUST NOT fire a timer of a session that is paused, blocked, or quarantined.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A retried append of a fire MUST be dropped as a duplicate.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A timer MUST hold at most one undelivered fire.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A timer MUST record the access scope of the turn that set it.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A timer's fires MUST carry that scope.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | A turn a fire starts MUST identify the fire as its cause.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | Every occurrence MUST count toward a timer's count, so that the end deadline is the due time of the count-th occurrence.
<!-- Allocation: T_S5#A_TIMER_ATOMIC_FIRE | runtime -->
Source: spec/core/timers.md#a-fire-is-exactly-once | Occurrences held while a session was paused, blocked, or quarantined MUST follow the missed-occurrence rule when the hold clears, with the guard evaluated.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A fire that waited MUST be checked against its guard again when it is applied.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A guard condition MUST be one of events held for delivery on given topics, no event on given topics since the timer's last evaluated occurrence, which is a position in the session's commit order, a slot key's value or a write to it, or one decision hook from the loaded definition.
<!-- Allocation: T_S6#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A local guard condition MUST be checked before any decision hook, so that a false guard causes no model call, no session load, and no unfreezing of a frozen session.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A once timer with a quiet guard whose event arrives MUST end as satisfied, with the satisfying event annotated.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A one-shot timer whose guard is false MUST end or wait rather than wake the model.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A recurring timer whose guard is false MUST skip the occurrence.
<!-- Allocation: T_S5#A_TIMER_GUARDS | runtime -->
Source: spec/core/timers.md#a-guard-prevents-a-model-wake-when-there-is-no-work | A timer MAY carry up to the configured number of guard conditions, each of which has to hold for a fire to wake the model.
<!-- Allocation: T_S5#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#a-partition-detects-a-faulty-clock | A wall-clock step of more than the configured bound MUST be an event, detected within one doorbell renewal.
<!-- Allocation: T_S5#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#a-partition-detects-a-faulty-clock | An owner whose clock differs from the store's commit timestamps by more than the configured bound MUST hold its timer fires and alert.
<!-- Allocation: T_S6#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A changed definition schedule whose timing is unchanged MUST keep its count and its consecutive-empty-wake count.
<!-- Allocation: T_S6#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A definition's schedule MUST reach a non-resident session without waking it.
<!-- Allocation: T_S6#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A parent MUST be able to set timers on its sub-sessions.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A session MUST NOT cancel, re-arm, or replace its definition's schedules.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A session MUST be able to set timers on itself.
<!-- Allocation: T_S6#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A session's timers MUST end with the session.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A timer MUST NOT be set across tenants.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A timer set by another principal MUST be owned and cancelled by its setter.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A timer set by another principal MUST count against a per-owner limit.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | A timer set on a session by another principal MUST require a grant, pass the control-operation hook, and arrive through the target's inbox, so that the target's partition is the only writer of its timers.
<!-- Allocation: T_S5#A_TIMER_OWNERS | runtime -->
Source: spec/core/timers.md#a-session-owns-its-own-timers | The note of a timer another principal set MUST pass the notification-arrival deciders when its fire is first considered.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | A timer MUST carry a note within the configured size, returned with each fire.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | A timer name MUST follow the configured grammar.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | A timer set without a name MUST derive one from its schedule, note, guard, and end condition, and not from its flex or priority.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | A timer's name MUST be unique within its session.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | Cancel MUST remove a timer or a name prefix.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | List MUST show every timer with its owner and counts.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | Re-arm MUST move a timer's next occurrence.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | Setting a timer identical to an existing one MUST change nothing and drop no fire.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | Setting a timer that differs from an existing one of the same name MUST replace it and report the previous next fire time.
<!-- Allocation: T_S5#A_TIMER_NAMES_MUTATION | runtime -->
Source: spec/core/timers.md#a-timer-s-name-is-unique-within-its-session | The prefixes reserved for definition schedules and derived names MUST be refused for other timers.
<!-- Allocation: T_S6#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A configuration change that lowers a timer limit below a session's current count MUST NOT delete existing timers.
<!-- Allocation: T_S6#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A configuration change that lowers a timer limit below a session's current count MUST refuse new timers.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A run that carries any event that is not a timer fire MUST NOT be held by the cap.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A session MUST hold no more than the configured number of its own timers.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A timer set in a turn that a timer caused MUST inherit that timer's consecutive empty-wake count.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | A wake MUST count as acting only when the session makes a side-effecting tool call, a send, or a publish.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | After consecutive empty wakes, a recurring timer's interval MUST lengthen up to the configured bound.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | An unguarded recurring timer MUST NOT repeat more often than its configured minimum interval.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | The window's reset MUST be a durable deadline at which a held fire wakes the session.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | Timer-caused empty wakes MUST be capped per session at the model-request hook, counted over the configured window that opens at the session's first counted wake.
<!-- Allocation: T_S5#A_TIMER_BUDGET_BACKOFF | runtime -->
Source: spec/core/timers.md#empty-wakes-are-bounded | When the cap holds a run, the session MUST stay idle rather than blocked, with no model call made and the fire waiting in its inbox.
<!-- Allocation: T_S5#A_TIMER_OBSERVATION | runtime -->
Source: spec/core/timers.md#every-timer-event-is-observable | Every set, rejection, guard check, skip, fire, delivery, drop, outcome, and end MUST be an event.
<!-- Allocation: T_S5#A_TIMER_OBSERVATION | runtime -->
Source: spec/core/timers.md#every-timer-event-is-observable | Timer metrics MUST split only by owner kind, schedule kind, condition kind, delivery, residency, and reason.
<!-- Allocation: T_S5#A_TIMER_FLEX_MISSED | runtime -->
Source: spec/core/timers.md#flex-and-missed-occurrences | A definition's schedules MUST spread their fires over their flex by a hash of the session.
<!-- Allocation: T_S5#A_TIMER_FLEX_MISSED | runtime -->
Source: spec/core/timers.md#flex-and-missed-occurrences | A recurring timer's missed occurrences MUST collapse into one fire that carries their count.
<!-- Allocation: T_S5#A_TIMER_FLEX_MISSED | runtime -->
Source: spec/core/timers.md#flex-and-missed-occurrences | A timer MAY carry a flex that bounds how late an occurrence may be delivered so that it shares a wake.
<!-- Allocation: T_S5#A_TIMER_FLEX_MISSED | runtime -->
Source: spec/core/timers.md#flex-and-missed-occurrences | An occurrence missed during an outage MUST be delivered late unless a stale limit drops it.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A calendar occurrence at a local time that does not exist MUST move forward by the gap.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A calendar occurrence at a local time that occurs twice MUST take the first.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A calendar schedule's shortest gap over the configured lead time MUST meet the minimum interval for its guard kind.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A due time MUST be stored as an absolute instant from the partition owner's wall clock.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A recurring timer MUST compute its next occurrence from its anchor or its last due time, so that a late fire does not drift it and a clock step cannot replay one.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A timer MUST NOT fire before its due time by the owner's clock.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A timer MUST be either a one-shot at an absolute time or after a duration, or recurring at an interval from an anchor or on a calendar schedule in a time zone identified by name.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A timer whose schedule fails that check MUST be refused.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | A timer's first occurrence MUST lie within the configured lead time.
<!-- Allocation: T_S5#A_TIMER_SCHEDULE | runtime -->
Source: spec/core/timers.md#once-or-recurring | An interval schedule MUST measure elapsed time.
<!-- Allocation: T_S5#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#one-deadline-set-per-partition | A new owner MUST rebuild the partition's deadlines from one consistent read of its durable state.
<!-- Allocation: T_S5#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#one-deadline-set-per-partition | A partition MUST track the earliest durable deadline across its sessions' timers and the platform's own deadlines.
<!-- Allocation: T_S5#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#one-deadline-set-per-partition | A process-scoped timer that stops when its process stops MUST NOT be durable.
<!-- Allocation: T_S6#A_TIMER_CLOCK_DEADLINES | runtime -->
Source: spec/core/timers.md#timers-add-no-keys-of-their-own | A session's timers MUST stay schedulable, without durable-store keys of their own, while the session is evicted or frozen.

---

ID: X
Title: Transcript, memory and freeze/thaw
Prerequisites: B, H, S
Deliverable: Bounded transcript head, frozen ranges, pins, memory references and reconstruction. Interface boundary: Append-only transcript, transformed scoped views, compaction cut/tail atomic swap, blob pins, bounded residency and reconstructible caches.
Acceptance: Real stored ranges reconstruct safely; concurrent append/open work prevents freeze; transformed content and scopes persist. Detailed acceptance and exact per-obligation allocation are in the linked phase specification's milestone gates; conditional permissions and change-process clauses retain their original guards.
Dependency: B | inferred | Requires owned blob, pin and reclamation interfaces for integration.
Dependency: H | inferred | Requires ordered hook decisions and atomic gated-transition interfaces for integration.
Dependency: S | inferred | Requires fenced session state, inbox and attempt transitions for integration.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/contracts/stored-form.md#frozen-ranges-are-compressed-before-encryption | A compressed range MUST be written in a format whose decoder is a published standard, so that the encoder can change without a format change.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/contracts/stored-form.md#frozen-ranges-are-compressed-before-encryption | A frozen transcript range MUST be compressed before it is encrypted.
<!-- Allocation: X_S5#A_AGENT_MEMORY | runtime -->
Source: spec/core/sessions-and-partitions.md#a-memory-belongs-to-an-agent-identity | A memory MUST be keyed by agent identity rather than by session.
<!-- Allocation: X_S5#A_AGENT_MEMORY | runtime -->
Source: spec/core/sessions-and-partitions.md#a-memory-belongs-to-an-agent-identity | An agent's memories MUST persist across every session of that agent identity.
<!-- Allocation: X_S5#A_AGENT_MEMORY | runtime -->
Source: spec/core/sessions-and-partitions.md#a-memory-belongs-to-an-agent-identity | The loss of one host MUST NOT destroy a memory.
<!-- Allocation: X_S3#A_SESSION_TRANSCRIPT_LIFETIME | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-is-one-continuous-transcript | A session MUST have one continuous transcript for its lifetime.
<!-- Allocation: X_S3#A_SESSION_TRANSCRIPT_LIFETIME | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-is-one-continuous-transcript | The platform MUST NOT bound a session's lifetime.
<!-- Allocation: X_S3#A_SESSION_TRANSCRIPT_LIFETIME | runtime -->
Source: spec/core/sessions-and-partitions.md#a-session-is-one-continuous-transcript | Which root sessions exist MUST be decided by their owner, through the mechanism of the adopted session-management outcome, rather than by the core.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | A compaction MUST switch the session to the compacted transcript in one commit, however many commits wrote it.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | A frozen session MUST occupy no durable-store key of its own.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | A queue in the durable store MUST be trimmed once its entries are applied and frozen.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | A transcript log that no session points to MUST be deleted.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | A turn's commit MUST append the turn's new transcript bytes rather than rewrite the transcript.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | An active session MUST occupy no more durable-store keys than the configured per-session key count.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | The durable store's working set MUST grow with the number of active sessions rather than with the total number of sessions.
<!-- Allocation: X_S5#A_TRANSCRIPT_WORKING_SET | runtime -->
Source: spec/core/sessions-and-partitions.md#cost-follows-active-sessions | The transcript head held in the durable store MUST be bounded by the configured size.
<!-- Allocation: X_S5#A_SESSION_FREEZE_CACHE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-resident-form-is-a-cache | A partition MUST load a session when the session has work.
<!-- Allocation: X_S5#A_SESSION_FREEZE_CACHE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-resident-form-is-a-cache | A session MUST freeze whole only while it has no work in progress, no tool call open, and its inbox applied.
<!-- Allocation: X_S5#A_SESSION_FREEZE_CACHE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-resident-form-is-a-cache | A whole-session freeze MUST fail when a publish reaches the session's inbox during the freeze, so that the publish is applied rather than lost.
<!-- Allocation: X_S5#A_SESSION_FREEZE_CACHE | runtime -->
Source: spec/core/sessions-and-partitions.md#the-resident-form-is-a-cache | Dropping a session's resident form MUST lose nothing.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#every-tier-above-the-log-is-a-cache | Dropping any tier of the transcript cache MUST lose nothing.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#every-tier-above-the-log-is-a-cache | Plaintext derived from a tenant's content MUST NOT be written to a cache tier that outlives the process.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#every-tier-above-the-log-is-a-cache | Plaintext derived from a tenant's content and held in a node's memory MUST expire within the decrypt cache's window.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#frozen-history-is-recoverable | A session's history MUST be recoverable from the blob store and the key service alone.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | A reasoning block that held a secret MUST be kept in the durable log only as the redaction marker.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | A rollback MUST remove a failed attempt from the model's view only where the tool's declared semantics allow it.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | A rollback MUST remove failed attempts from the model's view only.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | The durable log MUST keep a rolled-back attempt with the spans a blocking decider marked redacted.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | The durable log MUST keep rolled-back attempts for audit and observation.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | The transcript and the durable log MUST record content as it took effect after transforms rather than as it arrived.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#rollback-removes-attempts-from-the-view-only | Where a provider binds reasoning to the history it was produced in, a rollback MUST drop the bound reasoning rather than send it with an edited history.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | A frozen range MUST be encoded, compressed, and encrypted before it is written.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | A transcript range that stabilizes, by passing a compaction boundary or a size threshold, MUST be frozen into the blob store.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | An idle session MUST be able to freeze entirely, leaving a pointer record.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | Freezing triggers MUST be per-session configuration.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | The duplicate guard MUST be carried in the frozen form and restored with the session.
<!-- Allocation: X_S5#C_TRANSCRIPT_FREEZE_CACHE | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#stable-ranges-move-to-the-blob-store | The durable log MUST keep only a pointer to a frozen range: the blob name, the range, and its sizes.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-log-is-never-edited | The durable log MUST NOT be edited in place.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-log-is-never-edited | The model's view of a transcript MUST be derived from the durable log.
<!-- Allocation: X_S4#C_COMPACTION_SWAP_HISTORY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-swap-preserves-history | Swapping in a compaction MUST replace everything before the cut with the summary and keep the tail verbatim.
<!-- Allocation: X_S4#C_COMPACTION_SWAP_HISTORY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-swap-preserves-history | The history before the cut MUST be frozen into the blob store rather than deleted.
<!-- Allocation: X_S4#C_COMPACTION_SWAP_HISTORY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-swap-preserves-history | The swap MUST apply pending changes to the cached prefix, such as tool and system prompt changes, in the same rewrite.
<!-- Allocation: X_S4#C_COMPACTION_SWAP_HISTORY | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#the-swap-preserves-history | The switch to a compacted transcript MUST take effect in one commit, however many commits wrote the compacted transcript.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#tool-results-are-bounded-in-the-view | The bounded view of a tool result MUST be enforced before any model request, so that no single result can exceed the context window.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#trimming-leaves-a-resolvable-placeholder | A stale tool result trimmed from the view MUST be replaced by a placeholder the model can resolve by the call id.
<!-- Allocation: X_S4#C_TRANSCRIPT_VIEW_ROLLBACK | runtime -->
Source: spec/decisions/execution/direct-context-compaction-and-cache.md#trimming-leaves-a-resolvable-placeholder | Trimming MUST evict the oldest stale results first.
<!-- Allocation: X_S4#C_COMPACTION_SWAP_HISTORY | runtime -->
Source: spec/core/governance.md#a-revision-identifies-authoritative-source | A rendered or compacted representation of a charter MUST NOT replace that charter's authoritative source.
<!-- Allocation: X_S3#A_SESSION_TRANSCRIPT_LIFETIME | runtime -->
Source: spec/core/governance.md#every-turn-reply-carries-charter-references | A turn reply's durable record MUST retain its charter-reference metadata.

---

## Collaboration Declaration

ID: COLLAB
Title: Collaboration platform
Prerequisites: API, AUTH, DA, FEED, R, SF
Deliverable: Native collaboration product records and interfaces over existing shared durability, events, identity and encryption; authenticated agent/session interaction only through the frontend API.
Acceptance: The phased implementation specification supplies exact positive/negative acceptance locations for every native built-outcome obligation, retaining all guards and unresolved authority/lifecycle/representation choices. No task lease, goal entity or independent storage subsystem is selected.
Dependency: API | source-constraint | The product reaches agent/session state and operations through the API, never internal session records.
Dependency: AUTH | source-constraint | The product authenticates authors, verifies people assertions, and refuses identity spoofing.
Dependency: DA | inferred | Reuse released durable transactions for product records and atomic tenant revision; choose no additional engine.
Dependency: FEED | inferred | Reuse shared ordered change-feed and live-view mechanisms for product revisions and viewers.
Dependency: R | inferred | Reuse durable call guards and receipt/effect handling for uncertain commits and retries.
Dependency: SF | source-constraint | Tenant content and document versions use shared encryption and stored-form components.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A resolver MUST end the waits on it in the transaction that resolves it, whatever its outcome.
<!-- Allocation: COLLAB_S3_TASKS#TASK_PRIMITIVES | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task MUST be at most the configured number of levels below its top-level ancestor.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task MUST be blocked only while it waits on a task, a question, a document's operator review, or a review record.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task MUST be exempt from monitoring exactly while it is set aside.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task MUST be set aside only by an operator's own write or by a matching go-ahead.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task that has an open wait MUST be blocked.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task whose last wait ends MUST return to the status it held before its first wait, with its assignee.
<!-- Allocation: COLLAB_S3_TASKS#TASK_PRIMITIVES | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A task's status MUST be one of a closed set.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A wait on a resolver that is already closed MUST be refused.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A wait on an agent, a team, or an external party MUST be refused, so that nothing waits on a principal that may vanish.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | A wait that would close a cycle through waits and parent links MUST be refused.
<!-- Allocation: COLLAB_S5_DECISIONS#FULL_WAIT_GRAPH | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-block-is-a-wait-on-a-resolver | The event that unblocks a task MUST carry the outcome of each wait that ended.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A blocking question MUST take no default answer.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A denial, an absent block, or a cancelled, withdrawn, moot, expired, or imported answer MUST NOT count as a go-ahead.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A document's operator review MUST be decided only by an operator from an authenticated session.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A gate MUST be removed only by an operator's own write.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A gate on starting a task MUST cover the task's descendants, including those created or reparented later.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A gated action MUST proceed only on a recorded positive go-ahead whose typed fields match the action.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A go-ahead MUST bind to one principal, so that a reassigned gated task needs a new go-ahead.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A go-ahead posed with an expiry later than its control's longest expiry MUST be refused.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A moot mark MUST remove an answer's effect and never create one.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A moot mark on an answered question MUST record the acting principal and the answer that made it moot.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A posted comment, a question's typed fields, and a recorded answer MUST be immutable.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A question MUST be answered, and its answer superseded or withdrawn, only by the principal it is routed to, from an authenticated session.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A question routed to a team MUST be answerable by any member of the team under that member's own identity.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A question routed to an operator MUST ask exactly one decision.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | A question's prompt MUST state the recommended option.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An answer MUST be delivered to the asker if the asker is active, and otherwise to the current owner of the record.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An answer MUST record the authenticated principal and the authentication path it came through.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An answer whose author is not an authenticated person MUST NOT be presented as an operator's answer.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An answered question MUST NOT be cancelled or superseded.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An imported answer MUST record its import path.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | An option offered to an operator MUST NOT describe a step the operator would take by hand.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-decision-is-recorded-under-the-principal-that-made-it | The first recorded answer to a team-routed question MUST resolve its waits.
<!-- Allocation: COLLAB_S5_DECISIONS#DOCUMENTS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-durable-store-behind-it | A document MUST be tenant content held under the tenant's keys.
<!-- Allocation: COLLAB_S5_DECISIONS#DOCUMENTS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-durable-store-behind-it | A document version number MUST NOT be reused.
<!-- Allocation: COLLAB_S5_DECISIONS#DOCUMENTS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-durable-store-behind-it | Pruning document versions MUST NOT remove a current, an approved, or a pinned version, or one that a served definition version references.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-durable-store-behind-it | The service MUST keep its durable state in a store that survives the loss of a node.
<!-- Allocation: COLLAB_S5_DECISIONS#DOCUMENTS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-durable-store-behind-it | The service MUST refuse a content id a client supplies that differs from the one the service computed.
<!-- Allocation: COLLAB_S5_DECISIONS#QUESTIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-question-outlives-its-asker | A cancel or supersede MUST keep the original asker as the question's poser.
<!-- Allocation: COLLAB_S5_DECISIONS#QUESTIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-question-outlives-its-asker | A cancel or supersede MUST record the acting principal and a reason.
<!-- Allocation: COLLAB_S5_DECISIONS#QUESTIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-question-outlives-its-asker | A supersede by a principal other than the asker MUST pose the replacement under the acting principal and link the old question to the new one.
<!-- Allocation: COLLAB_S5_DECISIONS#QUESTIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-question-outlives-its-asker | An open question MUST remain cancellable or supersedable by a principal other than its asker, so that a retired asker leaves no question that cannot be closed.
<!-- Allocation: COLLAB_S5_DECISIONS#QUESTIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#a-question-outlives-its-asker | When a record's owner changes, its open questions MUST stay open.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#changes-within-a-tenant-are-ordered-by-one-revision | A change MUST NOT be ordered across tenants.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#changes-within-a-tenant-are-ordered-by-one-revision | A tenant MUST have one writer of its revision.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#changes-within-a-tenant-are-ordered-by-one-revision | Changes within one tenant MUST be ordered by one gapless revision in commit order.
<!-- Allocation: COLLAB_S3_TASKS#TASK_PRIMITIVES | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#coordination-logic-lives-in-the-collaboration-platform | Fanning work out, collecting results, looping with a cap, and re-delegating MUST be implemented in the collaboration platform as tasks and assignments rather than as a layer in the agent platform.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#every-record-has-a-durable-id-beside-its-cited-reference | A reference people cite MUST resolve only within the caller's tenant.
<!-- Allocation: COLLAB_S2_RECORDS#RECORDS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#every-record-has-a-durable-id-beside-its-cited-reference | Every record MUST carry a durable id unique across tenants beside the reference people cite.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | A project's refused statuses, required metadata, default assignee, and dwell deadline MUST be tenant configuration enforced at every write.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | A task that an open wait identifies as its resolver MUST NOT be archived automatically.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | A task that exceeds its project's dwell deadline MUST produce an overdue event to the project's coordinators.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | A task's close event MUST carry the task's pickup, waiting, and blocked durations.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | An automatic archive MUST be published as an event.
<!-- Allocation: COLLAB_S5_VIEWERS#HOUSEKEEPING | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#housekeeping-runs-from-deadlines | An automatic archive MUST be restorable.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | A refusal of a mismatched identity argument MUST be published as an event.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | A write MUST record the authenticated principal as its author.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | An identity argument MUST be accepted only when it equals the authenticated caller.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_IDENTITY | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | At most one runtime MUST be able to act as a given agent at any moment.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | Authorization MUST fail closed.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | Each external bridge or ingest service MUST act under a service principal of its own.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | Re-ingesting an external record MUST resolve to the existing record for the same external link rather than create a second one.
<!-- Allocation: COLLAB_S3_TASKS#AUTHENTICATED_MUTATIONS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | The acting identity of a call to the collaboration platform MUST come from authentication rather than from an argument.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_IDENTITY | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#identity-comes-only-from-authentication | The service MUST attest a person toward the agent platform only from the signed sign-in claims presented on the request, never from a stored copy.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-is-a-client-and-an-adapter-not-a-store-client | The service MUST reach the agent platform through the platform's API rather than through the durable store.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-keeps-no-delivery-mechanism-of-its-own | A change MUST be addressed to a person only when the change identifies that person directly, recorded at commit.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-keeps-no-delivery-mechanism-of-its-own | The service MUST NOT keep an agent inbox, a notification poll, or a liveness watchdog of its own.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-keeps-no-delivery-mechanism-of-its-own | The service MUST keep only the notifications addressed to a person.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-keeps-no-delivery-mechanism-of-its-own | The service MUST publish each new notification to that person's own topic.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | A task MUST count as open work for its assignee only when it is not started or in progress, is not archived, has no blocked or set-aside ancestor, and has no open child.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | Alarming a person MUST NOT be the only recovery action for any such state.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST NOT log or store a tail token.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST answer a stop-admission request according to whether the agent has open, unblocked work.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST broker a short-lived tail token rather than relay a session's tail itself.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST have a recovery action for every blocked, quarantined, and failed state the status-hook contract can report.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST obtain a new tail token at every connect and resume of a viewer's tail.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST receive the agent platform's session status hooks rather than have agents report their own status.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST subscribe an agent's session to a task when it assigns the task.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_WORK | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-manages-subscriptions-and-answers-hooks | The service MUST unsubscribe an agent's session from a task when the work is done.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | A published event's id MUST derive from the service's own ordered revision, so that the platform applies each change exactly once.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | A record's move into a scope MUST publish to the destination scope's topic as a change of that scope.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | A watch event MUST be scoped to the watcher alone.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | An event payload MUST carry the fields that changed, cut to the payload bound with a truncated flag.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | An event payload MUST identify each record it refers to by both its cited reference and its durable id, fixed at commit.
<!-- Allocation: COLLAB_S5_AGENT_LINK#EVENTS_API | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-publishes-every-record-change | Every change to a record MUST be published to that record's topic.
<!-- Allocation: COLLAB_S6_QUALIFICATION#QUALIFICATION | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-runs-on-many-servers-with-no-node-state | The service MUST hold no state that cannot be lost with a node.
<!-- Allocation: COLLAB_S6_QUALIFICATION#QUALIFICATION | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-runs-on-many-servers-with-no-node-state | The service MUST scale to the agent platform's session count and to the configured number of viewers.
<!-- Allocation: COLLAB_S5_AGENT_LINK#AGENT_IDENTITY | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#it-serves-definitions | An agent's definition identity MUST NOT be reassigned to another agent.
<!-- Allocation: COLLAB_S5_AGENT_LINK#DEFINITIONS | conditional-inactive -->
Source: spec/decisions/collaboration-platform/build.md#it-serves-definitions | Where the declarative alternative for root sessions is adopted, the composer MUST NOT issue a new version when the composed body is unchanged apart from its version field.
<!-- Allocation: COLLAB_S5_AGENT_LINK#DEFINITIONS | conditional-inactive -->
Source: spec/decisions/collaboration-platform/build.md#it-serves-definitions | Where the declarative alternative for root sessions is adopted, the service MUST compose a definition version only from approved document versions.
<!-- Allocation: COLLAB_S5_AGENT_LINK#DEFINITIONS | conditional-inactive -->
Source: spec/decisions/collaboration-platform/build.md#it-serves-definitions | Where the declarative alternative for root sessions is adopted, the service MUST serve a retired intent only for an explicit, recorded retirement, never inferred from a missing record or a failed read.
<!-- Allocation: COLLAB_S5_AGENT_LINK#DEFINITIONS | conditional-inactive -->
Source: spec/decisions/collaboration-platform/build.md#it-serves-definitions | Where the declarative alternative for root sessions is adopted, the service MUST serve agent definitions as a definition source over the definition-source contract.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | A check MUST report any agent whose intent and reported state differ.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | A non-emergency pause requested through the collaboration platform MUST take effect by setting that intent.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | An agent's run, paused, or retired intent MUST be the collaboration platform's mechanism for requesting a non-emergency pause of its sessions.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | Pausing an agent MUST NOT change its tasks or its questions.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | Recording an agent's retirement MUST, in the same transaction, unassign its open tasks, cancel the questions routed to it, and reroute the answers to the questions it asked to each record's current owner.
<!-- Allocation: COLLAB_S5_AGENT_LINK#LIFECYCLE | conditional-inactive -->
Source: spec/decisions/collaboration-platform/build.md#lifecycle-intent-is-the-only-pause-path | Where the declarative alternative for root sessions is adopted, the service MUST mark an agent retired only when the platform reports the retirement applied.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISION_QUEUE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-decision-queue-is-complete-by-construction | A decision owed to a person MUST NOT reach that person only through a message or a pointer outside the queue.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISION_QUEUE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-decision-queue-is-complete-by-construction | Archiving or deprecating a document in operator review MUST be refused while its review is open.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISION_QUEUE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-decision-queue-is-complete-by-construction | The decision queue MUST include every open decision routed to its viewer directly or through a team, and every document in the viewer's operator review.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-decision-queue-is-complete-by-construction | Withdrawing a document from operator review MUST publish an event to every operator.
<!-- Allocation: COLLAB_S5_DECISIONS#DECISION_QUEUE | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-decision-queue-is-complete-by-construction | Withdrawing a document from operator review MUST record the acting principal and a reason.
<!-- Allocation: COLLAB_S5_DECISIONS#OPERATOR_TEAM | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-operator-team-is-closed | A membership that would resolve a non-person into the operator team MUST be refused.
<!-- Allocation: COLLAB_S5_DECISIONS#OPERATOR_TEAM | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-operator-team-is-closed | A recovery entry for the operator team MUST be one-time, add-only, and applied once.
<!-- Allocation: COLLAB_S5_DECISIONS#OPERATOR_TEAM | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-operator-team-is-closed | A write that would remove the last operator MUST be refused.
<!-- Allocation: COLLAB_S5_VIEWERS#VIEWERS | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-operator-team-is-closed | Every change to the operator team MUST be published as an event to the operators.
<!-- Allocation: COLLAB_S5_DECISIONS#OPERATOR_TEAM | product-behavior -->
Source: spec/decisions/collaboration-platform/build.md#the-operator-team-is-closed | The operator team, and any team nested in it, MUST be changed only by an operator.
---

## Workspace Declaration

ID: WS
Title: Native workspace service
Prerequisites: API, AUTH, B, C, DA, R, SF
Deliverable: Full selected native workspace BUILD service: session-bound workspace and host leases, durable content snapshots and lineages, governed files/build/command tools, per-call credentials, host isolation, safe recovery, stored values and observable calls over existing shared primitives. Workspace leases are service-owned and distinct from session lease generations, partition fences, host leases, lineage writer generations and workspace incarnations.
Acceptance: All 150 exact native BUILD identities have one explicit primary milestone/gate allocation retaining strength and applicability. Actual host/process, credential, storage, API, fault and scale witnesses validate the selected production path; local models or reserved handles do not release it. No substrate/vendor, independent store, task lease, goal entity or deployment is selected by these proposed declarations.
Dependency: API | source-constraint | Lost-workspace notifications are published through the platform API; service callers use the existing governed API rather than writing agent state.
Dependency: AUTH | source-constraint | Every service call carries session identity; credentials come from the credential broker for the assertion and retain operator attribution, tenant/clearance scope and call lifetime.
Dependency: B | source-constraint | Workspace snapshots/forks, immutable stored values and source pins consume shared blob ownership and retention while preserving independent lineage and workspace-incarnation fences.
Dependency: C | source-constraint | The workspace is a listed tool service through the existing governed dispatcher; the generic core contains no workspace-specific executor semantics.
Dependency: DA | inferred | Atomic lineage head, incarnation/writer generation, receipts and promise records use the existing durable transaction primitive; no independent storage subsystem is selected.
Dependency: R | source-constraint | The outcome explicitly inherits tool-call receipts and guards and requires reconcile/cancel/never-received outcomes bound to actual workspace effects.
Dependency: SF | inferred | The original encryption, owner/key, tagged-format and compatibility constraints apply to workspace content and state; host-only data-key limits consume shared key resolution and sealing.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_ON_DEMAND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-is-acquired-only-for-work-that-needs-it | A host acquired for a workspace MUST be released when the work that needed it ends.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_ON_DEMAND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-is-acquired-only-for-work-that-needs-it | A workspace MAY exist without a host, as a stored tree whose builds run on a build service.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_ON_DEMAND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-is-acquired-only-for-work-that-needs-it | A workspace's stored tree MUST carry over onto a host when the host is acquired and back when the host is released.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_ON_DEMAND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-is-acquired-only-for-work-that-needs-it | Acquiring a host for a workspace MUST be a gated action that keeps the workspace's handle and lineage.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A destructive host operation MUST identify the workspace incarnation it targets and the lease it acts under.
<!-- Allocation: WS_S4_EXECUTION#D_PROMISE_BEFORE_ACK | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host MUST record each promise it makes before it answers the message that caused it.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host MUST refuse a destructive operation that identifies another incarnation or lease.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host MUST refuse as never received a message whose admission time plus the retention bound and the skew bound has passed.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_EPOCH_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host MUST treat a reboot or a damaged promise record as a lost host.
<!-- Allocation: WS_S4_EXECUTION#D_PROMISE_BEFORE_ACK | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host that cannot record a promise MUST answer the message with a retryable error and make no new promise.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_EPOCH_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host that meets a record format it does not know MUST stop with an alarm rather than start a new epoch.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_EPOCH_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A host that starts a new epoch MUST quarantine the previous epoch's workspace copies rather than delete them at once.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-host-keeps-its-promises | A lineage MUST have one writer instance at a time, fenced by a writer generation checked in the service's records before every host operation.
<!-- Allocation: WS_S4_EXECUTION#D_MUTATING_CALL_BATCH_SNAPSHOT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A call that has returned as running MUST be outside every batch.
<!-- Allocation: WS_S5_RECOVERY#D_FORK_PIN_AND_LINEAGE_RETENTION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A lineage MUST outlive its session for the tenant's retention of workspace lineages.
<!-- Allocation: WS_S5_RECOVERY#D_UNPLANNED_LOSS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A lost host MUST lose at most the calls in flight and the calls whose covering snapshot had not finished uploading.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A restore whose verification fails MUST be retried on another host.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A restore whose verification fails after the configured retries MUST fail rather than hand over the tree.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A restored tree MUST be verified under the rules recorded in the snapshot.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A restored tree MUST have the same content hash as its snapshot.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A session MUST be able to continue from its last snapshot on another host after its host is lost.
<!-- Allocation: WS_S2_STATE#D_LINEAGE_HEAD_COMMIT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot MUST NOT have two successors.
<!-- Allocation: WS_S2_STATE#D_LINEAGE_HEAD_COMMIT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot MUST become durable only when its head commit succeeds against the lineage's current head, the live workspace incarnation, and the lineage's writer generation.
<!-- Allocation: WS_S2_STATE#D_LINEAGE_HEAD_COMMIT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot MUST chain to the previous one, so that it stores only what changed.
<!-- Allocation: WS_S2_STATE#D_WHOLE_TREE_CAPTURE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot MUST cover the whole workspace, including every repository it holds.
<!-- Allocation: WS_S2_STATE#D_WHOLE_TREE_CAPTURE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot MUST store working-tree bytes as they are, applying no conversion or filter other than large-file retrieval.
<!-- Allocation: WS_S5_DELIVERY#D_VISIBLE_ID_AND_ACCESS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | A snapshot id in a result or an event MUST be an opaque id that reveals nothing about the snapshot's content.
<!-- Allocation: WS_S4_EXECUTION#D_MUTATING_CALL_BATCH_SNAPSHOT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | Calls held open on a workspace at the same time MUST form one batch covered by one snapshot.
<!-- Allocation: WS_S4_EXECUTION#D_MUTATING_CALL_BATCH_SNAPSHOT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-snapshot-follows-every-changing-call | The service MUST snapshot a workspace's contents, content-addressed, after every call that changed them.
<!-- Allocation: WS_S5_DELIVERY#S_STORED_VALUES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-stored-value-belongs-to-one-session | A stored value MUST be readable only by its session, or by a session acting for the same operator that holds the value's clearances through an explicit share.
<!-- Allocation: WS_S5_DELIVERY#S_STORED_VALUES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-stored-value-belongs-to-one-session | A stored value's id MUST be opaque.
<!-- Allocation: WS_S5_DELIVERY#S_STORED_VALUES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-stored-value-belongs-to-one-session | Every write of a stored value MUST pass the structured-secret detector before storage, with a match replaced by a marker that identifies the detector.
<!-- Allocation: WS_S5_DELIVERY#S_STORED_VALUES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-stored-value-belongs-to-one-session | The service MUST keep for a session a value that is referenced by name, versioned, and immutable, each version recording what produced it.
<!-- Allocation: WS_S5_DELIVERY#S_WORKSPACE_FREE_TOOLS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-free-tool-is-one-call-to-a-service | A session MUST receive compute of its own only when its task must check out, build, or run code.
<!-- Allocation: WS_S5_DELIVERY#S_WORKSPACE_FREE_TOOLS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-free-tool-is-one-call-to-a-service | The service MUST hold no state for such a tool beyond held credentials and the fence.
<!-- Allocation: WS_S5_DELIVERY#S_WORKSPACE_FREE_TOOLS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-free-tool-is-one-call-to-a-service | The service MUST serve a tool that is one call to an already-scaled service without provisioning a host.
<!-- Allocation: WS_S4_EXECUTION#S_BUILD_CACHE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A build cache MUST NOT be shared across tenants.
<!-- Allocation: WS_S4_EXECUTION#S_BUILD_CACHE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A build cache MUST be shared within a tenant.
<!-- Allocation: WS_S4_EXECUTION#S_BUILD_CACHE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A build cache entry MUST be keyed by the content hash of its inputs, so that one session cannot corrupt another's builds.
<!-- Allocation: WS_S4_EXECUTION#S_BUILD_CACHE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A build cache entry MUST be served only to a session whose clearance covers the domain the entry came from.
<!-- Allocation: WS_S4_EXECUTION#S_BUILD_CACHE | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A build cache entry MUST record the principal that wrote it.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A host MUST NOT be reachable inbound from outside the service.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A host MUST run one tenant's workspaces only.
<!-- Allocation: WS_S5_RECOVERY#S_DELETED_LINEAGE_COPIES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A reachable host MUST destroy a deleted lineage's copies on acknowledgment of the discard.
<!-- Allocation: WS_S5_RECOVERY#S_PROTECTED_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A restore or a rollback MUST hand the tree over through a surface the tool identity cannot write.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A session's use of the service MUST NOT be able to degrade another session's.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A tool MUST run unprivileged under resource limits.
<!-- Allocation: WS_S4_EXECUTION#S_CREDENTIAL_HANDLING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A tool process MUST NOT hold the host's store credential.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A tool process MUST see only the system and toolchain read-only, its own workspace read-write, and the paths declared visible at acquire.
<!-- Allocation: WS_S4_EXECUTION#S_EXECUTION_ISOLATION | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | A workspace's processes MUST see no other workspace's processes.
<!-- Allocation: WS_S4_EXECUTION#S_CONTROLLED_EGRESS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | All egress MUST leave through one point the service controls.
<!-- Allocation: WS_S5_RECOVERY#S_DELETED_LINEAGE_COPIES | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | An unreachable host MUST destroy a deleted lineage's copies when its lease, margin, and skew bound have passed.
<!-- Allocation: WS_S4_EXECUTION#S_CONTROLLED_EGRESS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | Every backend call MUST be bounded in time and in size.
<!-- Allocation: WS_S4_EXECUTION#S_CONTROLLED_EGRESS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | Every backend endpoint the service calls MUST come from its configuration.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_KEY_AND_STORE_AUTH | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | Every storage and key operation the service makes MUST run under a credential scoped to one tenant's prefix and key.
<!-- Allocation: WS_S4_EXECUTION#S_CONTROLLED_EGRESS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | The egress point MUST admit no route to a source that carries a domain except through the credential helper, so that the session's domain set records every entry.
<!-- Allocation: WS_S4_EXECUTION#S_CONTROLLED_EGRESS | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | The service MUST use an unencrypted transport only to an endpoint its configuration marks for it, and only where the request carries no secret.
<!-- Allocation: WS_S4_EXECUTION#S_CREDENTIAL_HANDLING | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | The service's own tooling MUST run nothing a session can configure in a repository or in a home directory.
<!-- Allocation: WS_S2_STATE#S_CAPTURE_CONFINEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded | The snapshot process MUST read every file under the workspace root whatever its modes, and nothing outside it by path or by handle.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_PROCESS_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A call whose supervisor is lost MUST end as stopped by the host.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_CREDENTIAL_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A call's credentials MUST reach only that call's own processes.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_PROCESS_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A call's processes MUST be terminated when the call ends, across any restart of the host agent.
<!-- Allocation: WS_S4_EXECUTION#S_CREDENTIAL_HANDLING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A credential helper MUST fail rather than prompt.
<!-- Allocation: WS_S4_EXECUTION#S_BROKER_CALL_BINDING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A credential the service acts with MUST allow a write only for a call that writes.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_CREDENTIAL_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A credential token MUST grant nothing once its call has ended.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_KEY_AND_STORE_AUTH | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A host MUST NOT hold a branch key or an owner key.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_KEY_AND_STORE_AUTH | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A host MUST hold only the data keys of the lineages it serves and the sources it has pinned, unwrapped off the host.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_KEY_AND_STORE_AUTH | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A host's store credential MUST allow only conditional creates of the exact objects the host may write, and never a replace.
<!-- Allocation: WS_S4_EXECUTION#S_CREDENTIAL_HANDLING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A log, event, or saved output MUST NOT carry a secret or an endpoint token.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_CREDENTIAL_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A served credential's claimed expiry MUST NOT exceed the configured upper bound, whatever the workspace type configures.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_PROCESS_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A tool identity MUST be unable to start a process that outlives or escapes its call.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_CREDENTIAL_LIFETIME | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | A workspace-local endpoint MUST serve each call's credentials only while that call runs.
<!-- Allocation: WS_S4_EXECUTION#S_BROKER_CALL_BINDING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | Credentials MUST come from the credential broker in exchange for the call's assertion.
<!-- Allocation: WS_S4_EXECUTION#S_BROKER_CALL_BINDING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | Every credential the service acts with MUST carry the operator as its source identity.
<!-- Allocation: WS_S4_EXECUTION#S_HOST_KEY_AND_STORE_AUTH | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | The host's own metadata endpoint MUST be unreachable from a tool.
<!-- Allocation: WS_S4_EXECUTION#S_BROKER_CALL_BINDING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | The service MUST hold no credential for an operator beyond the configured margin before its expiry.
<!-- Allocation: WS_S4_EXECUTION#S_CREDENTIAL_HANDLING | runtime -->
Source: spec/decisions/workspace-service/build.md#credentials-are-per-call | The service's own helpers MUST NOT place a credential in a file, an environment variable, or a command line.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_EVENTS | runtime -->
Source: spec/decisions/workspace-service/build.md#every-served-call-is-an-event | The service MUST publish an event for every call it makes to a backing service, carrying the service, the operation, the operator, and the status.
<!-- Allocation: WS_S4_EXECUTION#S_CALL_EVENTS | runtime -->
Source: spec/decisions/workspace-service/build.md#every-served-call-is-an-event | The service MUST publish an event for every tool call it serves, carrying the operator, the tool, and the outcome, never content.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_HEALTH_POOL | runtime -->
Source: spec/decisions/workspace-service/build.md#hosts-report-health-and-a-lost-host-frees-its-leases | A host MUST report its health on registration and on each heartbeat.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_HEALTH_POOL | runtime -->
Source: spec/decisions/workspace-service/build.md#hosts-report-health-and-a-lost-host-frees-its-leases | A host that misses heartbeats past the configured stale window MUST be removed from the pool and its leases freed.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_HEALTH_POOL | runtime -->
Source: spec/decisions/workspace-service/build.md#hosts-report-health-and-a-lost-host-frees-its-leases | An acquire MUST prefer a healthy host.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_HEALTH_POOL | runtime -->
Source: spec/decisions/workspace-service/build.md#hosts-report-health-and-a-lost-host-frees-its-leases | An acquire that falls back to a degraded host MUST state the degradation as a warning on the result rather than block.
<!-- Allocation: WS_S4_EXECUTION#S_LISTED_SERVICE_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#it-is-a-tool-server-separate-from-the-reasoning-loop | Every call to the service MUST carry the session's identity.
<!-- Allocation: WS_S5_DELIVERY#S_GENERIC_CORE | runtime -->
Source: spec/decisions/workspace-service/build.md#it-is-a-tool-server-separate-from-the-reasoning-loop | The core MUST contain no logic specific to what the service does beyond calling its tools.
<!-- Allocation: WS_S4_EXECUTION#S_LISTED_SERVICE_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#it-is-a-tool-server-separate-from-the-reasoning-loop | The service MUST be called as a tool server listed in an agent's definition.
<!-- Allocation: WS_S4_EXECUTION#S_LISTED_SERVICE_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#it-is-a-tool-server-separate-from-the-reasoning-loop | The service MUST be reachable only from the platform's nodes.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_LEASE_RENEWAL_AND_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | A command the session was told was interrupted MUST have stopped, even on a host cut off from the service.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_LEASE_RENEWAL_AND_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | A host MUST stop a lineage's commands when its host lease for the lineage ends unrenewed.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_LEASE_RENEWAL_AND_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | A host lease MUST be renewed only by the lineage's writer.
<!-- Allocation: WS_S5_RECOVERY#D_UNPLANNED_LOSS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | A lineage left with no durable snapshot MUST end, freeing its name.
<!-- Allocation: WS_S5_DELIVERY#D_OFFLINE_API_EVENTS | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | A lost host MUST produce one event per lost workspace into the holding session, published through the platform's API, carrying the handle, the reason, the last durable snapshot or none, and the ids of the calls whose effects were discarded.
<!-- Allocation: WS_S5_DELIVERY#D_OFFLINE_API_EVENTS | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | Deleting a held workspace's lineage MUST end its lease offline with an event that carries that reason and no snapshot.
<!-- Allocation: WS_S5_RECOVERY#D_UNPLANNED_LOSS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | The service MUST NOT restore a workspace after an unplanned loss without the session acquiring it again.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_LEASE_RENEWAL_AND_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | The service MUST declare a host lost only after its lease plus the margin since the last renewal it holds, or when the substrate reports the host stopped.
<!-- Allocation: WS_S5_RECOVERY#D_HOST_LEASE_RENEWAL_AND_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#offline-is-an-event | The writer MUST commit the next renewal of a host lease only after the host acknowledged the last.
<!-- Allocation: WS_S5_RECOVERY#D_COMMAND_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A call that its host never received MUST NOT be sent to the host late.
<!-- Allocation: WS_S5_RECOVERY#D_COMMAND_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A call that its host never received MUST settle as interrupted at the current head.
<!-- Allocation: WS_S4_EXECUTION#D_CALL_RESULT_PROTOCOL | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A call's ending MUST be one of a closed set.
<!-- Allocation: WS_S4_EXECUTION#D_CALL_RESULT_PROTOCOL | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A call's ending MUST state by itself whether the call's effects inside the workspace exist.
<!-- Allocation: WS_S4_EXECUTION#D_CALL_RESULT_PROTOCOL | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A call's spooled output MUST NOT be cut while the call can still write.
<!-- Allocation: WS_S5_DELIVERY#D_VISIBLE_ID_AND_ACCESS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A handle, a lineage, a snapshot, or a receipt that its caller is not entitled to MUST answer as if it did not exist.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_DEADLINE_STOP | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A host MUST end a call no later than the lesser of the message's transit time and twice the clock-skew bound after its deadline.
<!-- Allocation: WS_S4_EXECUTION#D_CALL_RESULT_PROTOCOL | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A late result MUST attach at most the configured tail of each output stream and state how many bytes it left out.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A lineage MUST belong to the session that started it.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A live workspace MUST be held by exactly one session.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A new incarnation of a session MUST end every lease the session's older incarnations held.
<!-- Allocation: WS_S4_EXECUTION#D_CALL_RESULT_PROTOCOL | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A reconcile answer for a workspace call MUST identify the snapshot that followed the effect.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A running command MUST count as use of the workspace's lease.
<!-- Allocation: WS_S4_EXECUTION#D_HOST_OPERATION_FENCING | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A side effect of an older session incarnation MUST NOT be admitted once the service has recorded a newer one.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A workspace MUST be addressed by a handle that is valid only for the session that holds its lease.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A workspace lease MUST belong to the service rather than to the platform.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | A workspace lease MUST expire unless it is used, so that the platform needs no eviction hook.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | Acquiring a workspace MUST carry the scope the task's grant allows.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | An acquire MUST NOT fail because the host that last held the workspace is unavailable.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | An acquire of a workspace name on which the session already holds a lease MUST return that handle and change nothing.
<!-- Allocation: WS_S5_DELIVERY#D_VISIBLE_ID_AND_ACCESS_BOUNDARY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | An expired handle MUST fail with a distinct error that identifies the workspace's last snapshot.
<!-- Allocation: WS_S5_RECOVERY#D_COMMAND_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | An implicit cancel MUST be recorded when it is decided.
<!-- Allocation: WS_S5_RECOVERY#D_COMMAND_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | Recovery MUST cancel rather than reattach to a command whose record is marked cancelled or whose lease has ended.
<!-- Allocation: WS_S4_EXECUTION#D_SERVICE_LEASE_AND_SCOPE | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | The scope of an acquire MUST default to read-only.
<!-- Allocation: WS_S5_RECOVERY#D_COMMAND_RECOVERY | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | The service MUST NOT repeat on its own a command that may have acted outside the workspace.
<!-- Allocation: WS_S5_DELIVERY#D_COMPLETE_SERVICE_OPERATIONS | runtime -->
Source: spec/decisions/workspace-service/build.md#operations | The service MUST let a session acquire a workspace, operate on its files, submit a build from it, and release it.
<!-- Allocation: WS_S5_RECOVERY#S_RESTORED_DOMAIN_CLEARANCE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A continuation, restore, or fork MUST be refused for a session whose clearance does not cover the lineage's domain set.
<!-- Allocation: WS_S4_EXECUTION#S_ACQUIRE_CREDENTIAL_CLEARANCE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A credential request for a repository outside the session's clearance MUST be refused at the time of the request, with an event.
<!-- Allocation: WS_S5_RECOVERY#S_RESTORED_DOMAIN_CLEARANCE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A fork MUST add its source session's domain set to its own.
<!-- Allocation: WS_S5_DELIVERY#S_DOMAIN_STATE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A lineage MUST carry its session's domain set.
<!-- Allocation: WS_S4_EXECUTION#S_NETWORK_PLACEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A session MUST be able to hold several workspaces at once, each with its own handle and its own snapshot lineage.
<!-- Allocation: WS_S5_DELIVERY#S_DOMAIN_STATE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A session MUST keep a domain set that only grows, recording the domain of every repository that enters any of its workspaces.
<!-- Allocation: WS_S4_EXECUTION#S_NETWORK_PLACEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A session's own workspaces MAY reach each other.
<!-- Allocation: WS_S4_EXECUTION#S_NETWORK_PLACEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A workspace MUST NOT reach another session's workspaces.
<!-- Allocation: WS_S4_EXECUTION#S_NETWORK_PLACEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A workspace type MUST carry its network placement, so that a tool needing a particular network runs on a workspace placed there.
<!-- Allocation: WS_S4_EXECUTION#S_NETWORK_PLACEMENT | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | A workspace type MUST carry text through its tools unchanged, including characters outside the basic character set.
<!-- Allocation: WS_S4_EXECUTION#S_ACQUIRE_CREDENTIAL_CLEARANCE | runtime -->
Source: spec/decisions/workspace-service/build.md#placement-and-clearance | Acquiring a workspace for a repository MUST require the session's clearance for the repository's domain.
<!-- Allocation: WS_S5_RECOVERY#D_FORK_PIN_AND_LINEAGE_RETENTION | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A fork MUST pin the snapshot it starts from.
<!-- Allocation: WS_S5_RECOVERY#D_FORK_PIN_AND_LINEAGE_RETENTION | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A fork MUST start a new lineage from a snapshot.
<!-- Allocation: WS_S5_DELIVERY#D_ACTUAL_SESSION_FORK_CALLERS | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A fork that needs a workspace MUST take a snapshot fork from the service.
<!-- Allocation: WS_S5_RECOVERY#D_PLANNED_MOVE | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A host lost after a planned move's restore has started MUST produce no offline event.
<!-- Allocation: WS_S5_RECOVERY#D_PLANNED_MOVE | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A planned move MUST NOT serve a call on the new host before the head is restored there.
<!-- Allocation: WS_S5_RECOVERY#D_PLANNED_MOVE | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A planned move MUST run between calls, after the last snapshot's upload.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A restore of any snapshot MUST read at most the configured number of blobs, whatever the lineage's length or fork depth.
<!-- Allocation: WS_S5_RECOVERY#D_ROLLBACK_EXCLUSION_AND_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A rollback MUST be refused while any call that changes the workspace is in flight.
<!-- Allocation: WS_S5_RECOVERY#D_ROLLBACK_EXCLUSION_AND_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A rollback MUST make an earlier snapshot the head as a new snapshot, so that a lineage only grows.
<!-- Allocation: WS_S5_RECOVERY#D_ROLLBACK_EXCLUSION_AND_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A rollback MUST start only when no read of the workspace is being served.
<!-- Allocation: WS_S5_DELIVERY#D_ACTUAL_SESSION_FORK_CALLERS | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A sub-session that needs its parent's workspace MUST take a snapshot fork rather than share the parent's live workspace.
<!-- Allocation: WS_S5_RECOVERY#D_RESTORE_VERIFICATION_AND_BOUND | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A swap MUST restore a workspace's snapshot onto any host.
<!-- Allocation: WS_S5_RECOVERY#D_ROLLBACK_EXCLUSION_AND_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | A workspace whose rollback check failed or whose hand-over did not finish MUST NOT be used again.
<!-- Allocation: WS_S5_RECOVERY#D_FORK_PIN_AND_LINEAGE_RETENTION | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | Deleting a lineage MUST NOT make a live fork of it unrestorable.
<!-- Allocation: WS_S5_RECOVERY#D_ROLLBACK_EXCLUSION_AND_HANDOVER | runtime -->
Source: spec/decisions/workspace-service/build.md#swap-fork-and-rollback | Every call on a workspace other than a reconcile or a close MUST be refused while its rollback is in flight.
<!-- Allocation: WS_S5_DELIVERY#S_WORKSPACE_FREE_TOOLS | runtime -->
Source: spec/decisions/workspace-service/build.md#the-service-holds-the-credentials-for-a-workspace-free-tool | A credential for a tool that needs no workspace MUST be held and used by the service alone, so that no model-written code or tool process holds it.
<!-- Allocation: WS_S5_DELIVERY#S_WORKSPACE_FREE_TOOLS | runtime -->
Source: spec/decisions/workspace-service/build.md#the-service-holds-the-credentials-for-a-workspace-free-tool | Every call to such a tool MUST act with credentials issued for the operator the call acts for.
---
