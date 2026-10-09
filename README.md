# borg

Borg contains Matthew Tolton's personal ideas on orchestration specification.

The specification of **Borg**, a multi-tenant harness that runs AI agents at scale: a core that
executes sessions, delivers events, calls models, dispatches tools, and governs them at fixed hook
points, while everything else, prompts, tools, policies, and the agents themselves, is configuration a
tenant supplies.

This repository holds **requirements, not an implementation**. It describes the shape of a valid
solution, independent of any one build. An implementation lives elsewhere and is a projection of these
requirements that cites them. It is written to be read and extended by agents and humans alike, and follows a clean-room, specification-first model:
the specification is the durable artifact; the implementation is a regenerable projection of it.

## The documents

- **[`constitution.md`](constitution.md)** — the non-negotiable invariants, as normative requirements:
  the general tenets (Core Principles), the platform principles (P1–P7), the platform's boundaries, and
  the governance floors.
- **[`spec/overview.md`](spec/overview.md)** — the target architecture and intent, which every
  requirement traces to. Descriptive.
- **[`spec/glossary.md`](spec/glossary.md)** — the controlled vocabulary. Descriptive.
- **[`spec/defaults.md`](spec/defaults.md)** — the concrete default choices a requirement deliberately
  leaves open (categories and open standards, not a vendor). Descriptive.
- **[`spec/contracts/`](spec/contracts)** — the interfaces independently deployed parties honor across
  versions: the signed assertion, the definition source, the decision hook, event hooks out, the event
  envelope, the tool call, the usage record, the evidence record, and the stored form. A change to one is
  additive, or carries a version increment and a migration path.
- **[`spec/capabilities/`](spec/capabilities)** — the behavior of the platform and of the products beside
  it (the collaboration platform, the workspace service, and spec-driven design), free of implementation
  detail, one document per area.
- **[`spec/traceability.md`](spec/traceability.md)** — the bidirectional map between the normative
  documents and the overview sections they realize. Descriptive.

## The requirement model

Every normative statement is a single RFC-2119 sentence (MUST / MUST NOT / SHOULD / SHOULD NOT / MAY)
under a stable heading, carrying exactly one obligation. A requirement's identity is the tuple
`(file, section, quoted sentence)`; there are no separate ids, so editing a sentence flags every citation
that no longer matches it. The descriptive files carry no requirements and are not part of the gate. See
[`AGENTS.md`](AGENTS.md) for the rules that govern editing the specs.

## The conformance gate

The gate is [duvet](https://github.com/awslabs/duvet), configured in
[`.duvet/config.toml`](.duvet/config.toml). The **requirement side** lists the normative files
(hand-owned, `format = "markdown"`); the **source side** is the globs of whatever implementation cites
the requirements. A citation is two markers in that code: `//= <file>#<section>` names the requirement and
`//# <exact sentence>` quotes it, and the gate validates the quoted text against the section, so a
reworded requirement invalidates every stale citation. A requirement is covered only when it has both an
implementation citation and a test citation that exercises the behavior.

```sh
# extract requirements from one spec (sanity-check a file)
duvet extract -f markdown -o ./ ./spec/capabilities/governance.md

# run the full requirement gate
duvet report
```

## Status

Clean-room specification, in authoring. Thirty normative files carry about 1,150 single-obligation
RFC-2119 requirements; the gate extracts them with no parse failures. The implementation is a projection
that cites them.

## License

Licensed under the [MIT License](LICENSE). Copyright (c) 2026 Matthew Tolton.

## Acknowledgments

Thanks to Cameron Bytheway for many fruitful discussions that helped shape these ideas.
