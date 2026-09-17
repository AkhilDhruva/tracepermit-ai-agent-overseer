# TracePermit

**The provenance gate for enterprise AI agents.** An interactive demonstration
and measured evaluation.

> Kill unsupported claims in the sandbox — before they corrupt your system.

[![integrity](https://github.com/AkhilDhruva/tracepermit-ai-agent-overseer/actions/workflows/ci.yml/badge.svg)](https://github.com/AkhilDhruva/tracepermit-ai-agent-overseer/actions/workflows/ci.yml)
![release](https://img.shields.io/badge/release-v1.1.0-blue)
![timestamped](https://img.shields.io/badge/OpenTimestamps-Bitcoin%20anchored-orange)

**Author:** Akhil Reddy Gaddam

## Run it

```bash
git clone https://github.com/AkhilDhruva/tracepermit-ai-agent-overseer
open index.html          # or just drag it into a browser
```

No build step, no dependencies, no server, no network calls. Corrupt anything
in the evidence table and the verdict recomputes from the new bytes —
the audit console runs **real SHA-256 in your browser** via the Web Crypto API.
Nothing is a lookup table and nothing is faked.

→ **[How it works](docs/ARCHITECTURE.md)** — the gate, the finding classes, the
measured results, and the published boundary.

## What it does

TracePermit is a model-neutral governance layer for AI agents operating inside
private knowledge systems. It records the sources an agent consumes, traces the
evidence behind its claims, and blocks, quarantines, or escalates output with
missing, altered, or untrusted provenance.

The Overseer is TracePermit's verdict engine — the component that renders
admit/quarantine decisions at the gate. It keeps that name throughout the
interface and in the paper.

## What it does *not* catch

It does **not** detect falsehood. A fabricated claim that cites a real,
unaltered, correctly-hashed internal document passes the gate — structural
grounding and semantic support are different problems, and the measured limits
of the first are published in the demo's final section rather than omitted.
Balanced accuracy against human-annotated hallucination spans is **0.53**; that
number is on the page.

| | |
|---|---|
| **Enforced** | identity · ledger · closure · output binding · replay |
| **Conditional** | origin completeness — requires a truthful gateway |
| **Not claimed** | truth · authority · confidentiality · semantic support |

## The result that matters

Five admission layers receive identical generator output, so only the
governance layer varies and the difference is causal. 240 cases, 180 attacks
across six classes; Wilson 95% intervals; zero false alarms in every arm.

| Admission layer | Attack success |
|---|---|
| Citations only, no substrate | 100% |
| **Provenance logging, no gate** | **100%** |
| Overseer admission gate | 33.3% |
| + truthful origin gateway | 0.6% |
| + colluding gateway | 17.2% |

Read the second row first. Record-keeping without a gate blocks nothing at all.
The gate causes the protection — not the audit trail.

## What's in this repository

A single self-contained page, `index.html`, plus its provenance and the CI that
enforces it. Five sections: a live audit console you can corrupt, an
orchestration demo showing an agent's unsupported output quarantined at the
gate, a provenance trace from claim to evidence roots, measured attack-success
results across five admission layers, and the published boundary including the
negative results.

## Verifying this release

The contents are cryptographically timestamped with
[OpenTimestamps](https://opentimestamps.org), anchored to the Bitcoin
blockchain:

```bash
sha256sum -c SHA256SUMS      # every published byte re-derives
ots verify index.html.ots    # when these bytes existed
```

See [PROVENANCE.md](PROVENANCE.md) for digests, the release history, and the
calendar-versus-block-header distinction. CI re-runs the digest check on every
push, and re-verifies the attested bytes at each release tag.

Fitting for a project about provenance: don't take the publication date on
trust — check it.

## Related paper

*Admission Control for AI-Generated Documents: A Content-Addressed Provenance
Substrate with an Independent Overseer.* Adversarial evaluation across 10 test
batteries, with cross-language auditor agreement over a 75,113-assertion graph.

## Scope

This is a capability demonstration. The in-browser panel runs standard SHA-256
only. The substrate specification, algorithms, and implementation source are
withheld. Results were measured on a research prototype.

© 2026 Akhil Reddy Gaddam. All rights reserved.
