# Capability — Simulation And Conformance

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how the platform is tested so that its invariants are checked, not asserted: every dependency
> under a simulator kept honest by a contract test, the invariants every scenario checks, and the
> requirement gate that maps every requirement here to the code and test that satisfy it. Requirements
> realize [Core Principle IV](../../constitution.md) and [Core Principle VII](../../constitution.md) and
> trace to [overview §17](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The simulation framework, the entropy source, and the gate tool
> are declared defaults.

## Purpose And Scope

A design is only as good as the tests that hold it to its invariants. This capability fixes that
everything runs under deterministic simulation, that every external dependency has a simulator a contract
test keeps honest, that the gating invariants are checked on every scenario, and that every
requirement in this specification binds to code and a test that exercises it.

## Deterministic Simulation

### Everything Runs Under Simulation

Every component MUST run under the deterministic simulation framework.

Every external dependency MUST be replaced by a simulator under simulation.

A component MUST take its entropy from the simulation's entropy source.

A component MUST NOT depend on a wall clock, a thread schedule, or a collection order that varies between
runs of the same inputs.

### Time Is A Dependency

The simulation MUST provide a per-node wall clock whose offsets, drift, and steps can be injected.

A clock-fault scenario MUST be expressible, so that the timer discipline is tested.

### Every Dependency Has A Simulator Kept Honest

Each dependency's simulator MUST inject latency and its tail, throttles, errors, timeouts, dropped
connections, duplicates, reordering, and partial failures.

Each simulator MUST have a contract test that runs the same expectations against the simulator and,
through a probe, against the real dependency.

A contract test MUST fail when the simulator drifts from the real dependency.

## The Invariants Every Scenario Checks

### The Gating Invariants

A scenario MUST check that each inbox event applies exactly once relative to the session's state,
through crashes, moves, duplicate publishes, and retried commits.

A scenario MUST check that events apply in order per session.

A scenario MUST check that no commit or dispatch from a superseded owner takes effect.

A scenario MUST check that a superseded owner's delayed dispatch cannot take effect before its receiver
records the replacement generation, including outside targets covered by the tool-call contract.

A scenario MUST check that a crash at any point leaves the session at its step in progress and the next
owner finishes it.

A scenario MUST check that no tool runs unless the card lists it.

A scenario MUST check that no output acts or leaves before its deciders pass except for clearly marked
unapproved output viewed by explicitly authorized privileged observers.

A scenario MUST check that ordinary viewers cannot receive unapproved output.

A scenario MUST check that privileged observation cannot bypass read grants or confidentiality checks.

A scenario MUST check that privileged observation of unapproved output does not permit an ungated tool
effect.

A scenario MUST check that a structured secret never reaches the model's context, the transcript, state,
events, or logs.

A scenario MUST check, with two tenants, that no record, cache entry, blob, or event crosses between
them.

A scenario MUST check that no transaction exceeds the key bound.

A scenario MUST check that the transcript head stays within its bound.

A scenario MUST check that queues are trimmed.

A scenario MUST check that exactly one usage record exists per model attempt, tool call, and decider
call.

### The Failure Table Is Covered

Each failure class MUST have a scenario that asserts its handling and the state the session ends in.

Each blocked and quarantined state MUST have a scenario showing a way out an adapter can drive.

### Properties Are Generated And Checked Against A Model

A property test MUST run generated sequences of events, tool outcomes, crashes, moves, and source
changes through the platform and through a reference model, and check the invariants on every run.

## The Requirement Gate

### Every Requirement Binds To An Enforcing Line

Every requirement in this specification MUST bind to at least one citation that detects its violation.

A statement that no mechanism can bind to an enforcing line MUST be written as descriptive prose rather
than as a requirement.

### A Governing Requirement Binds To The Change Process

A requirement that governs how the specification or its configuration may change MUST bind to the
change-process check rather than to a runtime citation.

A requirement that mandates a design or governance discipline rather than a runtime behavior MUST NOT be
counted as gating for a build's requirement gate.

A governing requirement excluded from the gating set MUST be recorded at the declared-default
location, so that two builds judge against an identical gating set.

### Coverage Requires Implementation And Test

A requirement MUST be counted as covered only when it has both an implementation citation and a test
citation.

A cited test MUST exercise the behavior its requirement describes rather than merely reference the
requirement's text.

A cited test MUST fail when the specific behavior its requirement describes is removed or violated.

Two requirements that describe distinct behaviors MUST NOT be discharged by one shared check that cannot
fail for one without failing for the other.

### Behavior Is Witnessed By Execution

A requirement that describes runtime behavior MUST be discharged by a test that executes that behavior
and observes its result.

A requirement that pins the shape of an artifact MUST NOT be counted as covered by a citation that
produces the shape without a path that exercises it.

A cited test's execution MUST be witnessed, so that a citation on code that never runs does not count.

### The Gate Is The Promotion Bar

A build in which any requirement at the MUST level lacks both an implementation and a test citation MUST
NOT be promoted.

A requirement at the SHOULD or MAY level MAY be left uncovered.

### Identity Is The Quoted Sentence

A requirement's identity MUST be the tuple of its specification file, its section, and its exact quoted
sentence.

A citation MUST record the requirement it satisfies by quoting the requirement's sentence, so that
changing the wording invalidates every citation that no longer matches.

A citation whose quoted text matches no requirement MUST be a failure that names its location.

### The Gate Judges Against An Immutable Snapshot

A gate run MUST judge against a content-addressed snapshot of the specification.

A gate run MUST record the configuration it judged against, so that two runs judge against an identical
set.
