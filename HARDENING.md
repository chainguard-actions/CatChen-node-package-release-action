<!-- markdownlint-disable -->

# Hardening Report: CatChen--node-package-release-action/v2.3.20

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **CatChen--node-package-release-action/v2.3.20** was hardened automatically. 10 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in build.yml use mutable version tags instead of pinned SHA commits, making the workflow vulnerable to supply-chain attacks. Failing references: actions/create-github-app-token@v3 (line 20), actions/checkout@v7 (line 26), actions/setup-node@v7 (line 35), CatChen/check-git-status-action@v2 (line 50).

Locations:

- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:26`
- `.github/workflows/build.yml:35`
- `.github/workflows/build.yml:50`

### unpinned-uses (severity: high)

All uses: references in codeql.yml use mutable version tags instead of pinned SHA commits. Failing references: actions/checkout@v7 (line 57), github/codeql-action/init@v4.37.6 (line 61), github/codeql-action/analyze@v4.37.6 (line 76).

Locations:

- `.github/workflows/codeql.yml:57`
- `.github/workflows/codeql.yml:61`
- `.github/workflows/codeql.yml:76`

### unpinned-uses (severity: high)

The uses: reference in dependabot.yml uses a mutable version tag instead of a pinned SHA commit. Failing reference: dependabot/fetch-metadata@v3 (line 16).

Locations:

- `.github/workflows/dependabot.yml:16`

### unpinned-uses (severity: high)

All uses: references in eslint.yml use mutable version tags instead of pinned SHA commits. Failing references: actions/checkout@v7 (line 19), actions/setup-node@v7 (line 24), CatChen/eslint-suggestion-action@v4 (line 30).

Locations:

- `.github/workflows/eslint.yml:19`
- `.github/workflows/eslint.yml:24`
- `.github/workflows/eslint.yml:30`

### unpinned-uses (severity: high)

All uses: references in release.yml use mutable version tags instead of pinned SHA commits. Failing references: actions/create-github-app-token@v3 (line 52), actions/checkout@v7 (lines 57, 79, 83), actions/setup-node@v7 (lines 63, 88).

Locations:

- `.github/workflows/release.yml:52`
- `.github/workflows/release.yml:57`
- `.github/workflows/release.yml:63`
- `.github/workflows/release.yml:79`
- `.github/workflows/release.yml:83`
- `.github/workflows/release.yml:88`

### unpinned-uses (severity: high)

All uses: references in ship.yml use mutable version tags instead of pinned SHA commits. Failing references: actions/checkout@v7 (lines 52, 79), actions/create-github-app-token@v3 (lines 54, 81), CatChen/accept-to-ship-action@v0.8 (lines 60, 87).

Locations:

- `.github/workflows/ship.yml:52`
- `.github/workflows/ship.yml:54`
- `.github/workflows/ship.yml:60`
- `.github/workflows/ship.yml:79`
- `.github/workflows/ship.yml:81`
- `.github/workflows/ship.yml:87`

### unpinned-uses (severity: high)

All uses: references in test.yml use mutable version tags instead of pinned SHA commits. Failing references: actions/checkout@v7 (lines 20, 55), actions/setup-node@v7 (lines 25, 60).

Locations:

- `.github/workflows/test.yml:20`
- `.github/workflows/test.yml:25`
- `.github/workflows/test.yml:55`
- `.github/workflows/test.yml:60`

### missing-permissions (severity: medium)

eslint.yml has no top-level permissions: key and the only job (eslint) has no job-level permissions: block. This means the workflow runs with the default (potentially broad) token permissions.

Locations:

- `.github/workflows/eslint.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level permissions: key, and the release job (which checks out code and runs the action) has no job-level permissions: block. The eslint reusable-workflow call job also has no permissions block.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

ship.yml has no top-level permissions: key and the concurrency-group job has no job-level permissions: block, meaning it runs with default (potentially broad) token permissions.

Locations:

- `.github/workflows/ship.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action references to full commit SHAs across 7 workflow files (build.yml, codeql.yml, dependabot.yml, eslint.yml, release.yml, ship.yml, test.yml). Added missing permissions blocks: eslint job in eslint.yml gets contents:read + pull-requests:write; release job in release.yml gets contents:write + pull-requests:write; eslint reusable-workflow call in release.yml gets contents:read + pull-requests:write; concurrency-group job in ship.yml gets permissions:{} (empty, as it only echoes a notice). All pinned SHAs were resolved via lookup_action_sha.

