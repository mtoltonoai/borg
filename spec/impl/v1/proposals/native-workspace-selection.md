# Native Workspace Service Selection

Status: native BUILD selected on 2026-10-12 and recorded in the main implementation
specification and shared v1 evaluation. The reviewed packet below remains the
selection's rationale and scope. Outcome selection does not establish an
implemented workspace service or choose its hosting technology.

## Recommendation

Select the existing native workspace service outcome, BUILD. It provides governed
file editing, command/build execution and recoverable workspace contents, using
Borg's shared durability, blobs, events, encryption, identity and call guards.
This fits the required hundred concurrent workspaces and a working cluster that
prepares its own runtime changes.

The alternative is to place an existing operated runner service behind Borg's tool
interface. It is useful if such a service is available and its recovery model is
acceptable. No specific existing runner has been supplied for this implementation;
this is not a claim that none exists. Native BUILD owns more behavior, but gives
the required recovery, identity and snapshot contracts one explicit home.

## What Selection Includes

The full [existing workspace contract](../../../decisions/workspace-service/build.md)
applies: 150 native requirements, including 148 absolute requirements and two
optional permissions. This is not just shell access.

- Session-bound expiring leases, incarnation and writer fencing.
- File operations and builds; durable snapshots after changing calls; verified
  restore, fork and rollback; recovery after host loss.
- Explicit command endings, deadlines, cancellation and unknown outcomes.
- Per-call credentials, isolation, domain checks and controlled egress.
- Host health/promise records, workspace-free tools, stored values and observation.

Workspace leases belong to the service; they are not session ownership leases or
new collaboration task leases. Shared primitives are reused rather than copied
into another storage engine or agent-state subsystem.

The selected v1 capacity remains ten simultaneous workspaces per counted host,
alongside one thousand agents on each of ten simultaneous hosts. The shared
outcome-bound default of four workspaces is retained for other implementations;
v1's explicit target overrides it.

## Exact Prepared Change

The [reviewed activation patch](/home/dev-user/borg-runtime/artifacts/planning/workspace-build-proposal-e1nzcicj/activation.patch)
changes only the shared evaluation selection, its decision record, deferred-status
description and overview. It activates BUILD, leaves existing runners inactive,
and reconciles the record/configuration inventory to 35 active and 12 commented
files. It does not rewrite any of the existing 150 requirement sentences.

Patch SHA256: fc4faa28e046600fdc1901622beb505ea34d5b5217a91a6f1fd0961e4a6531ab.

The [prepared v1 section](/home/dev-user/borg-runtime/artifacts/planning/workspace-build-proposal-e1nzcicj/proposed-v1-workspace-choice.md)
is now saved in the implementation specification. Pending workspace-outcome
descriptions are reconciled; unresolved host and delivery-interface choices remain
explicit. No foundation capability or milestone is approved merely by this outcome
selection.

## Boundaries Still To Design And Qualify

Selection does not choose a sandbox/container/VM product, host substrate, store,
codec, repository provider, resource profile or deployment service. Those choices
must support the production architecture and remain subject to the current
technology-approval rule. There is no permission to provision, deploy or grant
access merely from preparing this proposal.

A concrete topology candidate is a platform-node isolation domain and separate
workspace-executor isolation domains on each counted host. This needs a reviewed
resource and security mapping; it does not relax the node's process/command
boundary, expose node credentials or add uncounted hosts.

Agents may edit, compile, test and submit changes automatically. Workspace test
results remain development evidence. A protected checker outside agent-writable
workspaces produces the release evidence, and only final rollout needs a
per-change human approval in Borg's own deployment workflow.

## Actual Counterfactual Validation

Installed Duvet0.4.3 was run in isolated artifact copies using unchanged canonical
declarations. The [receipt](/home/dev-user/borg-runtime/artifacts/planning/workspace-build-proposal-e1nzcicj/receipt.json)
records inputs, commands and raw output hashes.

| Selection | Native planning associations | Missing | Partial / unmatched | Normal / strict exits |
|---|---:|---:|---:|---:|
| Actual current, 34 files | 1955 / 1955 | 0 | 0 / 0 | 0 / 0 |
| Candidate BUILD, 35 files | 1955 / 2105 | 150 | 0 / 0 | 0 / 1 |

All 150 added identities come from the unchanged BUILD source. They are not
silently assigned to an existing capability merely to raise coverage. A meaningful
workspace capability, phased producer interfaces and acceptance allocations still
need integration into the plan. The candidate snapshot is
5240674db8da3a7e50a76279e09df5620270bc1f549004b77090705c40c2532c.

These rows describe the preserved pre-activation experiment, not a fresh report of
the later saved selection. The actual evaluation now selects BUILD. The copied
runtime snapshot/lock, missing gate baseline and historical reports remain
unchanged. This historical counterfactual report is not relabeled as activation or
an ordinary successful lock-bound planning run.
