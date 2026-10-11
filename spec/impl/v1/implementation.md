# Implementation Specification

> **V1 IMPLEMENTATION SPECIFICATION.** This specification applies to implementation v1. The shared constitution, contracts, core requirements and selected outcomes take precedence. The constitution defines the meaning of the capitalized requirement keywords used here. Each keyword sentence states a binding v1 requirement or permission under a stable heading, with the strength defined by the constitution.
>
> The values in Implementation Decisions specify the implementation choices. The declaration contract defines the fields of capabilities and milestones, including their delivery, acceptance and gate criteria. Shared-source quotations retain their original identities and applicability and are not new v1 requirements.
>
> A requirement applies to its subject: collaboration boundaries govern product behavior; development, approval, traceability, schema and generation requirements govern the change process or its artifacts. The shared governing-requirement rules continue to apply. Fixed choices are not evidence of implementation, and process compliance is not runtime conformance. Paragraphs explicitly labeled informative explain these requirements without adding obligations.

## Implementation Decisions

### Tenancy Scope

One-tenant with dedicated hosts and stores.

### Execution

Direct execution.

### Collaboration Platform

The v1 implementation MUST provide the native collaboration platform specified by COLLAB and its seven milestone declarations.

The collaboration platform MUST reuse the existing shared-framework durability, events, change feeds, live streams, authentication, call guards, and encryption mechanisms for its product records.

The collaboration platform MUST access agent session state and operations only through the platform API.

The product's gapless per-tenant revision MUST order its own committed record changes.

Binding scope: the product revision orders product records, not agent session cursors, partition fences or platform event sequences. Tasks, documents, questions, reviews and revisions are domain data over existing primitives. This specification selects no new storage engine, independent agent-state store, task claim lease or goal entity. The tenancy and execution selections above apply unchanged.

Core emergency controls MUST remain independent of collaboration-product availability.

A non-emergency pause requested through the collaboration platform MUST use the agent's lifecycle intent.

A collaboration go-ahead MUST NOT widen agent authority.

Collaboration actions MUST remain subject to current platform policy and grants.

Product questions and go-aheads MUST NOT replace the platform's decision-hook protocol.

Product questions and go-aheads MUST NOT replace the checkers-only outcome.

The collaboration platform's runtime-exclusivity mechanism MUST NOT prohibit legitimate concurrent sessions.

Binding scope: runtime exclusivity does not establish a task lease. Session management is imperative; the five declarative-only collaboration clauses remain conditional-inactive.

Informative: the seven collaboration milestones contain the acceptance for assignment/publication/subscription recovery, repeated stop decisions, assignment/stop races, shared-queue deadline callbacks and bounded atomic retirement/resolver operations. The schemas, role mappings, exclusivity mechanisms, capacities and representations not fixed by those criteria remain unresolved.

Approval of the implementation-specific collaboration declarations MUST NOT be treated as activation of the shared collaboration outcome.

Informative: the active shared outcome is identified by the shared decision record and gate selection.

### Development Conformance Checks

Duvet MUST be run during development as implementation progresses.

Progress updates MUST report the latest actual Duvet metrics.

The existing obligations remain those in [Building From The Specification](../../../AGENTS.md#building-from-the-specification), [Coverage Requires Implementation And Test](../../core/simulation-and-conformance.md#coverage-requires-implementation-and-test), [Behavior Is Witnessed By Execution](../../core/simulation-and-conformance.md#behavior-is-witnessed-by-execution), [The Gate Decides Promotion](../../core/simulation-and-conformance.md#the-gate-decides-promotion), and [The Evidence Record](../../contracts/evidence-record.md).

Duvet results MUST NOT replace those gates.

### DAG-Driven Parallel Development

An implementation dependency DAG MUST be derived from the original Borg requirements and the current code.

The implementation dependency DAG MUST be maintained.

Each graph node MUST be associated with its original source requirement identities: file, stable section, and exact sentence.

Dependent work MUST honor its prerequisites before it starts.

Dependent work MUST honor its shared interfaces before it starts.

Dependent work MUST honor its invariants before it starts.

Ready independent branches MUST be executed in parallel with isolated ownership and file boundaries.

Coordination MUST remain in this existing BorgBuilder thread.

Shared files MUST NOT have simultaneous writers.

Integration MUST use the required behavior tests.

Integration MUST use actual Duvet checks.

Integration MUST use the original applicable conformance gates.

Integration MUST use the original applicable evidence gates.

### Single-Source Top-Level Capabilities

The authoritative implementation specification set MUST be the authored source of the top-level capabilities and their explicit prerequisites.

Top-level capability declarations reside in the linked [Implementation Capability Specification](implementation-capabilities.md), which is part of this authoritative set.

Each capability entry MUST have a stable ID.

Each capability entry MUST have an explicit prerequisite list.

Capability declarations MUST be written consistently enough for deterministic extraction without guessing.

JSON MUST be generated from those declarations.

Dependency order MUST be generated from those declarations.

Diagrams MUST be generated from those declarations.

Generated outputs MUST NOT be another authored manifest.

Duvet MUST be used for original requirement extraction.

Duvet MUST be used for original requirement traceability.

Duvet MUST be used for original requirement coverage.

Duvet MUST NOT be used to derive semantic dependencies.

The graph MUST remain at the capability level.

The graph MUST remain at the top level without nested subgraphs.

The graph MUST NOT introduce a duplicate requirement inventory.

Implementation status MUST remain outside structural capability declarations.

Worker assignments MUST remain outside structural capability declarations.

DAG validation MUST check output reproducibility.

DAG validation MUST check for duplicate IDs.

DAG validation MUST check for missing IDs.

DAG validation MUST check for unknown prerequisites.

DAG validation MUST check for cycles.

DAG validation MUST check original-source references.

Unmapped requirements MUST be reported explicitly.

### Simulation And Generated Testing

Bach is the selected simulation-testing framework.

Bolero is selected for generated test inputs, property testing, and fuzzing.

Original source: [Properties Are Generated And Checked Against A Model](../../core/simulation-and-conformance.md#properties-are-generated-and-checked-against-a-model).

### Component-Level Simulation

Every component developed for Borg MUST be individually testable under Bach simulation.

Every off-the-shelf component whose real implementation is not itself simulation-tested MUST be explicitly listed in this implementation specification at every level where it is consumed.

Informative: simulation of a substitute or interface does not establish simulation testing of the real third-party implementation.

Informative: listing a component does not approve an exception.

Each substantive exception entry MUST have Matthew's direct approval.

Original sources: [Core Principle IV: Deterministic By Construction](../../../constitution.md#iv-deterministic-by-construction), [Everything Runs Under Simulation](../../core/simulation-and-conformance.md#everything-runs-under-simulation), [Every Dependency Has A Verified Simulator](../../core/simulation-and-conformance.md#every-dependency-has-a-verified-simulator), and [Declared Defaults: Simulation](../../record.md#simulation).

### Phased Delivery And Coverage

A phased implementation specification MUST be defined.

Phase references MUST use stable capability IDs.

The linked [Phased Implementation Specification](implementation-phases.md) is the authoritative definition of the seven-stage structure within this implementation specification set. Its stages are milestone views, not implicit global barriers.

Implementation MUST proceed in phases that prioritize a working end-to-end durable system before completing coverage of isolated components.

The staged implementation MUST establish working end-to-end behavior for every applicable component.

Durable integration MUST demonstrate actual persistence.

Durable integration MUST demonstrate process-restart recovery.

Durable integration MUST demonstrate representative cross-component flows.

Informative: a first durable milestone is not completion of every component; an in-memory simulator does not establish durability.

Required behavior coverage MUST broaden as implementation progresses.

Required fault coverage MUST broaden as implementation progresses.

Required recovery coverage MUST broaden as implementation progresses.

Each phase MUST require essential correctness evidence.

Each phase MUST require essential safety evidence.

Each phase MUST report measured line coverage with remaining gaps.

Each phase MUST report measured branch coverage with remaining gaps.

Each phase MUST report actual Duvet results with remaining gaps.

No intermediate numeric coverage threshold is established here.

Final coverage MUST reach 100% of executable lines in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

Final coverage MUST reach 100% of instrumented branches in owned component logic under Bach simulation, within explicitly approved scope and exceptions.

Informative: original applicable requirements, real-dependency probe parity, and conformance and promotion gates still apply. Passing a development phase does not establish full conformance or authorize promotion.

Original sources: [P5: Durable State, Disposable Memory](../../../constitution.md#p5-durable-state-disposable-memory), [Durable Store Durability](../../core/shared-framework.md#durable-store-durability), [Self-Durable Store](../../decisions/durability/self-durable.md), [Simulation And Conformance](../../core/simulation-and-conformance.md), and [The Evidence Record](../../contracts/evidence-record.md).

### Duvet Artifact Traceability

Duvet MUST be used to link all artifacts back to the specification.

This includes the capability and dependency-DAG planning artifact.

Artifact traceability MUST use the same authored requirement references.

Native Duvet citation outputs MAY be generated from those references.

Generated citation outputs MUST NOT become another authored source.

Planning associations MUST be kept in a separate Duvet report.

Planning citations and report results MUST NOT count as runtime implementation evidence.

Planning citations and report results MUST NOT count as behavior-test evidence.

Planning citations and report results MUST NOT count as execution evidence.

Custom DAG tooling MUST be limited to capability and milestone declarations, explicit prerequisites, schema and cycle validation, phase readiness and gate reporting, and deterministic graph or execution-plan views and generated adapters.

A second hand-authored requirement inventory MUST NOT be maintained.

DAG and planning tooling MUST consume Duvet's native JSON requirement and citation graph and native coverage.

DAG and planning tooling MUST NOT re-extract the normative requirement catalog.

Original requirement identities MUST be resolved against each pinned Duvet report.

Stable capability IDs MUST be resolved against each pinned Duvet report.

Legacy annotation array indices MUST NOT be used as persistent identities.

Source hashes MUST be checked for integrity.

Exact original requirement references MUST be checked for integrity.

The selected interface is that supported by installed Duvet 0.4.3.

### Duvet Cap

Duvet Cap is the logical name of the existing capability and dependency layer over Duvet.

Capability DAG validation MUST use Duvet Cap.

Capability DAG generation MUST use Duvet Cap.

Duvet Cap MUST read capability declarations and explicit prerequisites from the authoritative implementation specification set.

Duvet Cap MUST consume Duvet's native requirement and citation graph and coverage.

Duvet Cap MUST validate exact requirement references.

Duvet Cap MUST validate the declaration schema.

Duvet Cap MUST validate dependency cycles.

Duvet Cap MUST deterministically generate dependency order.

Duvet Cap MUST deterministically generate JSON.

Duvet Cap MUST deterministically generate diagrams.

Duvet Cap MUST preserve stable capability IDs.

Duvet Cap MUST preserve original requirement identities: file, stable section, and exact sentence.

Duvet Cap MUST NOT use report-local annotation indices as persistent identities.

Duvet Cap MUST NOT re-extract the normative requirement catalog.

Duvet Cap MUST NOT introduce a second authored requirement inventory.

Informative: the planning/runtime evidence separation above applies to Duvet Cap. Tool use does not approve a declaration or dependency.

### Duvet Cap Phase Execution Plans

Duvet Cap MUST generate phase-specific execution plans from the linked authoritative implementation, phased, and capability specifications.

Duvet Cap MUST use the structured stage and milestone declarations defined below.

Duvet Cap MUST determine milestone readiness from recorded approvals and explicitly required interface readiness.

A milestone that integrates a capability MUST identify the required interface conditions for that capability's prerequisites.

An undeclared prerequisite condition MUST remain a readiness blocker.

An interface-definition-only milestone MUST identify its limited scope explicitly.

Duvet Cap MUST evaluate milestone completion separately against its declared acceptance and evidence gates.

Duvet Cap MUST generate deterministic dependency order.

Duvet Cap MUST generate deterministic parallel-ready sets with non-overlapping declared ownership boundaries.

Duvet Cap MUST report missing declarations explicitly.

Duvet Cap MUST report missing approvals explicitly.

Duvet Cap MUST report missing prerequisite readiness explicitly.

Duvet Cap MUST report missing gate evidence explicitly.

Duvet Cap MUST keep progress outside authoritative declarations.

Duvet Cap MUST keep worker assignments outside authoritative declarations.

Generated execution-plan views MUST NOT approve declarations.

Generated execution-plan views MUST NOT authorize runtime execution.

Informative: generated projections identify their source and evaluation scope. Reported interface/gate receipts are planning inputs, not proof of behavior or trusted conformance.

### Duvet Cap Declaration Contract

Duvet Cap declarations MUST conform to the field definitions in this section.

A milestone's implementation MUST satisfy its declared acceptance criterion.

A milestone's completion evidence MUST satisfy each declared gate expectation.

**Binding schema definitions.** These definitions specify declaration syntax and field meaning.

The main specification's links labeled "Implementation Capability Specification" and "Phased Implementation Specification" identify the two authoritative documents. Other links, including proposal links, are not imports. Each declaration has one authored definition. Source hashes cover all three authoritative documents.

Stage metadata is authored in one fenced `duvet-cap-stages` TOML block in the phased specification. Its `stages` array uses `id`, positive unique `order`, `heading` referring to an existing stage heading. Stage order controls presentation, not implicit prerequisite edges.

Milestones are authored in one fenced `duvet-cap-milestones` TOML block in the phased specification. An empty `milestones` array declares no milestones. Each entry uses:

| Field | Meaning |
|---|---|
| `id`, `capability`, `stage` | Stable milestone ID and references to declared capability and stage IDs. |
| `title`, `acceptance` | The authored deliverable label and observable acceptance criterion. Acceptance specifies the delivery obligation; parsing the field does not establish that the criterion is satisfied. |
| `scope` | `integration` by default; explicit `interface` describes interface definition only and leaves consumer integration prerequisites deferred. |
| `provides` | Interface IDs this milestone can make available. |
| `requires` | Explicit `producer-milestone:interface` references; cross-stage references are allowed if acyclic. |
| `owns` | Canonical relative file/directory paths, without glob patterns, defining declared ownership boundaries. Missing boundaries block readiness. |
| `gates` | Entries with stable local `id` and an `expectation`. Expectations define completion evidence; a gate is not a prerequisite for starting its own milestone. |

IDs use uppercase letters, digits and underscores, starting with a letter. Capability declarations identify deliverables, acceptance criteria, prerequisites, dependency rationales and exact shared-source references.

An optional progress overlay uses JSON fields `schema_version` (1), `spec_set_digest`, and `milestones`. Each milestone can report `interfaces`, `gates`, and an `assignment`. Interface receipts use `status` (`ready`, `not_ready`, `unknown`) and an `evidence` reference. Gate receipts use `status` (`pass`, `fail`, `unknown`) and an `evidence` reference. A positive status without an evidence reference is insufficient. The planner records these references without certifying their contents.

Informative: source-order invariance applies to semantic dependency/readiness results; provenance hashes change when authored bytes change. Parsing or passing tooling tests does not adopt a substantive milestone, acceptance criterion, dependency or architecture.

Informative: readiness uses the recorded capability, stage and milestone approvals, nonempty ownership boundaries and required producer-interface receipts. A consumer needs the declared producer release, not completion of every unrelated producer gate. Completion separately uses the milestone's declared gate receipts. The planner validates schema and evidence-reference presence; it does not evaluate acceptance prose or certify the contents of evidence.
