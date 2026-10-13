# Technology Approval Batch

Development/qualification batch approved on 2026-10-12. Current selected uses are
recorded in the main implementation specification and technology-inventory.md.
The following reviewed request retains its original scope and exclusions; other
listed candidates are not adopted. The main specification now requires inventory and Matthew's direct
approval before externally supplied technology is used.

## Selections Already Made

- Bach: individual-component simulation.
- Bolero: generated inputs, property testing and fuzzing.
- Duvet 0.4.3: native requirement extraction, traceability and coverage.
- OpenAI Responses API with gpt-6.1-sol: the current model API choice.
- V1 supplies its key service, integrated or separately deployed. No service
  product, cryptographic profile, custody mechanism or grants are selected yet.

These existing directions need no repeat approval. Exact development versions and
the currently resolved dependency closure are disclosed below; their presence in
a lockfile or successful run is not independent approval.

## Recommended Immediate Scope

Approve the existing **development and qualification stack only**, including the
pinned test dependency closure. This lets us continue the concrete private-engine
and format probes while production storage, durable encodings and key-service
products remain separate choices.

| Technology | Exact observed version | Requested use and consumer boundary |
|---|---|---|
| Rust, Cargo and standard library | rustc1.99.0 b940084d7; cargo1.99.0 5f94df478 | Build the owned contract libraries and isolated integration/qualification drivers. This does not select a production asynchronous runtime. |
| Python and standard library | System Python3.12.3 | Duvet Cap, validation and private test harnesses. Avoid the ambient virtualenv's automatic missing-import installer. |
| LLVM coverage tools and isolated Rust nightly | LLVM23.1.1 with stable compiler; nightly2026-10-11, rustc1.101.0-nightly 6f5da406b, with its matching LLVM tools | Measure owned component code under Bach. Nightly is isolated for branch instrumentation, not a production toolchain selection. |
| Bach and Bolero version/closure pin | Bach0.1.2 at8bddd1958ff66460ea7d0e060bf07b999262aadb; Bolero0.13.4 at71e5de0eceb043f3eda2f62ccd02d2be2b398c44; generator0.13.5 from that revision | Existing approved testing roles, with explicit version pins and transitive dependencies. This does not select every optional fuzz engine or make Tokio the production runtime. |
| PostgreSQL and libpq | PostgreSQL16.15 Ubuntu16.15-0ubuntu0.24.04.1; libpq160015 | Private temporary Unix-socket test servers, real transaction/restart/parity and caller-integration probes. No existing database, production engine/layout selection, service deployment or production tenant data. |
| minicbor | 2.3.0, default features disabled, no transitive dependency in its current experiment lock | Borrowed tagged-format qualification. Existing strict-length wrapper, malformed-input and preservation experiments do not adopt a durable format/profile or production codec. |

The recommendation favors reuse of already exercised mechanisms over developing
our own transaction engine or codec. It does not claim that the experimental
engines or libraries meet every Borg requirement. The alternative is to approve
the test/toolchain pins only and hold PostgreSQL/minicbor use pending separate
reviews; owned logic work can continue, but real-store and codec qualification
would remain held.

## Dependency Detail And Limits

The [generated manifest inventory](../../../../../borg-runtime/artifacts/execution/technology-approval-4gb0dczb/technology-inventory.json)
records each consuming Cargo manifest/lock and exact hashes. The
[complete locked-package list](../../../../../borg-runtime/artifacts/execution/technology-approval-4gb0dczb/locked-packages.md)
has65 unique external package/version/source identities. It includes optional and
target-specific entries; it is not a claim all65 were built or run.

The execution lead's independently collected
[inventory](../../../../../borg-runtime/artifacts/execution/expanded-dispatch-hvvj4nb1/technology-inputs-8yfghivh/inventory.json)
agrees on23 manifests and65 locked identities. A recorded host-filtered receipt
metadata result contains49 external identities; this still does not prove every
optional engine executed. The collaboration integration's
[native dependency closure](../../../../../borg-runtime/integration/collaboration-store/artifacts/dependency-approval-inputs.json)
records45 resolved ELF files for its actual native toolchain/engine dependencies,
with no unresolved file in that inspected closure. System/runtime subdependencies
are part of the relevant technology bundle, not silently treated as Borg code.

These inventories cover the examined development/qualification paths, not every
package installed on the host or a complete future release SBOM. Duvet's installed
binary and Cargo installation manifest are separately hash-pinned by every native
run. No inventory output is a second authored normative requirement catalog.

An earlier ambient Python missing-import hook installed unrelated environment
packages during a failing check. That was not an intentional technology selection
or approval. System Python and explicit import paths now avoid that hook; no
rollback of the shared environment was attempted.

## Choices Excluded From This Batch

- Production PostgreSQL deployment/layout/failure domains and the actual durable
  record profile, algorithms and key hierarchy bindings.
- OpenBao or any other key-service implementation. A self-hosted candidate needs
  backup-restoration/deletion and authority-separation qualification; ordinary
  soft delete or a wrapper refusal is not irreversible destruction.
- Additional model providers, new live-service access, credentials, grants,
  production infrastructure, and unselected optional fuzzing engines.

No new PostgreSQL/minicbor experiment starts while this approval is pending.
Existing evidence remains historical and accurately scoped. Approval of this
batch would be recorded in the implementation specification with its exact
consumer/version boundaries, without promoting experiments into production
choices or changing unrelated declaration status.
