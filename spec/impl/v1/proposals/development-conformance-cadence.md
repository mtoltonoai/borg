# Development Conformance Cadence

Matthew conditionally authorized adding continual Duvet use only if the original
specification did not already require it. This review concerns only that cadence,
not any implementation architecture or library proposal.

The original [decision record](../../../record.md) describes the requirement gate
that every build runs. [AGENTS.md](../../../../AGENTS.md) requires gate execution before
finishing, and [simulation and conformance](../../../core/simulation-and-conformance.md)
requires exact citations, behavior tests, execution witnesses, immutable inputs,
and refusal to promote a build with missing mandatory citation pairs.

Those existing obligations are not duplicated. They do not explicitly require
running Duvet between builds as implementation progresses or reporting actual
Duvet metrics in development progress updates. Matthew's latest instruction
adds that narrower operational cadence.

Exact additional wording, recorded under Matthew's conditional approval:

> Run Duvet during development as implementation progresses, and report the latest actual Duvet metrics in progress updates.

No other prose in this proposal is approved implementation-spec text. The
original normative specification, decision record, and gate selection are
unchanged. Duvet success alone does not establish behavioral conformance or
promotion readiness.
