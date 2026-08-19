<!-- markdownlint-disable -->

# Hardening Report: zaproxy--action-af/v0.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **zaproxy--action-af/v0.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag refs instead of full 40-character SHA commit pins, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

In `.github/workflows/check-dist.yml`:
- `uses: actions/checkout@v6` (line 19)
- `uses: actions/setup-node@v6` (line 22)
- `uses: actions/upload-artifact@v7` (line 40)

In `.github/workflows/check-run.yml`:
- `uses: actions/checkout@v6` (line 12)

All of these should be pinned to a full SHA, e.g. `uses: actions/checkout@<40-char-sha> # v6`.

Locations:

- `.github/workflows/check-dist.yml:19`
- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:40`
- `.github/workflows/check-run.yml:12`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/check-run.yml` declares a top-level `permissions:` key, and no job in either file has a job-level `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/check-run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. `.github/workflows/check-dist.yml`:
   - Added `permissions: {}` top-level block
   - Pinned `actions/checkout@v6` → `@d23441a48e516b6c34aea4fa41551a30e30af803 # v6`
   - Pinned `actions/setup-node@v6` → `@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6`
   - Pinned `actions/upload-artifact@v7` → `@043fb46d1a93c77aae656e7c1c64a875d1fc6a0a # v7`

2. `.github/workflows/check-run.yml`:
   - Added `permissions: {}` top-level block
   - Pinned `actions/checkout@v6` → `@d23441a48e516b6c34aea4fa41551a30e30af803 # v6`

All SHAs were resolved using lookup_action_sha against the real upstream repositories.

