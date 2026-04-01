# Stage243: External Reviewer Onboarding Framework

MIT License © 2025 Motohiro Suzuki

---

## Overview

Stage243 prepares this repository for real independent external reviewers.

This stage does not claim that a real external reviewer is already active in production.
Instead, it defines the onboarding structure required to activate one safely and transparently in a future stage.

Stage242 introduced the rule that an external reviewer is required.

Stage243 adds the operational layer needed to make that reviewer real later.

---

## What Stage243 Adds

- reviewer onboarding documentation
- reviewer invitation template
- reviewer metadata example
- reviewer registry structure
- reviewer registration tooling
- fail-safe registry verification
- Stage243-specific CI verification

---

## Why This Stage Matters

A policy can say that an external reviewer is required.

But unless the project defines:

- how a reviewer is added
- what metadata is recorded
- how the public key is registered
- how activation is validated
- how failure is handled before activation

the requirement remains conceptual.

Stage243 turns that concept into an operationally defined process.

---

## Security Meaning

This stage upgrades the project from:

- external reviewer exists in policy

to:

- external reviewer onboarding is operationally defined

This is an important shift from trust design to trust preparation.

---

## Key Property

If no active external reviewer is registered, verification fails safely.

This prevents the repository from silently behaving as if independent external review already exists when it does not.

---

## Registry Model

Reviewer state is stored in:

```text
metadata/reviewers/reviewer_registry.yaml

The registry supports:

empty initial state
active reviewer validation
public key path checking
future reviewer activation without redesign
Documentation Added
Reviewer onboarding guide
docs/reviewer_onboarding.md

Defines:

reviewer independence expectations
key ownership rules
public-key-only repository model
activation process
future real reviewer path
Reviewer invitation template
docs/reviewer_invitation_template.md

Provides a minimal invitation format for future independent collaborators.

Reviewer metadata example
docs/reviewer_metadata_example.yaml

Shows the expected structure for a future active reviewer.

Tooling Added
Register reviewer
tools/register_reviewer.py

Registers reviewer metadata into the registry.

Verify registry
tools/verify_stage243_registry.py

Checks that:

the registry exists
an active reviewer is present when required
the referenced reviewer public key exists
Build Stage243 manifest
tools/build_stage243_manifest.py

Builds a manifest of the files that define the onboarding framework.

Run demo
tools/run_stage243_demo.sh

Demonstrates:

safe failure with no active reviewer
example reviewer registration
successful registry validation
Policy

Stage243 policy is defined in:

docs/stage243_policy.yaml

It includes:

threshold: 2
external reviewer required
minimum independence groups: 2
reviewer registry path
active reviewer requirement
Demo

Run:

./tools/run_stage243_demo.sh

Expected behavior:

empty registry fails safely
example reviewer can be registered
registry validation then succeeds
Tests

Run locally:

pytest -q

Stage243-specific CI runs the onboarding workflow and validates the Stage243 registry path successfully.

CI Status

The stage243-reviewer-onboarding workflow is now passing, which confirms that the onboarding framework works in GitHub Actions as well as locally.

What Stage243 Does Not Claim

Stage243 does not claim:

that a real external reviewer is already active in production
that real third-party review has already been completed
that reviewer identity has already been independently verified

Instead, Stage243 makes a narrower and honest claim:

the repository is now prepared to onboard a real external reviewer safely
Future Activation Path

A future stage can activate a real reviewer by:

having the reviewer generate a keypair on their own device
receiving only the public key
registering reviewer metadata
marking the reviewer active
collecting an external signature
verifying approval under policy
Significance

Stage242 introduced independent approval as a rule.

Stage243 prepares the project to make that independence real in operation.

This stage is the bridge from policy-level independence to real-world reviewer onboarding.

Next Direction

Natural next steps include:

activating a real external reviewer
registering a real reviewer public key
collecting a real external signature
defining reviewer lifecycle rules
adding revocation / rotation support

---

# GitHub更新

```bash
cd ~/Desktop/test/stage243

cat > README.md << 'EOF'
# Stage243: External Reviewer Onboarding Framework

MIT License © 2025 Motohiro Suzuki

---

## Overview

Stage243 prepares this repository for real independent external reviewers.

This stage does not claim that a real external reviewer is already active in production.
Instead, it defines the onboarding structure required to activate one safely and transparently in a future stage.

Stage242 introduced the rule that an external reviewer is required.

Stage243 adds the operational layer needed to make that reviewer real later.

---

## What Stage243 Adds

- reviewer onboarding documentation
- reviewer invitation template
- reviewer metadata example
- reviewer registry structure
- reviewer registration tooling
- fail-safe registry verification
- Stage243-specific CI verification

---

## Why This Stage Matters

A policy can say that an external reviewer is required.

But unless the project defines:

- how a reviewer is added
- what metadata is recorded
- how the public key is registered
- how activation is validated
- how failure is handled before activation

the requirement remains conceptual.

Stage243 turns that concept into an operationally defined process.

---

## Security Meaning

This stage upgrades the project from:

- external reviewer exists in policy

to:

- external reviewer onboarding is operationally defined

This is an important shift from trust design to trust preparation.

---

## Key Property

If no active external reviewer is registered, verification fails safely.

This prevents the repository from silently behaving as if independent external review already exists when it does not.

---

## Registry Model

Reviewer state is stored in:

```text
metadata/reviewers/reviewer_registry.yaml

The registry supports:

empty initial state
active reviewer validation
public key path checking
future reviewer activation without redesign
Documentation Added
Reviewer onboarding guide
docs/reviewer_onboarding.md

Defines:

reviewer independence expectations
key ownership rules
public-key-only repository model
activation process
future real reviewer path
Reviewer invitation template
docs/reviewer_invitation_template.md

Provides a minimal invitation format for future independent collaborators.

Reviewer metadata example
docs/reviewer_metadata_example.yaml

Shows the expected structure for a future active reviewer.

Tooling Added
Register reviewer
tools/register_reviewer.py

Registers reviewer metadata into the registry.

Verify registry
tools/verify_stage243_registry.py

Checks that:

the registry exists
an active reviewer is present when required
the referenced reviewer public key exists
Build Stage243 manifest
tools/build_stage243_manifest.py

Builds a manifest of the files that define the onboarding framework.

Run demo
tools/run_stage243_demo.sh

Demonstrates:

safe failure with no active reviewer
example reviewer registration
successful registry validation
Policy

Stage243 policy is defined in:

docs/stage243_policy.yaml

It includes:

threshold: 2
external reviewer required
minimum independence groups: 2
reviewer registry path
active reviewer requirement
Demo

Run:

./tools/run_stage243_demo.sh

Expected behavior:

empty registry fails safely
example reviewer can be registered
registry validation then succeeds
Tests

Run locally:

pytest -q

Stage243-specific CI runs the onboarding workflow and validates the Stage243 registry path successfully.

CI Status

The stage243-reviewer-onboarding workflow is now passing.

What Stage243 Does Not Claim

Stage243 does not claim:

that a real external reviewer is already active in production
that real third-party review has already been completed
that reviewer identity has already been independently verified

Instead, Stage243 makes a narrower and honest claim:

the repository is now prepared to onboard a real external reviewer safely
Future Activation Path

A future stage can activate a real reviewer by:

having the reviewer generate a keypair on their own device
receiving only the public key
registering reviewer metadata
marking the reviewer active
collecting an external signature
verifying approval under policy
Significance

Stage242 introduced independent approval as a rule.

Stage243 prepares the project to make that independence real in operation.

This stage is the bridge from policy-level independence to real-world reviewer onboarding.

Next Direction

Natural next steps include:

activating a real external reviewer
registering a real reviewer public key
collecting a real external signature
defining reviewer lifecycle rules
adding revocation / rotation support
EOF

git add README.md
git commit -m "Stage243: finalize README for reviewer onboarding framework"
git push