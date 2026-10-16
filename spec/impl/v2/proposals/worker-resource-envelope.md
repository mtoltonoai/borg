# Worker Resource Envelope

The [worker-wide cgroup direction is selected](../decisions.md#worker-wide-resource-group-selected) for the existing [resource-bound requirements](../implementation.md#workers-are-trusted-disposable-environments). Numeric limits, filesystem/quota details and host setup remain to establish; this is not a claim that finite host controls are configured or the [four-host target](../production-target.md) is qualified. No quota backend, host tool, account change or provisioning action is selected here.

The [worker verification implementation](../../../../../borg-runtime/artifacts/v2/build-60156282/worker-envelope-implementation-e42rg9kw/completion.json) adds a read-only operator profile and startup/admission checks. Its local parser, genuine-mount refusal and cleanup tests do not configure or positively qualify finite host limits. The [operator interface](../protocol.md#worker-resource-profile) defines its fields; the [runtime guide](../../../../../borg-runtime/docs/v2-worker-resources.md) explains use and evidence limits.

## Selected Worker Boundary

Use one operator-provided Linux cgroup v2 envelope for each worker and its execution descendants, with finite CPU bandwidth, memory/swap and process limits. Ordinary startup configuration places the worker in its group before child execution. Choose generous limits with room for concurrent builds, control operations and cleanup, then measure the actual workload. Retain existing task deadlines, output/result bounds, model accounting and executor concurrency. Native HTTP requests stay asynchronous inside the bounded worker; no process or resource manager is added for each request.

Disk capacity and inode enforcement are separate work. Account-wide hard quotas across writable persistent filesystems remain the recommendation to evaluate; no quota backend, coverage claim or journal-reserve mechanism is selected by accepting the cgroup direction.

Each attempt shares the worker envelope. This gives no exclusive reservation, per-task resource attribution, fairness or hostile-workload isolation. One task can exhaust shared capacity and affect peers. All eight coding workspaces can still build concurrently; there is no separate build-permit queue. CPU bandwidth is a rate limit, not a lifetime CPU budget.

An explicitly configured profile is checked before advertising capacity or admitting work. Missing, inconsistent or unverifiable configured controls refuse without falling back to an unbounded profile. Absence of a profile keeps the current development behavior but establishes no hard-resource qualification. Successful production qualification needs actual configured controls and measured mixed-load results.

## Coverage And Recovery

The envelope covers the worker, Git, Codex, actual command-runner descendants, builds, acceptance commands and native validators. Storage coverage includes source and validation checkouts, private home and executor state, temporary directories, writable caches, build targets and journals. Shared files are charged once. Environment variables alone cannot stop commands writing to other account-writable paths; a workspace directory limit is not account-wide enforcement.

Existing attempt/process ownership and cleanup remain the stop proof. An occupied shared cgroup is not proof that a particular attempt is still running, and stopping one attempt does not justify killing its peers. Profile associations survive reconnect without resetting deadlines or budget accounting. Local cleanup does not settle unknown remote effects.

Admission headroom is not a hard journal reservation against concurrent writes. Qualification needs a concrete outcome for quota exhaustion during ownership/report writes. A protected or preallocated reserve may be needed; no reserve mechanism is selected here.

## Current Evidence And Remaining Inputs

The [read-only preparation receipt](../../../../../borg-runtime/artifacts/v2/build-60156282/worker/resource-profile-e1rpcoe6/review.json) binds the inspected sources and host observations. That namespace had read-only cgroup controls, no usable finite CPU/memory envelope, and ext4 storage without verified account quota. This does not establish the privileges or setup of other host environments.

Before integration can claim actual enforcement, identify:

1. The existing finite worker envelope and account/storage scope, or authorization for a separate host-setup step.
2. A real read-only quota interface for the actual filesystem. Filesystem capacity and directory usage reports do not prove an account quota. No new query tool is selected.
3. Numeric allowances and journal-exhaustion behavior, followed by actual CPU/memory/process/storage failure tests and mixed-load measurement.

Local profile/parser and injected-failure tests cannot replace kernel enforcement tests. The pinned executor's real command-runner descendants also need a zero-model inheritance probe on the configured host. Per-task reservations, per-slot cgroups, dynamic balancing and a custom resource manager are outside the selected initial approach. The [historical preparation analysis](../../../../../borg-runtime/artifacts/v2/build-60156282/worker/resource-profile-e1rpcoe6/PROPOSAL.md) records earlier candidate seams and tests, without establishing current implementation or host qualification.

Control semantics follow the [Linux cgroup documentation](https://docs.kernel.org/admin-guide/cgroup-v2.html); the separate quota investigation uses the [filesystem quota documentation](https://www.kernel.org/doc/html/latest/filesystems/quota.html). Selecting the worker boundary does not establish actual configured controls or authorize unrelated access changes.
