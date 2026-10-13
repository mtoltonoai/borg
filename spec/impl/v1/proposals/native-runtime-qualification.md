# Native Runtime Qualification

Status: private technology qualification selected on 2026-10-12; recorded in the
[main specification](../implementation.md#externally-supplied-technologies),
[inventory](../technology-inventory.md#native-runtime-qualification-bundle) and
[decision record](../decisions.md#native-runtime-qualification). Production
choices remain unselected. The scope below is the reviewed qualification packet;
selection is not evidence that acquisition or tests have completed.

## Recommendation

Qualify Tokio 1.53.2 and tokio-postgres 0.7.18 for a single Rust process that owns
the existing model/tool/session sequencing and database recovery. Python remains
external test supervision. This removes the current second orchestration path
without writing an I/O reactor or PostgreSQL protocol implementation.

The first experiment uses only synthetic data, the existing private Unix-socket
PostgreSQL fixture and a synthetic model dependency. It makes no new provider
request and selects no production runtime, backend layout, network TLS, stored
format, encryption scheme or deployment.

| Input | Exact proposed scope |
|---|---|
| Tokio | 1.53.2; default features disabled; rt, net, time, sync, io-util |
| tokio-postgres | 0.7.18; default features disabled; runtime |
| Dependency lock | 83 external identities, SHA256 06e270111b7bdb78a654fce69dbf173b6577c3941906081c25557a666527431b |
| Consumer | qualification/native-runtime in the implementation repository |

The [exact inventory and metadata evidence](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/dependencies.md)
contains 19 identities already in the approved test lock at a new consumption
boundary, two version changes and 62 new package names. Some entries are specific
to other targets. Protocol authentication dependencies remain in the bundle even
though the private fixture uses no TLS; that is not selection of an old password
mode or a Borg content cipher.

The approved Cargo tool generated this lock using registry metadata only and
reproduced it offline. All 83 checksums match official metadata. No candidate
package was acquired, built or executed. The actual compiled target/feature graph
must be checked after approved acquisition; a lock is not evidence of behavior.

## Alternative

An owned Rust FFI adapter over the already approved libpq160015 avoids the new
protocol stack. It adds unsafe lifetime/result ownership, per-connection exclusion,
cancellation/draining and readiness code. A separate blocking worker pool also
adds scheduling and shutdown machinery. Prefer the maintained Rust client for
qualification, while preserving libpq as the serious alternative if the measured
behavior or dependency burden is unacceptable.

## Qualification Acceptance

The [fourteen proposed probes](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/qualification-probes.md)
cover actual application/database restart, a held unresolved writer, current
fences, pinned read versions, notification/catch-up races, cancellation and
independent-session progress. They also check that provider credentials remain
outside core and that the same expectations discriminate correct and deliberately
incorrect Bach/real adapters.

Notification handling must use the client's notification-driving interface;
ordinary connection-future progress alone does not establish watch delivery.
Canceling a future is not proof that a database transaction rolled back. Current
thread execution also does not prove that no helper thread is used. These are
qualification cases, not conclusions from source inspection.

The real encrypted storage path and bounded owner-transfer mapping remain
separate work. A successful plaintext fixture does not establish complete durable
v1 operation, power/media/all-node survival, or full trusted conformance.

## Exact Review Scope

The selected scope covers acquisition, inspected feature resolution and private
qualification of the exact roots and 83-identity lock above, together with the
already approved test stack in its existing scope. Additional identities or
features remain subject to approval before consumption. This does not approve
the entire implementation graph or any production choice.

The [reviewed research packet](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/README.md)
and [completion receipt](/home/dev-user/borg-runtime/artifacts/execution/native-runtime-qualification-peho1xpw/completion.json)
bind original requirement references, source inputs, resolver commands, official
metadata and the independent review. Completion receipt SHA256:
72d0b91e63bc979acb55173f6288925193defbdf3d9c0bf114f1eb80b5599b2c.
