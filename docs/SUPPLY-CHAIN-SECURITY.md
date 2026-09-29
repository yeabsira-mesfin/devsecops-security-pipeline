# Software supply-chain security

The pipeline already demonstrates SAST, SCA, secret scanning, container scanning, DAST, and security gates. This document captures the controls needed to extend that workflow into stronger release provenance.

## Dependency integrity

- Keep lockfiles committed and review dependency changes separately from application changes when practical.
- Fail builds on known high-impact vulnerabilities according to an explicit, documented policy.
- Review abandoned or unexpectedly replaced packages before accepting updates.

## CI trust

- Pin third-party GitHub Actions to reviewed immutable commit SHAs for higher-assurance workflows.
- Grant each workflow/job the minimum GitHub token permissions it needs.
- Never expose repository or deployment secrets to untrusted pull-request code.
- Protect release environments with explicit approval and branch rules when deploying externally.

## Build provenance

- Generate an SBOM for release/container artifacts.
- Record source commit, dependency lockfile, builder/workflow identity, and artifact digest.
- Sign or attest release artifacts before promotion to a production registry.
- Verify signatures/attestations during deployment instead of trusting artifact names or tags alone.

## Container release controls

- Prefer minimal base images and pin them by digest for controlled releases.
- Re-scan images after base-image updates and before deployment.
- Run as non-root with minimal Linux capabilities and a read-only filesystem where possible.

## Verification goals

A mature version of this lab should be able to answer: what source produced this artifact, which dependencies were included, which security gates passed, whether the artifact changed after the build, and who authorized promotion.
