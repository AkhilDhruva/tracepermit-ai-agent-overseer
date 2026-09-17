# Provenance

This release is cryptographically timestamped with
[OpenTimestamps](https://opentimestamps.org). The proofs are committed
alongside the files they attest to.

A timestamp attests to **specific bytes**, not to a repository. When the bytes
change, the old proof does not become wrong — it keeps attesting to the release
it was made for. This file therefore tracks releases, not just digests.

## Releases

| Release | State | Proofs |
|---|---|---|
| **v1.1.0** (current) | digests published below; **OpenTimestamps stamp pending** | — |
| **v1.0.0** | attested; bytes preserved at tag `v1.0.0` | `attestations/v1.0.0/` |

### v1.1.0 — current

```
8f63edd19229d74840cfbec88b4657f40bf5255f5a4535845593c638bc5207ed  index.html
6c5ecbdb30aacafcf0894fcab74c4d654cb45094084eff8f0893b5e1e6bd12bf  README.md
```

`SHA256SUMS` holds the same manifest; its own digest is
`609ab9e0fe9ec4d39f7479bf1707318510651a5fda416673b05d3c8d95f7a6a5`.

**What changed from v1.0.0, and why.** The results panel carried a stat reading
*"19/19 cross-language identity vectors agree after I found & fixed a 2⁵³
divergence."* The research record behind this work **retired that figure**: it
was a fixture-framed count produced by a harness that was subsequently
superseded, and the authoritative record is *6 of 15 boundary vectors diverged
in v0.9.0*, with *15/15 meeting required verdicts against the v0.9.1
successor*. The page now states the authoritative figures.

Correcting it required changing attested bytes, which is why this is a release
rather than an edit. That is the intended behaviour of the mechanism, not a
workaround: a system whose claims cannot be corrected without leaving a trace
would be a worse system, and a correction that silently overwrote an
attestation would be exactly the failure this project exists to make visible.
The v1.0.0 bytes remain reachable, attested, and wrong — which is the point.

The README was rewritten in the same release. `index.html.ots` and
`SHA256SUMS.ots` for v1.1.0 do not exist yet; see **Stamping** below.

### v1.0.0 — attested, superseded

```
bf6a696b6ef77f6c766726f3345cec490e76e0f555bacbbf8eb3791b758c1578  index.html
2f2f8401ade75f481315724e8cf3c8e3802c97f68bf2558e5c66a5bd72dcabb0  README.md
```

Manifest digest `ede6c336853b8b19aad71e17cdd112253f6bb0f893496f4252d8b42cb0e886b0`.

| Proof | Attests to | Calendars |
|---|---|---|
| `attestations/v1.0.0/index.html.ots` | the v1.0.0 page | 4 |
| `attestations/v1.0.0/SHA256SUMS.ots` | the v1.0.0 manifest, and transitively every file in it | 4 |

Stamped 2026-07-19 against `alice.btc.calendar.opentimestamps.org`,
`bob.btc.calendar.opentimestamps.org`, `btc.calendar.catallaxy.com`, and
`finney.calendar.eternitywall.com`.

The proofs live under `attestations/v1.0.0/` rather than in the repository root
so that nothing at `HEAD` looks like an attestation of bytes it does not
attest to. To verify them against the bytes they were made for, check out the
tag — where the proofs sit beside their subjects exactly as they were stamped:

```bash
git checkout v1.0.0
sha256sum -c SHA256SUMS
ots verify index.html.ots
ots verify SHA256SUMS.ots
```

**Attestation state: pending.** As of stamping these were **calendar
commitments, not yet Bitcoin block headers** — each calendar signed that it
received the digest and committed to including it in a future block. To upgrade
after anchoring:

```bash
ots upgrade attestations/v1.0.0/index.html.ots
ots upgrade attestations/v1.0.0/SHA256SUMS.ots
```

Until that happens the proofs rest on the calendars' word; after it, they rest
on Bitcoin's proof of work.

## Stamping v1.1.0

Not yet done — it needs the author's machine and outbound calendar access:

```bash
ots stamp index.html
ots stamp SHA256SUMS
git add index.html.ots SHA256SUMS.ots
git commit -m "Stamp v1.1.0"
git tag -s v1.1.0 -m "v1.1.0 — corrected canonicalization figure"
```

Until those proofs exist, v1.1.0 carries digests but no timestamp, and this
file says so rather than implying otherwise.

## Verifying

```bash
sha256sum -c SHA256SUMS
```

CI re-runs this on every push, re-derives the manifest's own digest and checks
it against the value published above, and re-verifies the attested bytes at
each release tag — so "the manifest is current" is a continuously enforced
property rather than a claim in a markdown file.

`ots` is the [OpenTimestamps client](https://github.com/opentimestamps/opentimestamps-client)
(`pip install opentimestamps-client`). Verification without a local Bitcoin
node falls back to a block explorer, which reintroduces a trusted party for the
block-header lookup; `ots verify --bitcoin-node` against your own node does not.

Commits and tags in this repository are signed. To verify them, add the signing
key to an allowed-signers file:

```bash
echo "druva.akhil@gmail.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIkaQGT7ecUicc/gr+dXz7G+WjfC7JhlLaDU7zV/mBaH" > allowed_signers
git -c gpg.ssh.allowedSignersFile=allowed_signers log --show-signature -1
git -c gpg.ssh.allowedSignersFile=allowed_signers tag -v v1.0.0
```

## What this does and does not establish

A timestamp proves that **these exact bytes existed no later than the anchoring
block**. That is all it proves.

It does not establish authorship, originality, correctness, or that the
measured results reported in the page are true. It cannot prove the file did
not exist *earlier*. Anyone can timestamp anyone's bytes.

For a project whose subject is the difference between structural verification
and semantic support, the distinction is the point: this is an integrity and
ordering claim, not a truth claim. TracePermit makes the same distinction about
the claims it admits — provenance that reconstructs is not the same as a
statement that is true.

v1.1.0 is the worked example. The v1.0.0 attestation is still valid, still
verifiable, and attests to a page carrying a figure the author's own record had
retired. A timestamp certifies *when*, never *whether*.
