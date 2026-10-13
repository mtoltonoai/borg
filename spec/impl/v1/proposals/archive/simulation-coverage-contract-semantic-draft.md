# Simulation Coverage Contract Proposal

Status: UNAPPROVED proposal for Matthew's review. This file changes no approved
specification, implementation, test, graph, dependency, or exception. Coding
remains paused. Bach and Bolero are already selected in
[the implementation specification](../spec/implementation.md#simulation-and-generated-testing);
their versions, engines, integration, and the contract below are not approved.

## Proposed Contract

Measure simulation against a reviewed inventory of distinct, versioned behavior
obligations: required transitions, fault activations, and explicitly selected
combinations. Derive that inventory from the applicable original requirements
and directly approved state-model decisions, not from whichever paths tests
happen to execute. Instrumentation observes execution; it cannot infer what
behavior ought to exist. A transition is an obligation because of that reviewed
mapping, not because a developer labels it critical.

Use Duvet's pinned extraction as the requirement catalog. Each obligation refers
to an original identity (file, stable section, exact sentence); do not author a
second requirements database. Proposed coverage declarations belong alongside
the state-model declarations they describe, with one authored source and any
checker input generated from it. Their eventual location and encoding require
review. Current experimental model enums are evidence to inspect, not approved
state-model declarations by default.

## Exact Proposed Input Fields

These are logical fields for review, not an adopted serialization format.

| Record | Fields and meaning |
|---|---|
| Contract | `schema_version`, `contract_digest`, `approval_ref`, `spec_snapshot_digest`, `gate_selection_digest`, `duvet_report_digest`, `implementation_spec_digest`, `model_source_digest`, `model_version`, `scope_id`, `scope_version`. Pin both the original requirements and the approved implementation decisions. |
| Scope | `components`, `consumption_sites`, `required_obligation_refs`, `scenario_invariant_refs`, `bounds`, `applicability_rules`, `unmapped_requirement_refs`, `unresolved_model_decisions`. Denominator membership is explicit before execution. Simulation of a substitute and testing the real component remain separate scopes. |
| Obligation | `id`, `version`, `kind` (`transition`, `fault`, `combination`), `component`, `requirement_refs`, `model_element_ref`, `provenance` (`source_constraint` or `approved_model_decision`), `decision_ref` when applicable, `precondition`, `activation_predicate`, `assertion_refs`, `completion_condition`. Identity for counting is `(id, version)`. A semantic change increments the version; an ID is never reused for a different obligation. |
| Transition detail | `from_state`, `trigger`, `guard`, `to_state_or_observable_outcome`. Include refusal/no-change and recovery transitions when required; an attempted operation is not automatically an observed transition. |
| Fault detail | `dependency_boundary`, `fault_class`, `injection_point`, `activation_observation`. Distinguish selecting/requesting a fault from the simulator actually injecting it at the declared operation. |
| Combination detail | `member_refs`, `correlation_keys`, `temporal_predicate`, `completion_condition`. Specify order, overlap, same transaction/session/owner as appropriate, and the recovery/assertion endpoint. No implicit Cartesian product or inferred pairwise policy. |
| Assertion | `id`, `version`, `requirement_refs`, `oracle_source_digest`, `test_ref`, `predicate`, `observation_refs`, `evaluation_points`, `completion_condition`. A negative property needs an observation interval or bounded domain; absence of an event alone is insufficient. |
| Recorded run | `run_id`, all input digests above, `input_manifest_digest`, `source_revision`, `working_tree_content_digest`, `test_binary_digests`, `checker_version_and_digest`, `toolchain_pins`, `tool_versions_and_digests`, `configuration_digest`, `commands`, `scenario_refs`, `trace_digests`, `replay_refs`, `termination`, `trust_class`. Include the used Bach/Bolero versions and engine/configuration when integrated; do not invent pins now. |
| Scenario/replay | `scenario_id`, `input_digest`, `generated_input_ref`, `seed`, `generator_version`, `schedule_or_choice_trace_ref`, `fault_plan_ref`, `model_bounds_digest`, `termination`, `replay_command`, `replay_artifact_digests`. Preserve the actual generated/shrunk input and scheduling/fault choices needed for replay; a seed alone is not a cross-version reproduction promise. |

`requirement_refs` resolve to Duvet identities from the pinned report and original
source, including exact quotations; they do not carry editable requirement
definitions. Unresolved references, duplicate obligation IDs, stale model or
assertion references, and missing approval references prevent an accepted
denominator. An incomplete draft can produce a labeled preview, never a claim
that it inventories all applicable behavior. Unmapped requirements stay visible;
governing exclusions are not inferred from missing runtime mappings.

## Required Observations For This Proposal

Proposed trace event fields are `schema_version`, `run_id`, `scenario_id`,
`sequence`, `operation_id`, `component`, `kind`, relevant obligation/assertion
ID and version, `correlation_keys`, and `observation_ref`. Use logical ordering
and pinned observation data, not wall-clock arrival order. Event kinds are:

- `transition_observed`: records actual pre-state, trigger, and post-state/result.
- `fault_requested` and `fault_activated`: activation comes from the injection
  boundary and identifies the affected operation; a never-consumed request earns
  no fault coverage.
- `assertion_evaluated`: records assertion identity, supporting observation
  references, and `pass`, `fail`, or `unknown`; `pass` is emitted only after the
  declared predicate and completion condition have been evaluated.
- `scenario_finished`: records `completed`, `failed`, `timed_out`, `truncated`,
  or `tool_error`, plus the last complete sequence and pending checks. A missing
  terminal record is incomplete evidence, not implicit success.

Observation hooks would be needed at component outcomes, actual fault injection
points, and reference-model/assertion evaluation sites. These hooks are NOT
implemented or authorized by this proposal. Retain checks of observed behavior
against an independent model; an emitted ID by itself is only presence evidence.
The checker joins assertions to the same scenario and operation interval as the
obligation. Combinations require their complete declared temporal predicate in
one scenario; hits from unrelated runs cannot be unioned into a combination.
Sequence gaps, unknown IDs, missing observation artifacts, and dropped trace
records remain explicit evidence errors. Instrumentation records observations,
not a self-issued certificate of correctness.

## Metrics And Denominators

For each scope and each kind, let `N` be the count of distinct required
`(id, version)` obligations in its approved, pinned declaration. Publish the ID
sets as well as counts. Do not count event frequency, test functions, seeds, or
elapsed time as this denominator; never average per-component percentages.

| Metric | Exact definition |
|---|---|
| `required_total` | `N`, with declaration digest and scope. Unknown until the scope's inventory is approved; a proposal preview has a separately labeled count. |
| `exercised / N` | Distinct required obligations with a validated activation/transition/combination witness, regardless of assertion outcome. |
| `locally_assertion_satisfied / N` | Distinct required obligations with all their declared assertions observed passing over a completed obligation interval. May include an interval in a scenario that later fails or times out; this is not a completed-scenario pass. |
| `passing_complete_witness / N` | Distinct required obligations with all required assertions passing and completion conditions met in a completed, passing scenario with complete validated trace. |
| `failed / N` | Distinct required obligations with a valid failing assertion witness. Keep failure IDs and replay references even if another scenario passes. |
| `mixed / N` | Intersection of `failed` and `passing_complete_witness`. A successful replay never removes the counterexample. |
| `missing_exercise` | `N - exercised`, with the missing IDs. |
| `missing_complete_pass` | `N - passing_complete_witness`, with IDs and reasons, including assertion gaps and incomplete scenarios. This can overlap `failed`. |
| `assertion_gap` | Exercised IDs lacking a complete set of valid assertion outcomes for an obligation interval; distinguish never evaluated, unknown, and incomplete observation. |
| `incomplete_attempts` | Counts and scenario/obligation references by `timed_out`, `truncated`, `tool_error`, or absent terminal record. These are run diagnostics, not percentages of covered obligations. |
| `failed_assertions`, `pending_assertions` | Distinct versioned assertion IDs plus occurrence/replay references; keep per-scenario results to avoid hiding conditional checks. |
| `scenario_invariant_checks` | Passed, failed and missing expected checks keyed by `(scenario_id, assertion_id, version, evaluation_point)`. Derive expected checks from the declared invariant schedule and scenario inputs/observed steps, not from assertion receipts that happened to arrive. Incomplete traces leave completeness unknown. A passing check in one scenario cannot satisfy a missing check in another. |
| `requirement_mapping` | Distinct Duvet identities mapped to declarations / all extracted identities in the pinned gate selection, plus unmapped identities and approved scope/exclusion decisions. Mapping is not implementation or behavioral coverage. |
| `evidence_strength` | Report original levels `presence`, `executed`, `mutation-checked`, `proved` separately per requirement, with their actual evidence references. Assertion-backed execution alone does not establish mutation sensitivity. |

These sets overlap and are not additive categories. A zero denominator reports
`not assessed` unless an explicit reviewed scope establishes no obligations of
that kind; never display vacuous 100%. Requirements and obligations are different
denominators: one original requirement may entail several independent obligations.
Unreachable or disabled paths do not disappear from `N` based on test results;
any scope revision is reviewed, versioned, and visible as a denominator change.
Simulator fault injection and real-adapter probe parity are reported separately.

Keep native Duvet implementation/test link counts, exact citation errors, local
execution witnesses, assertion receipts, and mutation/proof evidence distinct.
None of these metrics individually establishes full conformance or promotion.

## Bounded Exploration

Record `model_digest`, `bounds` (state/input domains, actors, queues, time and
fault/scheduling choices, and trace depth), `exploration_method_and_version`,
`visited_states`, `observed_model_edges`, `pending_frontier`,
`unexplored_successors`, `pruning_or_reduction_rules`, and `termination`.

For sampling/fuzzing, distinct visited state/edge counts describe observations;
the total reachable-state denominator is unknown. Use the word exhaustive only
when every initial state and allowed successor within the declared finite model
bounds has been explored, no frontier or successor remains, and no timeout,
truncation, unsupported choice, or unjustified pruning invalidates that claim.
Any reduction needs its stated validity argument. The claim is only about that
model and those bounds, never all platform states or real concurrency. Seed,
step, case and wall-clock counts remain effort metrics. No exploration engine,
numeric budget, Loom, or Miri adoption is proposed as an approved choice.

## One Deterministic Check Interface

Proposed extension to the existing development checker; THESE FLAGS DO NOT EXIST:

```text
python3 tools/development_check.py --simulation-contract <approved-declarations> --recorded-run <run-directory> --output <report-directory>
```

This interface checks already-recorded evidence; it does not silently launch
tests. It validates the pinned Duvet report and original identities, approvals,
input/tool digests, trace integrity and replay artifacts, then computes the
sets above. Replaying scenarios is a separately recorded execution whose result
can be checked through the same interface. Same pinned inputs yield identical
sorted obligation results, metrics and failure IDs; timestamps/output paths are
diagnostic metadata outside the deterministic result digest.

Proposed output: `scope_result` (`pass`, `fail`, `unknown`), `metrics`,
`obligation_results`, `failures`, `incomplete_scenarios`, `unmapped_requirements`,
`input_and_tool_digests`, `replay_refs`, `trust_class`, and
`promotion_allowed=false`. A valid counterexample makes the scope fail despite
passing witnesses. With no counterexample, missing obligations/assertions,
unapproved or incomplete denominator, timeout/truncation, or invalid inputs
produce unknown, not pass. A scope pass requires all declared obligations to
have complete passing witnesses and no unresolved evidence errors. It is not
a full-spec verdict. Every expected scenario-invariant check also needs a passing
receipt; aggregate obligation coverage cannot hide a missing check in another
scenario. Proposed exit codes: 0 scope pass, 1 valid fail or incomplete
scope, 2 invalid input/tool failure; structured results preserve the distinction.
These interface/result choices require approval before implementation.

## Existing Evidence And Reuse

Read-only inspection used raw checkpoint
[`run-_pijyq0s`](../../borg-runtime/artifacts/conformance/run-_pijyq0s/traceability-report.json),
not the older checkpoint in `docs/development-conformance.md`. No development-check
run or runtime test was executed for this proposal.

- [Native Duvet output](../../borg-runtime/artifacts/conformance/run-_pijyq0s/duvet-metrics.json):
  version 0.4.3, 32 selected files, 134/1,817 full implementation/test pairs,
  1,683 missing implementation and 1,683 missing test links (overlapping),
  1,769 mandatory requirements. Report/query-implementation/query-test/strict
  exits were 0/1/0/1. Exact checker found 0 stale/malformed citations. Duvet's
  extra descriptive keyword blockquote accounts for the local inventory's
  1,816 denominator; keep the discrepancy visible, not silently excluded.
- [Execution evidence](../../borg-runtime/artifacts/conformance/run-_pijyq0s/execution-evidence.json):
  105 separately executed cited tests, represented by 157 citation entries, with
  commands, executable hashes and per-test LLVM coverage. This is not evidence
  that all 199 runtime tests were separately instrumented. Scope classifications
  were 124 linked/locally executed, 31 partial, 11 unimplemented, and 1,650
  unassessed (567 associated with absent subsystems). No semantic obligation
  coverage percentage can be recovered from these records.
- [Input manifest](../../borg-runtime/artifacts/conformance/run-_pijyq0s/input-manifest.json):
  49 hashed inputs. Read-only comparison found current `dispatch.rs` differs;
  the checkpoint does not witness that subsequent edit. The proposed extension
  would also pin approved implementation decisions, coverage/model declarations,
  oracle/instrumentation sources, trace artifacts and complete tool/replay inputs.
- [development_check.py](../../borg-runtime/tools/development_check.py):
  reuse native Duvet staging/metrics (lines 34-133), source hashing (75-81),
  exact-test selection and per-test LCOV (188-250), reviewed classifications
  (253-284), and before/after input verification (287-317). Add no parallel gate.
  Gaps: no semantic transition/fault IDs, assertion receipts, activated-fault
  counts, combination evaluator, replay manifest, or exploration completeness
  accounting. Current timeouts become a general failure record, not per-scenario
  truncation/pending-obligation accounting. Duvet 0.4.3 supplies no Rust execution
  coverage through its query interface; LLVM is a separate local witness.
- [citation_inventory.py](../../borg-runtime/tools/citation_inventory.py):
  reuse pinned snapshot/config validation and exact file/section/sentence
  matching (lines 273-340); it deliberately cannot witness execution. Retain it
  as an exact-match diagnostic against Duvet, not a second authored catalog.
- [traceability-review.json](../../borg-runtime/tools/traceability-review.json):
  166 reviewed identities distinguish bounded implemented, partial and absent
  behavior. Reuse those scope assessments; they do not specify the denominator
  of transitions, faults, assertion IDs or combinations and cannot approve the
  current code's design assumptions.
- [session_model.rs](../../borg-runtime/components/borg-sim/tests/session_model.rs):
  independent history checks and repeated-seed replay, with generated apply,
  hold/drop, move, reconstruct, stale-owner and before/after-failure actions
  (lines 507-559). [processing_model.rs](../../borg-runtime/components/borg-sim/tests/processing_model.rs)
  checks recovered attempt phases and effects against its journal (lines 609-700).
  [store_model.rs](../../borg-runtime/components/borg-sim/tests/store_model.rs)
  explicitly asserts a generated outcome set (497-523) and exercises chosen
  transaction failures plus a moved fence (539-603). These are concrete reusable
  checks, not exhaustive state exploration or an approved transition inventory.
  Their iteration counts are not coverage; none emits the proposed receipts.

## Source Mapping And A Small Review Example

The original sources already require the behavior/evidence below; the proposed
field names, aggregation rules and executable interface are new review choices.

| Original source | Existing obligation relevant to this proposal |
|---|---|
| [Constitution IV](../constitution.md#iv-deterministic-by-construction), lines 46-56 | Every component runs under simulation; external dependencies have verified simulators; entropy and ordering are deterministic. |
| [Every Dependency Has A Verified Simulator](../spec/core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator), lines 29-37 | Inject actual dependency failures, verify simulator/real-probe expectations, and fail a chosen store transaction. |
| [The Gating Invariants](../spec/core/simulation-and-conformance.md#the-gating-invariants), lines 43-88 | Assertions for inbox order/exactly-once, fences, recovery, two tenants, bounds and other applicable invariants, including a superseded owner regaining access. |
| [The Failure Table Is Covered](../spec/core/simulation-and-conformance.md#the-failure-table-is-covered), lines 92-94; [failure handling](../spec/core/failure-handling.md#the-failure-table) | Every failure class has a scenario asserting handling/final state; blocked and quarantined states have an exercised exit. |
| [Properties Are Generated And Checked Against A Model](../spec/core/simulation-and-conformance.md#properties-are-generated-and-checked-against-a-model), lines 98-99 | Generated sequences and independent model comparison, with invariants checked on every run. |
| [Coverage Requires Implementation And Test](../spec/core/simulation-and-conformance.md#coverage-requires-implementation-and-test), lines 123-132; [Behavior Is Witnessed By Execution](../spec/core/simulation-and-conformance.md#behavior-is-witnessed-by-execution), lines 138-144 | Discriminating behavior tests and witnessed execution; presence/hits alone are insufficient. |
| [Identity Is The Quoted Sentence](../spec/core/simulation-and-conformance.md#identity-is-the-quoted-sentence), lines 154-160; [immutable snapshot](../spec/core/simulation-and-conformance.md#the-gate-evaluates-against-an-immutable-snapshot), lines 164-166 | Exact identities, stale-reference failures, pinned specification and gate configuration. |
| [Evidence Record](../spec/contracts/evidence-record.md), lines 21-68 | Checker/subject/configuration identity, pass/fail/unknown, evidence strength, reproducibility and trusted-runner restrictions. Local workspace records are not promotion evidence. |

Illustrative declarations ONLY, not an approved denominator:

- `store.chosen-transaction-failure@1` (fault) references
  `spec/core/simulation-and-conformance.md`, section
  `every-dependency-has-a-verified-simulator`, exact sentence:
  "The durable store's simulator MUST be able to fail one chosen transaction on demand, so that a scenario exercises the retry of a fence check."
  Proposed witness: the chosen operation's injection point is observed, followed
  by the declared retry/fence assertions. An injection request alone is missing
  activation. The specific failure point remains part of the reviewed model.
- `ownership.reconnect-after-move@1` (combination) references that file, section
  `the-gating-invariants`, exact sentence:
  "A scenario that checks the fence MUST include a superseded owner that regains access to the store after its partition moved."
  Proposed witness: within one scenario, ownership moves, the old owner regains
  store access and attempts a write, and an assertion observes rejection with
  no committed effect. Separate move and stale-write tests cannot jointly claim
  this ordered combination. Its required safety assertion also references
  `spec/core/sessions-and-partitions.md`, `ownership-is-fenced-by-generation`:
  "A commit by a superseded owner MUST fail."

## Decisions Still Unapproved

Review this contract's fields, ID/version rules, event receipts, aggregation and
check interface before implementing them. Approve the relevant state-model
decisions, complete obligation/combination inventory, applicability and model
bounds before using an accepted denominator. No full inventory exists yet.
Actual numeric budgets, replay retention, trace encoding/storage, tool versions,
fuzzing engines, instrumentation integration and any exploration/reduction
technique remain proposals. Existing declared defaults are not changed here.
No off-the-shelf exception is selected; any substantive entry still needs direct
approval and observation at its consumption levels. Bach/Bolero selection is
already approved; neither is integrated in current Borg manifests/source.
No Loom/Miri adoption, real-adapter architecture or trusted-runner provisioning
is authorized by this proposal.
