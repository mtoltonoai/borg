# Private Workspace Execution Qualification

This is a proposal for the next gVisor qualification step, not a technology,
deployment, resource-policy or production architecture selection. Native
workspace BUILD and the existing combined scale target remain in force.

## Existing Result

The approved acquisition/static inspection is complete for release
20261005.0, source revision
`97e8e896904cb364319cd9758241d3f0bd27a6f4`. The retained bundle hash is
`bd4eb2ca0ee74f0fbbb5f5273ba011e79c56a29fbb047393b88b981cd3230ede`.
All seven executable members were inspected without executing them. The
[static completion record](/home/dev-user/borg-runtime/qualification/workspace-execution/artifacts/acquisition-static-20261005/completion.json)
retains the exact input, seven safety checks and 121 tooling checks. That
approval does not authorize launching the downloaded programs.

## Concrete Next Step

Qualify the retained `runsc`, `gvisor_sentry` and
`gvisor-sentry-prewarmer` subset with a small owned Rust guest. The proposed
profile uses rootless `run`, systrap and network-none. It has no external
image, provider connection or tenant workload. It preserves the exact
downloaded originals and uses separately verified executable copies for the
private experiment.

The source-backed [execution packet](/home/dev-user/borg-runtime/artifacts/execution/gvisor-private-execution-prep-xp12yg60/README.md)
contains exact argv, the OCI template, owned guest source and build plan,
seven finite acceptance cases, failure handling and cleanup. Its
[request](/home/dev-user/borg-runtime/artifacts/execution/gvisor-private-execution-prep-xp12yg60/request.json)
separates the proposed actions from their prerequisites. No downloaded
program, guest build, namespace probe or host change was performed while
preparing it.

The proposed private experiment uses a separately bounded resource domain
with a 512 MiB memory limit, one CPU and 128 tasks. These are proposed test
bounds, not production capacity defaults. They do not establish the required
ten simultaneous workspaces and one thousand working agents on each of ten
hosts.

## Current Blocker

Read-only inspection found no writable private cgroup in the current executor;
the observed memory, task and CPU maximums are unlimited. Namespace operation
under the current AppArmor and seccomp controls is untested. The packet does
not disable those controls, ignore cgroup failures or substitute host networking.
In particular, empty OCI resource fields do not prove that this release makes
no cgroup access attempt.

Before requesting the actual launch, identify a concrete permitted private
resource domain and finish the bounded supervisor against that domain. The
present packet is not permission to establish delegation, change host-wide
settings, use another host, start a daemon or widen access. No such operation
is hidden in the proposed launch. Other component and simulation work can
continue independently of this missing execution prerequisite.

## Qualification Limits

The first experiment checks the selected program and owned fixture under an
explicit profile. It does not qualify arbitrary tenant builds, per-call
credential isolation, complete descendant termination, controlled egress,
durable content snapshots or runtime self-delivery. Those remain the
source-backed cases in the [existing workspace qualification proposal](workspace-execution-qualification.md#contract-the-execution-profile-must-demonstrate).
Rootless namespace-root is not represented as a nonroot production workspace.

The source definition of a platform node remains a platform process, not a
physical host. Core and workspace isolation on the same counted hosts does not
permit command admission into the core or exposure of its credentials. This
proposal selects no process, sandbox-per-call, host placement or credential
topology for production.

## Evidence

The [preparation completion](/home/dev-user/borg-runtime/artifacts/execution/gvisor-private-execution-prep-xp12yg60/completion.json)
has SHA256
`c3870843796baed1e3f8dd8c27dceb58cec2b2de52dec9826dab8601483168b6`.
It binds the exact member identities, official pinned source, host observations,
profiles and independent source/configuration review. The original
[static-only scope](workspace-execution-qualification.md) and
[technology inventory](../technology-inventory.md#workspace-sandbox-static-inventory)
are unchanged. No execution approval is recorded by this proposal.
