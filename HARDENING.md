<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-af/v0.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-af/v0.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference third-party actions using mutable version tags (@v4) instead of immutable full 40-character commit SHA digests. A tag can be silently moved to point to a different (potentially malicious) commit, enabling supply-chain attacks. Affected references: check-dist.yml — actions/checkout@v4, actions/setup-node@v4, actions/upload-artifact@v4; check-run.yml — actions/checkout@v4. Each should be pinned to a full SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.

Locations:

- `.github/workflows/check-dist.yml:17`
- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:36`
- `.github/workflows/check-run.yml:13`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level permissions: key, and neither job within them declares a job-level permissions: key. Without explicit permissions, GitHub Actions grants the default token permissions (which may include write access to repository contents, packages, etc.), violating the principle of least privilege. A minimal permissions: block (e.g. contents: read) should be added at the top level or per-job in both check-dist.yml and check-run.yml.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all third-party action references to full 40-character commit SHAs with original tags preserved as comments — actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4, actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4, actions/upload-artifact@ea165f8d65b6e75b540449e92b4886f43607fa02 # v4. (2) Added top-level `permissions: contents: read` to both check-dist.yml and check-run.yml to enforce least-privilege access.

