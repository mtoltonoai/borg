# Traceability

> **What this document is.** The bidirectional map between the normative specifications and the
> architecture they realize. Every normative document traces to a section of
> [overview.md](./overview.md), the intent arbiter, and every overview section is served by at least one
> normative document. This document is descriptive; it carries no requirements. It exists so that a
> change to intent and a change to a requirement can each be checked against the other.

## Normative document → overview sections it realizes

| Normative document | Realizes overview sections |
|---|---|
| `constitution.md` | §1, §2, §3, §4, §7, §8, §9, §10, §11, §13, §15, §16, §17, §22 |
| `spec/contracts/signed-assertion.md` | §9, §7 |
| `spec/contracts/definition-source.md` | §12 |
| `spec/contracts/decision-hook.md` | §8 |
| `spec/contracts/event-hook.md` | §14 |
| `spec/contracts/event-envelope.md` | §4 |
| `spec/contracts/tool-call.md` | §7 |
| `spec/contracts/usage-record.md` | §15 |
| `spec/contracts/evidence-record.md` | §8, §20 |
| `spec/contracts/stored-form.md` | §11 |
| `spec/capabilities/sessions-and-partitions.md` | §3, §4 |
| `spec/capabilities/events-and-subscriptions.md` | §4 |
| `spec/capabilities/failure-handling.md` | §16 |
| `spec/capabilities/model-turns-and-scheduling.md` | §5 |
| `spec/capabilities/context-compaction-and-cache.md` | §5, §6 |
| `spec/capabilities/tools-and-dispatch.md` | §7 |
| `spec/capabilities/governance.md` | §8 |
| `spec/capabilities/identity-delegation-and-secrets.md` | §9 |
| `spec/capabilities/tenancy-and-confidentiality.md` | §10 |
| `spec/capabilities/encryption-and-blobs.md` | §11 |
| `spec/capabilities/definitions-and-sources.md` | §12 |
| `spec/capabilities/timers.md` | §13 |
| `spec/capabilities/the-frontend.md` | §14 |
| `spec/capabilities/observation-and-usage.md` | §15 |
| `spec/capabilities/durability-and-deployment.md` | §16 |
| `spec/capabilities/simulation-and-conformance.md` | §17 |
| `spec/capabilities/collaboration-platform.md` | §18 |
| `spec/capabilities/workspace-service.md` | §19 |
| `spec/capabilities/spec-driven-design.md` | §20 |
| `spec/capabilities/shared-framework.md` | §21 |

## Overview section → normative documents that serve it

| Overview section | Served by |
|---|---|
| §1 The one idea | constitution.md |
| §2 Why the platform exists | constitution.md |
| §3 Sessions, partitions, and the durable store | sessions-and-partitions.md; constitution.md |
| §4 Events, inboxes, and subscriptions | events-and-subscriptions.md; sessions-and-partitions.md; event-envelope.md; constitution.md |
| §5 Model turns, scheduling, and the prompt cache | model-turns-and-scheduling.md; context-compaction-and-cache.md |
| §6 Context: transcripts, compaction, and freezing | context-compaction-and-cache.md |
| §7 Tools are services | tools-and-dispatch.md; tool-call.md; signed-assertion.md; constitution.md |
| §8 Governance is one mechanism | governance.md; decision-hook.md; evidence-record.md; constitution.md |
| §9 Identity, delegation, and secrets | identity-delegation-and-secrets.md; signed-assertion.md; constitution.md |
| §10 Tenancy and confidentiality | tenancy-and-confidentiality.md; constitution.md |
| §11 Encryption at rest and the blob store | encryption-and-blobs.md; stored-form.md; constitution.md |
| §12 Definitions come from sources | definitions-and-sources.md; definition-source.md |
| §13 Timers | timers.md; constitution.md |
| §14 The frontend | the-frontend.md; event-hook.md |
| §15 Observation, usage, and the improvement loop | observation-and-usage.md; usage-record.md; constitution.md |
| §16 Failure handling, durability, and deployment | failure-handling.md; durability-and-deployment.md; constitution.md |
| §17 Deterministic simulation and conformance | simulation-and-conformance.md; constitution.md |
| §18 The collaboration platform | collaboration-platform.md |
| §19 The workspace service | workspace-service.md |
| §20 Spec-driven design | spec-driven-design.md; evidence-record.md |
| §21 Shared framework components | shared-framework.md |
| §22 What is deliberately left open | constitution.md |
| §23 What earlier designs taught | (descriptive; served by the project's design history) |
