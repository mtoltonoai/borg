# Working in this repository

This file orients any agent (or person) working on the Borg specification. It is agent-agnostic.

## What this repository is

The specification is the durable artifact; the harness that these specs describe is a disposable,
regenerable projection of them, and lives elsewhere. Read [README.md](README.md) for the idea and
[spec/overview.md](spec/overview.md) for the architecture and intent. The non-negotiable invariants are
in [constitution.md](constitution.md).

This repository contains **only** the specification. The implementation that satisfies it is maintained
separately and cites these requirements; it is never the source of truth here.

## The rules that govern editing the specs

1. **Requirements are single, atomic RFC-2119 sentences under stable headings.** Every normative
   statement (MUST / MUST NOT / SHOULD / SHOULD NOT / MAY) is one self-contained sentence carrying
   exactly one obligation, under a heading that does not change casually. This is essential for the
   gate: it identifies a requirement by `(file, section, quoted sentence)`, so a sentence buried in a
   paragraph extracts ambiguously, and rewording one flags every citation that no longer matches.

2. **Stay standalone.** No normative specification names a concrete engine, store, provider, library,
   numeric width, or path. Describe the store as "the durable store", a dependency by its role, a bound
   as "the configured bound". A concrete choice lives in [spec/defaults.md](spec/defaults.md) as a
   declared default, never in a requirement.

3. **Write literally.** No idioms or figurative language in the specs. Name a thing by what it is.

4. **Use the glossary.** Vocabulary comes from [spec/glossary.md](spec/glossary.md); add a term there
   before using it normatively elsewhere.

5. **Descriptive vs. normative.** `spec/overview.md`, `spec/glossary.md`, `spec/defaults.md`, and
   `spec/traceability.md` are descriptive and carry no requirements; they are NOT listed as
   `[[specification]]` in the gate. The constitution, everything under `spec/contracts/`, and everything
   under `spec/capabilities/` are normative.

6. **Contracts change only under discipline.** A change to `spec/contracts/**` is additive with respect
   to deployed parties, or it carries a version increment and a stated migration path, per the
   constitution's Governance Floors.

7. **Traceability is bidirectional.** Every normative document maps to an overview section in
   [spec/traceability.md](spec/traceability.md), and every overview section is served by at least one
   normative document.

8. **A governing requirement binds to the change process.** A requirement that governs how the spec or
   its configuration may change, or that mandates a design discipline rather than a runtime behavior,
   binds to the change-process check rather than to a runtime citation, and is not counted as gating for
   a build's requirement gate.

## The gate

The requirement gate is [duvet](https://github.com/awslabs/duvet), configured in
[.duvet/config.toml](.duvet/config.toml). Three facts to respect:

- **Every `[[specification]]` needs `format = "markdown"`** — the default format extracts nothing from
  this prose.
- **Source paths are repository-root-relative.**
- **A citation is two markers: `//=` then `//#`.** `//= <file>#<section>` names the requirement; `//#
  <exact sentence>` quotes it, and the gate validates the quoted text against the section (a hard error
  if the cited words are gone — this enforces the quoted-sentence identity model).

```sh
# extract requirements from one spec (sanity-check a new file)
duvet extract -f markdown -o ./ ./spec/contracts/tool-call.md

# run the full requirement gate
duvet report
```
