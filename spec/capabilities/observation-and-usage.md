# Capability — Observation And Usage

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines what the platform records about its own behavior: a usage record per unit of work, observation
> records that point at transcript ranges, and the loop by which those outputs feed back as
> configuration. Requirements realize [Platform Principle P7](../../constitution.md) and
> [Core Principle III](../../constitution.md) and trace to [overview §15](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. The usage record's fields are pinned by the
> [usage-record contract](../contracts/usage-record.md); the observation triggers and queues are declared
> defaults.

## Purpose And Scope

The platform's own outputs are data. This capability fixes the usage stream that measures cost against
cause, the observation records that let a reader see what a session did without copying its transcript,
and the loop that turns both into configuration changes.

## Usage

### One Record Per Unit Of Work

Exactly one usage record MUST exist per model attempt, tool call, and decider call.

A usage record MUST carry the fields the usage-record contract pins.

A usage record MUST NOT carry transcript content.

### Versions Compare On One Stream

A model attempt's record MUST name the definition digest the session ran, so that versions compare on
the same stream.

Budget deciders MUST read the usage stream.

Metering MUST read the same usage stream rather than a separate one.

## Observation

### A Session Publishes Observation Records

A session MUST publish an observation record to a queue when it crosses a size threshold, and on
spin-down, compaction, or request.

An observation record MUST name the session and identity, the reason, the transcript range, the size in
bytes and tokens, the model and configuration version, and timestamps.

An observation record MUST point at the transcript rather than copy it.

An observation record MUST point at the same frozen blobs the transcript ranges name.

### Reading An Observed Range Is Granted And Audited

A principal reading a range an observation record names MUST hold a read grant on the session.

Every read of an observed range MUST be audited.

A reader MUST pass the access-scope check for the range it reads.

### Triggers And Queues Are Configuration

The triggers for an observation record and its target queue MUST be configuration.

## The Improvement Loop

### Improvement Runs Through The Platform's Own Primitives

An observer MUST propose a change as a new definition version through the source's change control rather
than by writing configuration directly.

A proposed change MUST run on a cohort against its parent before it rolls out.

The signals that judge a change MUST come from the usage and observation streams.
