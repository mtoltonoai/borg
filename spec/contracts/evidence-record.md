# Contract — The Evidence Record

> **CONTRACT.** This document pins the record a checker produces about a change, which a gate spec reads
> to decide whether the change may proceed. It is the shape that lets confidence come from mechanism
> rather than from a click, and it is honored across releases by every checker and every gate. Its
> requirements realize [Core Principle VII](../../constitution.md) and [Governance Floor "Agents Never
> Approve"](../../constitution.md) and trace to [overview §8](../overview.md) and
> [overview §20](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence under a stable
> heading.

## Purpose And Scope

Agents game checkers: hidden assumptions, invented axioms, tests that never run the code. So evidence is
the checker's own record, produced by a trusted runner outside the agent's reach, tied to the exact
artifact it judged, and reproducible. A gate decides on the record and never on an agent's account of it.

## Provenance

### Evidence Is The Checker's Own Record

An evidence record MUST be produced by the checker that ran, rather than by the agent whose work it
judges.

An evidence record MUST be signed by the principal of the runner that executed the checker.

An evidence record produced in an agent's workspace MUST NOT count as evidence for a gate.

## Fields

### The Record Names Its Subject

An evidence record MUST name the subject it judged by the digest of the artifact and of its change.

### The Record Names The Property

An evidence record MUST name the property it checked, by the digest of the specification pin and the
version of the gate spec.

### The Record Names The Checker

An evidence record MUST name the checker, its version, and the digest of the configuration it ran with.

### The Record Carries A Result Of A Closed Set

An evidence record's result MUST be exactly one of pass, fail, or unknown.

A fail MUST name the failures it found.

An unknown result MUST NOT allow a change.

### The Record Names Its Scope And Strength

An evidence record MUST name the requirements and the files its check covered.

An evidence record MUST state, per requirement it covers, the strength of its evidence as one of
presence, executed, mutation-checked, or proved.

### The Record Is Reproducible

An evidence record MUST carry what is needed to reproduce it: the commits and the toolchain pins.

Re-running an evidence record's reproduction MUST yield the same result and the same failure ids.

## Use

### A Checker With A Model Inside It Cannot Admit

An evidence record from a checker that has a model inside it MUST be used only to block or to flag,
never to admit.

### The Record Is An Event

An evidence record MUST be published as an event the gate's hook can read.

## Additive Evolution

### Additive Evolution Of This Contract

A change to this contract MUST be additive with respect to deployed gates, or else carry an explicit
version increment.

A change to this contract that is not additive with respect to deployed gates MUST carry a stated
migration path.
