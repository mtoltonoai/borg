# Four-Host Production Target

This descriptive document records the selected first production qualification target, not measured capacity or authorization to deploy. Binding target clauses are in [the main specification](implementation.md#initial-four-host-qualification); selection provenance is in [the decision record](decisions.md#four-host-target-accepted), with the measurement meaning clarified by [the worker-capacity selection](decisions.md#worker-capacity-measurement-selected). The earlier ten-host objective remains a later scale goal and is not qualified by this smaller target. Resource allowances, service-latency objectives and soak durations below remain proposed qualification details.

## Planning Hardware

The operator confirmed four devboxes of the same size as the current box. Local read-only inspection on 2026-10-13 Pacific time found 32 CPU affinity slots, approximately 98.2 GiB physical RAM and a 580.2 GiB filesystem, with approximately 496.2 GiB then available. The visible session/user cgroup ancestors report no additional CPU quota or memory maximum. The other three boxes were not accessed; equal sizing is a supplied assumption. Storage latency, sustained I/O and provider capacity have not been measured.

## Recommended Layout And Workload

| Role | Count | Selected responsibility or qualification target |
|---|---|---|
| Controller | 1 host | SQLite on local storage, task/product API, assignment and status; reserve it from agent builds. No automatic failover. |
| Execution worker | 3 hosts | Eight active Codex coding workspaces per host: 24 cluster-wide. |
| Build/test work | Available in every coding workspace | Direct repository commands under ordinary host/task resource bounds; no separate build-slot limit or admission service. |
| Bounded native model work | Qualification target: 256 per worker | 768 concurrent outstanding real native model requests cluster-wide at the worker client boundary, subject to measured resource capacity and actual provider limits. |
| Source checkpoints | Existing external repository | Immutable pushed Git commits linked to committed task state; no database-backup claim. |

This is a target of 24 concurrently running coding tasks plus 768 outstanding real native model requests, with builds and tests available in every coding workspace. Coding tasks move through model, command, build, test and checkpoint steps, so their provider concurrency is measured separately and shares the applicable provider limits. Report actual outstanding client calls to the provider, active commands, build-waiting tasks and locally queued tasks separately. Stored agent definitions and idle conversations are not active capacity.

The [selected worker resource boundary](decisions.md#worker-wide-resource-group-selected) is one cgroup v2 per worker with generous shared limits. Ordinary startup establishes membership; all execution descendants are covered. Exact ceilings and exhaustion/recovery behavior require real host qualification. Disk quotas remain separate work. This selects no per-task resource manager or additional build-admission queue.

Every coding workspace can invoke repository builds and tests directly. Normal task/host limits bound CPU, memory, disk and output; there is no separate heavy-build permit pool or fixed four-build cap. Configure compiler parallelism to the actual host/workload so simultaneous workspaces do not overcommit it. Exact resource values require mixed-load measurement, not another service or admission mechanism.

Provider rate limits and token budgets are shared by native and coding traffic. If the available provider capacity cannot sustain the selected mixed workload, report that limit and the actual achieved concurrency; locally queued tasks do not substitute for the target. Selecting the target does not grant a provider quota increase or authorize calls.

## Native Capacity And Admission

The native executor design uses bounded asynchronous request state while inference happens at the provider; it does not allocate a compiler, workspace or dedicated operating-system thread per request. This supports testing substantially higher concurrency, but is not a completed implementation or performance result.

Qualify 32, 64, 128 and 256 outstanding real native model requests per worker while the coding and heavy-build workloads also run. A further 512-per-worker step is a stress test only. The selected production target is 256, not a promise to admit that many regardless of memory, provider capacity or cost. Locally queued tasks are not counted as outstanding calls. Count real provider calls from client dispatch to terminal response, failure or cancellation. Admission and allowance reservations alone do not count. Client connection-pool, DNS, TLS, network and provider wait can be included in this interval; the measure establishes worker capacity, not concurrent provider-internal processing. Record successful completion, refusal, failure, cancellation and unavailable observations separately. No artificial request or response delay is part of the workload.

Measure peak request/response buffering, executor memory, CPU used for parsing and validation, open connections, cancellation latency and controller database write/completion latency. Aggregate admission reserves room for Codex work and builds. Bound individual inputs/outputs and all queues; do not obtain a higher concurrency count by discarding required durable state or starving the controller.

Provider request and token limits constrain rate, not concurrency directly. Request latency and token volume connect the quantities. For illustration only, 768 continuously busy requests averaging 30 seconds imply about 25.6 completions per second, or 1,536 per minute. At 5,000 total tokens per request, that is about 7.68 million tokens per minute before Codex traffic. These are hypothetical averages, not measured model performance, an account quota or a spend authorization; actual provider accounting may reserve capacity differently.

Set the eventual operating cap to satisfy every measured local, latency, provider and budget constraint together. No model-provider account or quota was inspected for this proposal. [Official rate-limit guidance](https://developers.openai.com/api/docs/guides/rate-limits) describes request/token limits and their organization/project scope.

## Qualification And Service Objectives

Begin with four coding tasks per worker, allowing each workspace to build, then ramp to eight on the same architecture. The final qualification uses real repository edits, builds, tests and pushed checkpoints with representative native tasks. Start with a one-hour mixed-load run; follow a stable result with a budgeted 24-hour soak before calling this target production-qualified.

Proposed control-plane objectives at that load:

- Accepted task creation and status reads: p95 at most one second, excluding client/provider network latency.
- Dispatch to an available eligible slot: p95 at most two seconds after committed admission. Queue wait under saturation is a separate metric.
- Cancellation acknowledgment: p95 at most two seconds. This does not claim process termination; actual termination obeys its configured deadline and is measured separately.
- No unbounded task/output queues, out-of-memory failures, leaked child processes or unreported resource saturation.
- Controller-process restart recovers committed tasks, memory and results on intact storage.
- Worker loss rejects stale writes and restarts eligible tasks from the last committed external checkpoint after actual termination or inability to continue is established.
- Lost submission acknowledgments are reconciled; settled effects are not repeated; merging cannot trigger an unauthorized rollout.

Capacity and fault qualification are separate. Losing a worker initially reduces admitted capacity from 24 to 16 coding workspaces; no full-capacity-under-failure guarantee is proposed. Controller failure interrupts coordination. Destruction of its only database storage can lose task state because backups remain deferred. Git checkpoints preserve source independently, not database history.

## Relationship To The Previous Scale Objective

Release sequence: qualify this four-host first production target, retaining the earlier ten-host target as a later scale objective on the same architecture. The previous 1,000 active agents and ten workspaces per counted host are not satisfied by this smaller target or by counting queued/idle identities. The later objective's exact active-agent workload definition and larger-scale acceptance still need resolution; its existing requirement sentences remain unchanged in the main specification.

SQLite's single-writer behavior is an early qualification concern, not a reason to claim the application cannot serve remote workers. Workers use the controller API rather than a network-mounted database. See [SQLite application-server guidance](https://www.sqlite.org/whentouse.html) and [WAL constraints](https://www.sqlite.org/wal.html). A failed representative workload result reopens storage/throughput design; no custom replication layer or automatic backend migration is part of this proposal.
