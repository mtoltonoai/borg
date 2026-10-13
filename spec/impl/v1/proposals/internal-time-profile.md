# Internal Time Profile For Qualification

Engineering candidate for the two existing internal model-deadline and grant-expiry scalar fields. This document is not an adopted production durable format, a change to public contract times, or an addition to the approved implementation specification. It introduces no external technology.

## Recommendation And Scope

The initial qualification used one explicitly versioned, nonnegative Unix-microsecond representation for these two internal fields. It reused the selected Rust standard library and the existing native/Bach clock boundaries. Microseconds preserve subsecond precision without the much shorter range of signed nanoseconds, while fitting the current common nonnegative signed-64-bit storage bound. This was an engineering candidate, not a precision requirement extracted from the shared specification. The subsequent comparison below now favors seconds plus nanoseconds for the next behavior-preserving consumer evaluation; neither representation is an adopted durable format.

The shared implementation guidance delegates behavior-preserving layout and encoding choices to the implementer. This supports private codec implementation and qualification without asking the operator to choose a unit. It does not authorize changing accepted external inputs, authority validity, timer guarantees, retained-data interpretation, or canonical implementation-spec wording. Any such change remains a separate decision.

Sources: [implementer-owned mechanisms](../../../decisions/README.md#what-the-implementer-decides-without-asking), [irreversible choices](../../../../constitution.md#irreversible-choices-are-decided-first), and [the existing whole-second public contract convention](../../../record.md#contracts).

## Candidate Conversion Rules

The rules in this section describe the initial microsecond candidate only; they do not specify the seconds-plus-nanoseconds comparison profile.

- Give the profile an explicit version/domain identity. A bare integer is insufficient evidence that a stored value uses this profile. The tag must be retained with the record or unambiguously bound by its enclosing durable format. Physical tag placement is not selected here.
- Decode a profile-qualified integer exactly as that many microseconds after the Unix epoch. Check the nonnegative signed-64-bit intersection, arithmetic overflow and actual native time representability; do not wrap, saturate or reinterpret negative values.
- Convert existing public whole-second timestamps explicitly and exactly, with checked multiplication. Public units and serialization remain unchanged. Before integration, audit accepted external range and precision: an internal representation limit is not permission to reject an otherwise required valid input. A range or precision conflict blocks this candidate integration.
- The candidate codec accepts an absolute timestamp only when it is exactly representable in both its declared source format and this internal profile. Its unsupported-value result is a qualification finding, not permission for the API to reject an otherwise required valid input. Valid off-grid inputs disqualify an incompatible integration; do not silently round an absolute expiry upward or a not-before due downward.
- For comparison against an already encoded grid-aligned instant, floor a fresh wall observation to microseconds. This preserves the boundary exactly: `floor(wall / quantum) >= encoded_due` if and only if `wall >= decoded_due`. Pre-epoch and out-of-range observations refuse.
- Keep timestamp construction separate from observation. Compute a relative target at full available precision before quantization. A grant must not gain lifetime through rounding, and a not-before timer must not fire earlier than requested. If one representable instant cannot preserve all applicable bounds, refuse instead of selecting a weaker interpretation. The codec does not automatically choose a relative-duration or model-timeout policy.
- Preserve actual backward-step/currentness checks. Do not use quantization, a process-wide maximum or monotonic extrapolation to conceal a relevant wall-clock change. Each authority check obtains a fresh observation.
- After restart, decode the original stored absolute due. Do not replace it with restart time plus a remembered duration. A monotonic instant remains a process-local wait hint only.

Unlabelled integers in existing synthetic fixtures remain qualification-domain values. They do not acquire Unix meaning. Historical reports and old fixture bytes remain unchanged. No production migration or reissue is specified here.

## Required Qualification Cases

Test exact boundaries immediately before, at and after an encoded due/expiry; checked whole-second conversion; negative/pre-epoch inputs; signed-limit and native-time overflow; unknown/missing domain tags; and refusal of unlabelled old fixture values. Exercise both actual consumers' equality rules without replacing their currentness or authority checks.

Test relative construction separately with values between adjacent ticks and with simultaneous lower/upper bounds. Include a case that cannot preserve both bounds and therefore refuses. Quantization must not silently turn a maximum validity bound into a minimum-delay rule, or the reverse.

Run the same owned comparison/decoding cases under Bach and the native source. Keep real dependency observations distinct from modeled wall time. Demonstrate restart use of the original due, backward steps, forward steps, node offsets and drift. This does not establish physical clock synchronization or timely observation of a step.

Affected durable writes remain behind a reviewed version/tag binding and the accepted-input range/precision check. Pure codec tests and the already reviewed raw clock tests can proceed independently. No source, policy or legacy-data reinterpretation is implied by passing them.

## Remaining Clock Producers

This encoding supplies no synchronization service, healthy-clock assertion, clock-step notification, store commit timestamp, owner/route proof, issuer or authority. The original clock-step event, timer hold/alert, fresh wall read and grant-currentness requirements remain necessary. No polling workaround, new scheduler or time abstraction framework is selected.

Sources: [time as a dependency](../../../core/simulation-and-conformance.md#time-is-a-dependency), [absolute timer due and no early firing](../../../core/timers.md#once-or-recurring), [clock-fault handling](../../../core/timers.md#a-partition-detects-a-faulty-clock), and [partition wake discipline](../../../core/sessions-and-partitions.md#a-partition-sleeps-on-its-doorbell-and-its-deadlines).

## Alternatives

| Representation | Tradeoff |
| --- | --- |
| Unix microseconds | Initial qualified candidate; fits the current scalar shape but has the conditional range and precision limits below. |
| Unix nanoseconds | Preserves more native precision but substantially narrows the signed-scalar date range. |
| Unix milliseconds or seconds | Coarser quantization; seconds match public contract units but do not establish every internal construction's required precision. |
| Seconds plus nanoseconds | Preferred next consumer-evaluation candidate after the five-case comparison; retains precise subsecond values over a wider range but changes the existing single-scalar/storage shape. |

The consumer mapping and range audit can disqualify either candidate. That result would be reported rather than hidden by changing public input bounds or silently reinterpreting records.

## Executed Comparison And Next Integration Boundary

The [seconds-plus-nanoseconds comparison](/home/dev-user/borg-runtime/contracts/deadlines/artifacts/native-clock-odnf88j7/consumer-binding-audit-clxenm7o/execution-completion.json) passed five Bach tests. An [independent execution audit](/home/dev-user/borg-runtime/integration/governed-reference-turn/artifacts/native-durable-model/deadline-successor-review-a9pa4zj2/pair-execution-audit.json) verified the retained sources, executable, commands and cleanup. It ran no native clock, database or provider case. Earlier raw-clock and microsecond results remain separate historical subjects; the earlier comparison notes describe their pre-execution state.

The pair represents nonnegative seconds through the signed-64-bit bound with normalized nanoseconds. It preserves exact nanosecond boundaries and equal relative lower/upper bounds that can have no microsecond grid point. The whole-second number 9,223,372,036,855 is below the public number-form limit but overflows the microsecond scalar after multiplication. This is a conditional compatibility witness: no existing public model-deadline parser or grant issuer establishes that it accepts that value as a Unix timestamp. It is not an observed public-input failure.

The next integration must establish each actual input's domain, precision and required range. It must bind the representation to the immutable original model-start record or the actual grant record and read set. Existing revision or transaction identifiers do not provide that binding. Unlabelled scalar fixtures keep their original meaning.

No source-record tag placement, durable encoding, schema migration, issuer or wait strategy is selected here. A far-future pair can compare correctly even when a native sleep target cannot be constructed; that does not permit rejecting a required input or adding periodic polling. The actual caller still supplies clock health, current ownership, authority and cancellation settlement. This comparison introduces no additional operator question or external dependency.
