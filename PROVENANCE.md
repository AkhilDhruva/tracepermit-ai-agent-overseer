# Provenance

This release is cryptographically timestamped with [OpenTimestamps](https://opentimestamps.org). The proofs are committed alongside the files they attest to.

## Digests

```
bf6a696b6ef77f6c766726f3345cec490e76e0f555bacbbf8eb3791b758c1578  index.html
2f2f8401ade75f481315724e8cf3c8e3802c97f68bf2558e5c66a5bd72dcabb0  README.md
```

`SHA256SUMS` holds the same manifest; its own digest is
`ede6c336853b8b19aad71e17cdd112253f6bb0f893496f4252d8b42cb0e886b0`.

## Proofs

| Proof | Attests to | Calendars |
|---|---|---|
| `index.html.ots` | the page itself | 4 |
| `SHA256SUMS.ots` | the manifest, and transitively every file in it | 4 |

Stamped 2026-07-19 against `alice.btc.calendar.opentimestamps.org`, `bob.btc.calendar.opentimestamps.org`, `btc.calendar.catallaxy.com`, and `finney.calendar.eternitywall.com`.

## Current state: pending

The attestations are **calendar commitments, not yet Bitcoin block headers**. Each calendar has signed that it received the digest and has committed to including it in a future block. Once those calendars anchor — typically within a few hours to a day — the proofs upgrade to full Bitcoin attestations and become independently verifiable against the blockchain without trusting any calendar.

To upgrade the proofs after anchoring:

```bash
ots upgrade index.html.ots
ots upgrade SHA256SUMS.ots
```

Then commit the upgraded files. Until that happens, the proofs rest on the calendars' word; after it, they rest on Bitcoin's proof of work.

## Verifying

```bash
sha256sum -c SHA256SUMS
ots verify index.html.ots
```

`ots` is the [OpenTimestamps client](https://github.com/opentimestamps/opentimestamps-client) (`pip install opentimestamps-client`). Verification without a local Bitcoin node falls back to a block explorer, which reintroduces a trusted party for the block-header lookup; `ots verify --bitcoin-node` against your own node does not.

Commits and tags in this repository are signed. To verify them, add the signing key to an allowed-signers file:

```bash
echo "druva.akhil@gmail.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIkaQGT7ecUicc/gr+dXz7G+WjfC7JhlLaDU7zV/mBaH" > allowed_signers
git -c gpg.ssh.allowedSignersFile=allowed_signers log --show-signature -1
git -c gpg.ssh.allowedSignersFile=allowed_signers tag -v v1.0.0
```

## What this does and does not establish

A timestamp proves that **these exact bytes existed no later than the anchoring block**. That is all it proves.

It does not establish authorship, originality, correctness, or that the measured results reported in the page are true. It cannot prove the file did not exist *earlier*. Anyone can timestamp anyone's bytes.

For a project whose subject is the difference between structural verification and semantic support, the distinction is the point: this is an integrity and ordering claim, not a truth claim. TracePermit makes the same distinction about the claims it admits — provenance that reconstructs is not the same as a statement that is true.
