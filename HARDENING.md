<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-af/v0.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-af/v0.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference external actions using mutable version tags (@v4) instead of pinned 40-character commit SHA digests. A compromised or altered tag could silently execute malicious code in CI. Affected references: actions/checkout@v4, actions/setup-node@v4, actions/upload-artifact@v4 in check-dist.yml; actions/checkout@v4 in check-run.yml.

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/check-run.yml:16`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` key, and no job within them has a job-level `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all four mutable action references to full 40-char commit SHAs with tag comments preserved — actions/checkout@v4→@11d5960a, actions/setup-node@v4→@49933ea5, actions/upload-artifact@v4→@ea165f8d in check-dist.yml, and actions/checkout@v4→@11d5960a in check-run.yml. (2) Added top-level `permissions: contents: read` to both check-dist.yml and check-run.yml, granting only the minimum permission needed for repository checkout.

