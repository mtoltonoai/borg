# Workspace Execution Qualification

Status: the exact acquisition/static-inventory scope is selected in the
[technology inventory](../technology-inventory.md#workspace-sandbox-static-inventory).
The execution/isolation profile remains proposed. Native workspace BUILD is
already selected; this does not reopen that outcome, tenancy or direct execution.
The current plan allocates all 150 BUILD requirements, but a running isolated
workspace is not established by those associations or by metadata adapter tests.

## Recommendation And Immediate Scope

Qualify gVisor/runsc first as the external execution boundary behind a Borg-owned
Rust workspace controller. Its userspace application kernel reduces the surface
of the host kernel reached by workspace programs. Compatibility and overhead are
qualification questions, not assumed results. [Architecture](https://gvisor.dev/docs/architecture_guide/intro/)

The selected immediate technology scope is acquisition and static inventory
of the exact official bundle below. It does not authorize sandbox execution,
privilege or host-configuration changes, a new image, Docker/containerd operation
or production adoption. This narrower first step is necessary because the archive
and its sidecar/input closure have not yet been inspected, and the current host's
ability to enforce a complete profile is unproven. It is not a request to accept
an inadequately isolated rootless configuration.

| Candidate | Benefit | Cost or unresolved constraint |
| --- | --- | --- |
| gVisor/runsc: recommended first qualification | Maintained application-kernel boundary and OCI interface; no need to adopt Docker or Kubernetes merely to use it. | Linux compatibility, filesystem/syscall overhead, complete launch privileges and per-call control need measurement. |
| Native OCI/runc | Broad native Linux compatibility and a mature low-level runtime. | Workspace code reaches the shared host kernel; the namespace, privilege, syscall and resource profile still needs explicit qualification. An installed binary is not approval. |
| Firecracker microVMs | A separate guest kernel behind a KVM boundary. | Adds a guest kernel/root filesystem and lifecycle/network/filesystem integration; this host has no exposed KVM device. |

Primary references: [gVisor](https://gvisor.dev/docs/),
[runc](https://github.com/opencontainers/runc),
[Firecracker](https://firecracker-microvm.github.io/).
No candidate is claimed to meet Borg's combined capacity target from vendor data.

## Exact Candidate Input

| Field | Value |
| --- | --- |
| Release | gVisor 20261005.0, tag release-20261005.0 |
| Source revision | 97e8e896904cb364319cd9758241d3f0bd27a6f4 |
| Official bundle | gvisor-x86_64.tar.bz2 |
| Published SHA256 | bd4eb2ca0ee74f0fbbb5f5273ba011e79c56a29fbb047393b88b981cd3230ede |
| Proposed consumption site | qualification/workspace-execution, initially static acquisition/inventory only |

[Official release](https://github.com/google/gvisor/releases/tag/release-20261005.0),
[bundle](https://github.com/google/gvisor/releases/download/release-20261005.0/gvisor-x86_64.tar.bz2),
[published checksums](https://github.com/google/gvisor/releases/download/release-20261005.0/SHA256SUMS).
The checksum text and GitHub asset digest agree. The executable bundle has not
been downloaded by this preparation.

The distribution is multi-file: runsc, required gvisor-bin sidecars and other
bundled binaries, including the containerd shim and metric server. The initial
inventory covers the whole archive; it does not select every contained binary
for execution. In particular, a containerd shim in the archive does not authorize
starting containerd or connecting to its daemon. Exact member identities, safe
paths, modes, linked inputs and the intended active subset are recorded before
an execution proposal is finalized.

The pinned publisher workflow describes an internal Buildkite build and
SHA512-verified mirroring from GCS to GitHub. This is distribution provenance,
not independently verified reproducibility or binary-to-source attestation.
The documented GitHub route avoids relying on GCS access.
[Pinned mirror workflow](https://github.com/google/gvisor/blob/97e8e896904cb364319cd9758241d3f0bd27a6f4/.github/workflows/release_mirror.yml)

## Existing Host Facts And Limits

Read-only inspection found Linux6.8.0-106-generic on x86_64, uid1000, no runsc or
Firecracker executable and no /dev/kvm. runc and Docker paths exist; neither was
executed or adopted by this inspection. The installed runc binary is supplied by
containerd.io2.4.1-2~ubuntu.24.04~noble; its embedded metadata identifies runc1.5.2
at revision29dd3dc2b13b4123162e5fe132504bb4b15569f1, built with Go1.27.2.
Its SHA256 is 513bdc1b1dae584fca6a37b5abeb0cefb30211d18aab48db0c0dd46c6a8d80dc.

User-namespace controls are numerically enabled, but the AppArmor restriction on
unprivileged user namespaces is also enabled. Cgroup controllers are present;
usable delegation and effective launch permissions have not been demonstrated.
No namespace, KVM, cgroup, daemon or sandbox probe ran, and no configuration was
changed. Tool presence and file permissions are not evidence of a usable profile.

The pinned rootless documentation lists missing create, runtime save/restore and
Netstack functionality, ignored cgroup-configuration errors and host networking
for external connectivity. That simple mode is not a complete Borg isolation
profile. Runtime checkpoint support is distinct from Borg's required durable
content snapshots; absence of the former alone does not reject an otherwise
conforming content-snapshot design.
[Pinned rootless documentation](https://github.com/google/gvisor/blob/97e8e896904cb364319cd9758241d3f0bd27a6f4/g3doc/user_guide/rootless.md)

## Contract The Execution Profile Must Demonstrate

The [node definition](../../../glossary.md#the-platform-and-its-parties) identifies a platform
process on a host. The [tool boundary](../../../../constitution.md#tools-execute-through-governed-interfaces)
does not prescribe a process or machine boundary, while the
[node restrictions](../../../core/durability-and-deployment.md#a-node-runs-only-the-platforms-process)
still forbid command admission into the platform node and exposure of host
metadata or node credentials. Distinct core and workspace isolation domains may
share a counted physical host; this is not permission to weaken those rules.

| Observable qualification | Required evidence and failure cases |
| --- | --- |
| Per-call isolation inside one workspace | Two overlapping calls cannot obtain or use each other's credentials; credential-bearing processes and control endpoints enforce call ownership. Ending a call terminates all its descendants, including after supervisor/host-agent restart. One sandbox per workspace is not sufficient evidence by itself. |
| Local stop and deadline enforcement | An unrenewed host lease or call deadline stops work locally during a network partition. Supervisor loss produces a verified stopped outcome. A missed heartbeat or disconnected control channel is never presented as proof of termination. |
| Filesystem/process confinement | Only the read-only system/toolchain, the call's own workspace and paths declared visible at acquire are exposed; other workspaces' processes, node credentials and host metadata are inaccessible. Test symlinks, inherited handles, process inspection and attempted namespace escape. |
| Controlled credentials and egress | All egress uses the service-controlled point; credentials remain per-call, scoped and unavailable after termination. No credential or endpoint token enters logs, saved output, helpers' files, environment or command line. |
| Snapshot and recovery boundary | Capture every in-root file regardless of modes without following paths or handles outside the root. Hand restore/rollback over through a protected surface. Runtime/container checkpoints do not substitute for Borg blob ownership, fenced lineage-head commits or host-loss recovery. |
| Actual runtime self-delivery | Edit and compile the real Borg Rust code/toolchain, including hostile repository build scripts. Keep workspace feedback separate from protected checker evidence; only the final rollout has a per-change human gate. |
| Combined host capacity | Exercise ten active workspaces alongside one thousand working agents on each of ten hosts in the same interval. CPU, memory, process, output and storage pressure cannot defeat core/peer isolation. Smaller fixtures are preliminary evidence, not this acceptance. |

These cases derive from [workspace deadlines](../../../decisions/workspace-service/build.md#operations),
[per-call credentials](../../../decisions/workspace-service/build.md#credentials-are-per-call),
[workspace isolation](../../../decisions/workspace-service/build.md#a-workspace-is-isolated-and-bounded),
[v1 scale](../implementation.md#simultaneous-production-scale) and
[self-delivery](../implementation.md#runtime-self-delivery).

The eventual profile still needs a concrete confined filesystem/toolchain image,
per-call supervision/credential boundary, controlled networking, enforced resource
limits and an environment capable of hosting them. No numeric production budget,
privileged launcher, sandbox-per-call versus nested-control design, host placement
or new third-party image is selected here. Any required access/configuration
change remains a distinct, concrete action for review; it is not smuggled into
technology acquisition.

## Evidence And Independent Work

[Research receipt](/home/dev-user/borg-runtime/artifacts/execution/workspace-runsc-research-dqst70xm/research-receipt.json),
SHA256 37146ed6f9ca3d0eaa64ccb9a85f8642f36890a0d48e26189b2cef8e6db4c78d,
binds exact primary source and installed-binary metadata. Checksums and source
documentation were inspected; no sandbox execution or capacity measurement is
claimed.

The selected BUILD metadata, lease/promise models, durable command adapters and
snapshot/key interfaces can continue independently. They remain subject to their
own real-producer and Bach evidence. This proposal adds neither an independent
agent-state store nor a new storage engine or collaboration task lease.
