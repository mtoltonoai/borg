# Capability — Spec-Driven Design

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how a requirement becomes an entity that code and tests cite one by one, how a specification
> version moves from Draft to Green against the code, and how the change gate uses the same data as a
> floor that can block a change but never admits one alone. Requirements realize
> [Core Principle VII](../../constitution.md) and [Governance Floor "Agents Never
> Approve"](../../constitution.md) and trace to [overview §20](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The requirement-gate tool and the export format are declared
> defaults; the evidence a gate reads is pinned by the [evidence-record contract](../contracts/evidence-record.md).

## Purpose And Scope

Whether code still matches its specification should be computed, not judged. This capability fixes
requirements as citable entities, the specification lifecycle, and the gate's role as a floor. The
mechanism that runs the gate at a hook, and the autonomy it earns, is the governance capability.

## Requirements As Entities

### A Requirement Is Cited One By One

A requirement MUST have an identity that code and tests cite individually.

A citation MUST quote the requirement it satisfies, so that a reworded requirement invalidates the
citations that no longer match.

A reworded requirement MUST invalidate exactly the citations of that requirement and no others.

Retitling or moving a document MUST NOT invalidate a citation.

### Documents Are Trees Of Typed Blocks

A specification document MUST be a versioned tree of typed blocks.

A requirement block MUST carry a stable id, a level, a polarity, a statement, conditions, parameters,
relations, and rationale.

A requirement's text MUST be generated from its block, so that extraction is exact.

A requirement block's id MUST survive an edit, a move, a split, and a merge.

A requirement block's id MUST NOT be reused.

### An Export Is The Pin

Each repository MUST commit an exported specification that citations resolve against.

A citation MUST name a requirement by its id compared against the statement at the pinned version.

## The Lifecycle

### A Version Moves From Draft To Green

A specification version MUST move through Draft, Preview, Target, and Green.

Preview MUST show the failures a version would add and resolve against the code.

Target MUST open one tracking ticket whose work list is the gate's failures.

Green MUST be reached, and the ticket closed, exactly when the trusted reports on every bound
repository's main show no failures against the version.

A version that goes Green MUST be decided by mechanism rather than by a person.

### Publishing Is Governed

Publishing a version MUST be a governed operation.

A narrowing or neutral version MAY publish by default.

A version with a widening change to a mandatory requirement MUST defer to the approver the tenant names.

A version that weakens a requirement MUST NOT be approved by its author.

## The Change Gate

### The Gate Is A Floor, Not An Approval

The gate MUST compare a change's failures against the base commit's, run with the base commit's
configuration.

The gate MUST be able to block a change that adds a failure.

The gate MUST NOT admit a change on the traceability check alone.

### A Touched Requirement Needs Execution Plus More

A mandatory requirement a change touches MUST be discharged by execution plus mutation testing or a
proof.

An exception on a mandatory requirement MUST be treated as a widening change.

### Evidence Is The Checker's, Not The Agent's

The gate MUST decide on an evidence record from a trusted runner rather than on an agent's account.

A checker that has a model inside it MUST be able to block or flag a change.

A checker that has a model inside it MUST NOT admit a change.

Output produced in an agent's workspace MUST NOT count as evidence.

### The Gate Runs In Shadow First

The gate MUST run in shadow, recording what it would decide, before it blocks.

The gate MUST begin blocking only once its shadow record meets the threshold its gate spec states.

## Delegation

### Gate Checking Is Delegated, Not Native

The requirement-gate tool MUST run as a service in a workspace and a component in the collaboration
platform, not as platform state.

The agent platform MUST NOT learn what the requirement-gate tool is.

### An Implementer Cannot Mark Its Own Work Done

A failure item MUST close only from a trusted report, never from a tool an implementer holds.

A tool that writes a specification MUST declare a spec-write call type, so that the hook can deny it to
an implementer.
