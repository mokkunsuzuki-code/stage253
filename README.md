# Stage253: QSP Session Anchoring

Stage253 introduces Session Anchoring for QSP.

This stage generates `session_manifest.json` from QSP-style session execution output and binds the following security-relevant properties:

- transcript hash
- policy
- fail-closed result

The resulting manifest is then anchored and independently verifiable through:

- local witness
- checkpoint witness
- GitHub anchor receipt
- OpenTimestamps (OTS)
- cosign bundle

## Why this stage matters

The core value of Stage253 is not merely adding more signatures.

The important step is showing that the evidence structure is derived from session execution itself.

This creates the following evidence chain:

QSP execution  
→ session result  
→ session manifest  
→ anchored evidence  
→ independent verification

That makes later external review stronger, because the reviewer can inspect not only the code, but also the evidence path generated from execution output.

## What this stage verifies

Stage253 demonstrates that:

- the manifest is derived from session execution output
- transcript integrity is fixed via `transcript_hash`
- policy is fixed in the manifest
- fail-closed behavior is explicitly recorded
- all witnesses and external anchors refer to the same manifest
- the full chain can be re-verified independently

## Architecture

QSP session execution  
→ `qsp_session_result.json`  
→ `session_manifest.json`  
→ local witness  
→ checkpoint witness  
→ GitHub anchor receipt  
→ OTS  
→ cosign bundle

## Files

- `tools/run_qsp_session_demo.py`  
  Minimal deterministic QSP-style session output for Stage253

- `tools/build_session_manifest.py`  
  Builds `session_manifest.json` from the session result

- `tools/sign_session_manifest.py`  
  Creates local witness and checkpoint witness

- `tools/verify_session_anchor.py`  
  Verifies manifest binding and anchor artifacts

- `tools/run_stage253_qsp_session_anchor.sh`  
  Local end-to-end runner for Stage253

- `.github/workflows/stage253-session-anchor.yml`  
  Produces GitHub anchor receipt, OTS, and cosign bundle

- `tools/safe_push.sh`  
  Push guard that checks repository name against the configured remote

## Quickstart

### 1. Local execution

```bash
./tools/run_stage253_qsp_session_anchor.sh
2. Push to GitHub
git add .
git commit -m "Stage253: update"
./tools/safe_push.sh
3. Check GitHub Actions
gh run list --workflow "stage253-session-anchor" --limit 5
4. Download artifacts
RUN_ID=$(gh run list --workflow "stage253-session-anchor" --limit 1 --json databaseId --jq '.[0].databaseId')

gh run download "$RUN_ID" \
  -n stage253-session-anchor-artifacts \
  -D downloaded_stage253_session_anchor
5. Verify
python3 tools/verify_session_anchor.py \
  --manifest downloaded_stage253_session_anchor/out/session/session_manifest.json \
  --session-result downloaded_stage253_session_anchor/out/session/qsp_session_result.json \
  --local-witness downloaded_stage253_session_anchor/out/witnesses/local_witness.json \
  --local-public downloaded_stage253_session_anchor/keys/local_witness_public.pem \
  --checkpoint-witness downloaded_stage253_session_anchor/out/checkpoints/checkpoint_witness.json \
  --checkpoint-public downloaded_stage253_session_anchor/keys/checkpoint_witness_public.pem \
  --github-receipt downloaded_stage253_session_anchor/out/anchors/github_session_anchor_receipt.json \
  --ots downloaded_stage253_session_anchor/out/session/session_manifest.json.ots \
  --cosign-bundle downloaded_stage253_session_anchor/out/anchors/session_manifest.json.cosign.bundle
Latest verification status

Confirmed on GitHub Actions:

manifest verification: OK
local witness verification: OK
checkpoint witness verification: OK
GitHub anchor receipt verification: OK
cosign bundle verification: OK

OTS may remain in pending state until Bitcoin blockchain confirmation is completed.

Limitations
Stage253 uses a deterministic QSP-style demo session
It does not yet replace the real QSP implementation
It does not claim a new cryptographic proof
The focus is evidence structure, anchoring, and independent verification
Security meaning

This stage strengthens the claim that evidence is not attached later as commentary.

Instead, evidence is generated from session execution output and then anchored through multiple verifiable layers.

Next step

Stage254 extends this by replacing the deterministic demo with real QSP execution integration.

License

MIT License

Copyright (c) 2025 Motohiro Suzuki
