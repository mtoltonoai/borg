# Core: Implementation Organization

> **CORE SPECIFICATION.** Organization and evidence-attribution rules shared by every implementation, independent of implementation-specific choices. This document defines how implementations share one contract while keeping their sources, targets and evidence distinct. It traces to [overview section 15](../overview.md).
>
> The capitalized key words are normative with the meaning given by the constitution. Each requirement is one self-contained sentence with one obligation under a stable heading. Concrete directory layout and evaluation selections are recorded descriptively in the decision record.

## Implementation Organization

### Implementations Share One Contract

The specification repository MUST allow authored implementation specifications for multiple implementations of the shared specification.

Implementations described in the specification repository MUST share one authoritative specification.

Each implementation MUST have a stable implementation identifier.

Each implementation's authored implementation specification MUST occupy a distinct implementation directory in the specification repository.

An implementation's source code, tests, tooling, and generated artifacts MUST be kept in an implementation repository separate from the specification repository.

An implementation's authored decisions, capability declarations, and phase declarations MUST be kept in its implementation specification.

### Each Implementation Identifies Its Target

Each implementation MUST declare a target reference that resolves to the exact immutable shared-specification source it targets.

### Evidence Is Attributed To Its Implementation

Implementation evidence MUST identify the implementation it concerns.

An implementation's planning associations MUST remain separate from its implementation citations, test citations, and conformance results.

An implementation's conformance claim MUST NOT be discharged by another implementation's citations or execution results.

### Shared Tests Keep Results Separate

Shared conformance test definitions MAY be kept outside implementation directories.

A result of a shared conformance test MUST be attributable to the implementation it exercised.

### Generated Artifacts Are Separate

Generated artifacts MUST be kept separate from authored sources.

The requirement identities and evaluation snapshots remain those specified under [Identity Is The Quoted Sentence](simulation-and-conformance.md#identity-is-the-quoted-sentence) and [The Gate Evaluates Against An Immutable Snapshot](simulation-and-conformance.md#the-gate-evaluates-against-an-immutable-snapshot). Evidence provenance remains subject to [The Record Identifies Its Subject](../contracts/evidence-record.md#the-record-identifies-its-subject), [The Record Identifies The Property](../contracts/evidence-record.md#the-record-identifies-the-property), and [The Record Is Reproducible](../contracts/evidence-record.md#the-record-is-reproducible). No particular source-control technology is required by this organization mechanism.

These organization requirements concern design, change process, and attribution of evidence under [A Governing Requirement Binds To The Change Process](simulation-and-conformance.md#a-governing-requirement-binds-to-the-change-process); their classification is recorded with the declared defaults. They remain requirements and remain visible in the raw extracted inventory. This section does not make planning associations evidence of runtime behavior or weaken existing execution and promotion rules.

An implementation identifier identifies a realization of the shared specification, not a wire or protocol version. This organization mechanism introduces no version-negotiation or adapter requirement; existing protocol and durable-format compatibility requirements continue to apply. The concrete directory layout is recorded descriptively in [the decision record](../record.md#implementation-organization). Recording the organization does not migrate existing work or approve implementation-specific proposals.
