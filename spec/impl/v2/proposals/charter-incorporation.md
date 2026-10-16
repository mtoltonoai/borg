# Shared Charter Incorporation

This is the selected V2 charter increment. Matthew directed, “We should pull in the charter stuff from the shared spec.” It adopts the full fourteen shared Charter References obligations and the charter-specific definition, configuration and executor behavior needed to make them usable. It supersedes the earlier per-inner-reply citation/audit deferral. It does not claim the behavior is already implemented or qualified.

## Source And Scope

The authoritative shared source is `spec/core/governance.md`, section Charter References. Its fourteen sentences are copied exactly into the V2 main source. The exact source hash is `d687a5a53ccb79da3fc377f816969c9ea3a23b82466a4b125bfd152be5f384a3`. The four stable leaf sections are A Charter Has Normative Requirements (two obligations), A Revision Identifies Authoritative Source (three), Every Turn Reply Carries Charter References (six), and Reference Checks Produce Audit Information (three). Their governing words and audit-only meaning are unchanged.

A charter means the definition's reusable identity and purpose prompt components. A task instruction remains task-specific. A turn is one model request and its reply with the tool calls the reply makes; a final task result or an outer supplied-executor turn does not substitute for every such turn. The glossary's streaming ambiguity does not alter that model-request scope.

The exact source/acceptance mapping accompanying this proposal contains all 28 new main tuples, original source bytes and hashes, section and line locations, and one primary capability/gate allocation per tuple. Fourteen are exact shared adoptions; ten narrow existing configuration/executor obligations to charter changes; four state the concrete reusable-definition, delivery and effective-binding responsibilities. No new opaque normative ID is invented in that mapping; native extraction generates IDs from the saved main source.

| Supporting shared source | Selected meaning | Scope not imported |
|---|---|---|
| `spec/glossary.md`: Charter, revision/reference/clause/metadata, Definition, Turn and Turn boundary | Reusable source; exact clause tuple; actual model request/reply scope; boundary after prior results commit. | No new source service or arbitrary path/URL resolution authority. |
| `spec/core/sessions-and-partitions.md`: A Change Applies At The Next Turn Boundary | Charter-specific pending/effective binding; safe next boundary; check on load; no idle wake solely to adopt. | Keyed-template fanout, partition placement and general live adoption of all definition settings. |
| `spec/decisions/execution/direct.md`: A Prefix Change Is A Mid-Conversation Instruction | Deliver a changed native charter as a mid-conversation instruction. | General transcript/cache compatibility claims. |
| `spec/decisions/execution/direct-context-compaction-and-cache.md`: A Charter Change Schedules A Compaction | Debounced low-priority change-triggered compaction; summary under new charter; include an update in already-due compaction. | General context-error retry/reset/blocking, blob freezing or all transcript compaction behavior. |
| `spec/decisions/execution/hosted-harness.md`: The Platform Delivers And Wakes / A Step Boundary Has Nothing In Flight | Deliver charter configuration at an actual supported step boundary with no model/tool call in flight. | Invented Codex signals, an executor upgrade/fork, separate Borg compactor or new host controls. |
| `spec/contracts/definition-source.md`: What A Definition Carries | Background evidence that prompt components belong to reusable definition configuration. | The source contract is explicitly conditional on a deferred decision; this increment does not activate its index/change-feed service or import its complete configuration field list. |

Shared Specification As A Product remains deferred. Separately selected governance work is composed independently; charter citation validity does not import completion judgment, rule evaluation, retry, reset, emergency authority or semantic compliance.

## Immutable Inputs And Live Adoption

The initial charter source belongs to the accepted definition. Existing complete immutable definition or document-version identities are reused when they resolve all authoritative source; rendering or compaction is not source authority. Exact clause references retain file, stable section and exact sentence within that revision. Existing access rules apply, and a logical source filename does not grant filesystem or network access.

A live charter update is a separate narrow canonical operation and pending/effective session binding. It preserves original operation identity and compares the current binding before accepting an immutable successor. It does not mutate accepted TaskInput, selected inference, tools, budget, limits or prior replies. The existing twelve-field limit update remains limit-only and future-only. Selecting a new agent head is not evidence that a running session adopted it.

A model request uses an acknowledged effective binding selected at the actual next-turn boundary. A change arriving during a tool call waits until its result is committed. Recovery checks the pending binding on load, retains historical effective bindings and resolves uncertain original operations without resending model or tool effects. An idle session is not woken merely to adopt the change.

Native update delivery and the debounced compaction are actual missing implementation work. They are triggered by the configuration change, independently of citation audit findings. The compaction still needs an actual selected executor/provider mechanism and durable completion association; scheduling a flag or appending text does not demonstrate it.

## Reply Records And Passive Audit

Each reply needs a durable original occurrence association, its effective authoritative revision, original declared clause references and structural check outcome. Missing or malformed metadata remains identifiable as missing or malformed; host bookkeeping does not fill in governing clauses or turn absence into a valid empty declaration. A reply cited by a task result/history/audit view retains this association. A lookup receipt never substitutes for the original reply or update receipt.

Checking compares revision/file/section/exact sentence with retained authoritative source. It records invalid or mismatched references with their reply. It does not prove the behavior complied, and does not itself produce corrective prompts, retry, reset, blocking, compaction, a model judge or new authority. Independent output validation and durable operation ordering still apply. A storage failure is reconciled through existing recovery rather than disguising an unknown request as an empty successful reply.

## Implementation Batches And Decisive Acceptance

| Milestone | Actual producer/consumer scope | Decisive acceptance |
|---|---|---|
| `V2_CHARTER_RECORDS` | Root common types; immutable source resolver; initial/pending/effective bindings; reply/audit record and existing store/query consumer. | Exact source reuse/resolution; clause identity and changed-body rejection; CAS/replay/failed commit/restart; old records preserve absence; both executor consumers compile against one contract. Freeze wire/schema/worker and edge representations explicitly. |
| `V2_CHARTER_NATIVE` | All selected native request paths and actual worker reservation/response/tool sequence. | Controlled Responses receives separate charter and task instruction; every received response retains correct original association, including tool-only/malformed-reference cases; live update waits for prior result; burst schedules one actual compaction under the new source; restart never repeats unknown effects. |
| `V2_CHARTER_CODEX` | Pinned supported instruction delivery and actual request/reply boundary capture. | Actual pinned executable proves initial and changed model-visible charter at safe boundaries plus every model reply's references. Public item capture alone is partial evidence; a synthetic private notification or final-only result does not close the gap. |
| `V2_CHARTER_JOIN` | Existing product/read projections and recovery over both actual executor producers. | Durable original reply and reference audit association survives bounded reads, restart and compacted rendering. Wrong revision/unknown clause/missing metadata produces passive findings without behavioral intervention. Full every-turn evidence is required for the release dependency. |

Existing capability and stage IDs remain. The four new milestones use the existing controller store, native/workspace executors and product consumers. `V2_RELEASE` additionally consumes `V2_CHARTER_JOIN:CHARTER_JOURNEY`; old graph edges and readiness are preserved. Native implementation can proceed while the Codex interface gap is investigated, but an incomplete Codex seam does not qualify the full joined release.

## Source-Backed Engineering Gaps

The engineering packet `charter-engineering-integration-01/handoff.json` is bound by SHA-256 `39e0eb7f66960a5cde3401f7a643f4361217a8d612001ebd1ef935c2a039745a`; its design is `ae30f9b8d3f4c83c4f98fa03ad53185f757b5cdc4a239f8de8e53384d18051fb`. It contains sixteen proposed discriminators and six logical ownership groups, not executed tests.

Current Definition has no charter. Native request paths own response boundaries but lack a complete durable per-response reference record and compaction/update mechanism. Current session and definition-limit code preserves accepted inputs; it does not implement pending/effective charter updates. These are required production changes, not annotation gaps.

The pinned Codex public app-server `ServerNotification` union does not expose the internal raw-response definitions as supported notification methods. Public completed items are not proven one-to-one with upstream model replies or safe inner request boundaries. Generic experimental opt-in does not establish a missing method. The exact independent source review is `ab843df8a7ea0e0e61c033a4e8dcdd2e734fbd2467d288fd7b05aed9086400c5`; its report is `220a852975c7f85b645ba48247111eae08cf0de3e4025bae20b6f5aa12d5972f`. Full Codex every-reply delivery therefore remains a concrete interface/qualification gap. No new version, fork, dependency or proxy is selected by this proposal.

## Unspecified Interface Details

Streaming segmentation, empty applicability and charter-specific retention are deliberately unspecified by shared governance. Root's concrete contract records partial/failed response associations without redefining Turn, an explicit empty declaration without treating it as semantic compliance, and source retention sufficient for existing retained audit associations under the chosen existing policy. It does not invent a charter TTL, force all empty lists to fail, fabricate citations for old data or silently delete source. Bounded encoding/metadata and old/new-reader disposition also need concrete consumer cases. These remain engineering contract details, not permission to omit every-reply behavior or an invented extra human-approval gate.

## Adoption And Evidence Boundaries

This artifact changes specification, protocol, capability allocation, plan, decision/provenance and review files only. All 334 prior main requirement tuples and existing graph IDs are intended to remain; 28 new authored tuples give a proposed charter-only total of 362 before separately approved governance composition. The historical shared-profile reconciliation and retained old snapshots stay unchanged.

Root merges concurrent governance additions, saves canonical files once, uses the existing native source capture/planning route, replaces the capability snapshot/selection header with actual new output and rebinds review hashes. The old snapshot header in the staged capability file is intentionally pending that execution; it is not evidence for the new paragraphs. No build, test, provider, host, installed-package, native extraction or full conformance claim accompanies this source preparation.
