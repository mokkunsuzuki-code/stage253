Stage252: External Anchor Integration (OpenTimestamps + Cosign Bundle)

## Overview

Stage252 extends the multi-anchor verification model by adding **external anchoring mechanisms** beyond GitHub.

This stage introduces:

- OpenTimestamps-based timestamp anchoring
- Cosign bundle-based external signature verification
- Continued compatibility with Stage251 multi-anchor verification

The result is a stronger trust model that does not depend on a single platform or a single verification path.

---

## Why This Matters

Earlier stages improved verification through:

- Signed evidence
- Transparency checkpoints
- Multi-anchor threshold verification

However, trust was still partially concentrated around local or GitHub-linked evidence.

Stage252 improves this by adding:

- **OpenTimestamps** for external timestamp anchoring
- **Cosign bundle verification** for external signing and transparency-linked verification

This moves the project closer to a genuinely distributed trust model.

---

## Architecture

### Existing Anchors from Stage251

| Anchor Type | Description |
|------------|------------|
| GitHub Release Anchor | External release-linked anchor |
| Local Witness | Ed25519-based local signature |
| Checkpoint Witness | Append-only checkpoint history |

### New External Anchors in Stage252

| Anchor Type | Description |
|------------|------------|
| OpenTimestamps | External timestamp proof |
| Cosign Bundle | External signature bundle with verifiable blob signature |

---

## What Was Added

### 1. OpenTimestamps Anchor

A timestamp proof is generated for the Stage250 release manifest:

```bash
./tools/generate_ots_anchor.sh downloaded_stage250_release_anchor/release_manifest.json

Verification:

./tools/verify_ots_anchor.sh downloaded_stage250_release_anchor/release_manifest.json

Note:

Initial verification may show Pending confirmation in Bitcoin blockchain
This is normal until the timestamp becomes fully confirmed
2. Cosign Bundle-Based External Verification

A blob signature bundle is created for the same release manifest:

cosign sign-blob \
  --yes \
  --key keys/stage252_cosign.key \
  --bundle out/external_anchor/receipts/release_manifest.sigstore.json \
  downloaded_stage250_release_anchor/release_manifest.json

Verification:

cosign verify-blob \
  --key keys/stage252_cosign.pub \
  --bundle out/external_anchor/receipts/release_manifest.sigstore.json \
  downloaded_stage250_release_anchor/release_manifest.json

Expected result:

Verified OK
Output
External anchor artifacts
out/external_anchor/receipts/release_manifest.sigstore.json
downloaded_stage250_release_anchor/release_manifest.json.ots
Additional related files
keys/stage252_cosign.pub
tools/generate_ots_anchor.sh
tools/verify_ots_anchor.sh
tools/register_rekor.sh
tools/verify_rekor.sh
Security Properties
1. Reduced Platform Dependence

Verification no longer depends only on GitHub.

2. External Timestamp Evidence

OpenTimestamps provides external time-based anchoring.

3. External Signature Verification

Cosign bundle verification provides a verifiable external signature path.

4. Layered Trust Model

Stage252 combines:

local verification
checkpoint verification
GitHub anchoring
external timestamping
external bundle verification
5. Tamper Evidence

If the artifact changes, verification fails.

Limitations
OpenTimestamps may initially remain in pending state before full blockchain confirmation
The current external bundle verification is strong, but broader independent third-party witness diversity can still be improved further
This stage improves trust distribution, but it is not yet a full production trust federation
Conclusion

Stage252 advances the project from:

multi-anchor internal/external hybrid verification

to

externally anchored, layered verification

This is an important step toward a more distributed and reviewer-friendly trust model.

Next Step

Possible next evolutions:

broader third-party witness integration
external reviewer signatures
stronger policy binding between anchors and claims
reviewer-facing verification package refinement
License

MIT License

Copyright (c) 2025 Motohiro Suzuki