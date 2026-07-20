# TracePermit

**The provenance gate for enterprise AI agents.** An interactive demonstration and measured evaluation.

> Kill unsupported claims in the sandbox — before they corrupt your system.

TracePermit is a model-neutral governance layer for AI agents operating inside private knowledge systems. It records the sources an agent consumes, traces the evidence behind its claims, and blocks, quarantines, or escalates output with missing, altered, or untrusted provenance.

**Author:** Akhil Reddy Gaddam

## What it does and does not catch

TracePermit detects unsupported claims, missing provenance, unreliable sources, and broken evidence lineage before agent output is admitted.

It does **not** detect falsehood. A fabricated claim that cites a real, unaltered, correctly-hashed internal document passes the gate — structural grounding and semantic support are different problems, and the measured limits of the first are published in the demo's final section rather than omitted. Balanced accuracy against human-annotated hallucination spans is 0.53; that number is on the page.

## What's in this repository

A single self-contained page, `index.html`. No build step, no dependencies, no network calls. The audit console runs real SHA-256 in your browser via the Web Crypto API — the verdicts recompute from the bytes you edit, and nothing is faked.

Five sections: a live audit console you can corrupt, an orchestration demo showing an agent's unsupported output quarantined at the gate, a provenance trace from claim to evidence roots, measured attack-success results across five admission layers, and the published boundary including the negative results.

The Overseer is TracePermit's verdict engine — the component that renders admit/quarantine decisions at the gate. It keeps that name throughout the interface and in the paper.

## Related paper

*Admission Control for AI-Generated Documents: A Content-Addressed Provenance Substrate with an Independent Overseer.* Adversarial evaluation across 10 test batteries, with cross-language auditor agreement over a 75,113-assertion graph.

## Verifying this release

The contents are cryptographically timestamped with [OpenTimestamps](https://opentimestamps.org), anchored to the Bitcoin blockchain. See [PROVENANCE.md](PROVENANCE.md) for digests and verification steps.

Fitting for a project about provenance: don't take the publication date on trust — check it.

## Scope

This is a capability demonstration. The in-browser panel runs standard SHA-256 only. The substrate specification, algorithms, and implementation source are withheld. Results were measured on a research prototype.

© 2026 Akhil Reddy Gaddam. All rights reserved.
