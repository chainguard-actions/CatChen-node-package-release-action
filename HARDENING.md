<!-- markdownlint-disable -->

# Hardening Report: CatChen--node-package-release-action/v2.3.21

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **CatChen--node-package-release-action/v2.3.21** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files use mutable tag-based `uses:` references instead of immutable 40-character SHA commit digests, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

Failing references in build.yml: `actions/create-github-app-token@v3`, `actions/checkout@v7`, `actions/setup-node@v7`, `CatChen/check-git-status-action@v2`.

Failing references in codeql.yml: `actions/checkout@v7`, `github/codeql-action/init@v4.37.7`, `github/codeql-action/analyze@v4.37.7`.

Failing references in dependabot.yml: `dependabot/fetch-metadata@v3`.

Failing references in eslint.yml: `actions/checkout@v7`, `actions/setup-node@v7`, `CatChen/eslint-suggestion-action@v4`.

Failing references in release.yml: `actions/create-github-app-token@v3`, `actions/checkout@v7` (multiple), `actions/setup-node@v7` (multiple).

Failing references in ship.yml: `actions/checkout@v7` (multiple), `actions/create-github-app-token@v3` (multiple), `CatChen/accept-to-ship-action@v0.8` (multiple).

Failing references in test.yml: `actions/checkout@v7` (multiple), `actions/setup-node@v7` (multiple).

Locations:

- `.github/workflows/build.yml:24`
- `.github/workflows/build.yml:29`
- `.github/workflows/build.yml:36`
- `.github/workflows/build.yml:51`
- `.github/workflows/codeql.yml:57`
- `.github/workflows/codeql.yml:63`
- `.github/workflows/codeql.yml:80`
- `.github/workflows/dependabot.yml:16`
- `.github/workflows/eslint.yml:21`
- `.github/workflows/eslint.yml:25`
- `.github/workflows/eslint.yml:33`
- `.github/workflows/release.yml:57`
- `.github/workflows/release.yml:64`
- `.github/workflows/release.yml:71`
- `.github/workflows/release.yml:93`
- `.github/workflows/release.yml:97`
- `.github/workflows/release.yml:104`
- `.github/workflows/ship.yml:52`
- `.github/workflows/ship.yml:54`
- `.github/workflows/ship.yml:62`
- `.github/workflows/ship.yml:82`
- `.github/workflows/ship.yml:84`
- `.github/workflows/ship.yml:92`
- `.github/workflows/test.yml:26`
- `.github/workflows/test.yml:31`
- `.github/workflows/test.yml:71`
- `.github/workflows/test.yml:76`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and contain at least one job that also lacks a job-level `permissions:` block, meaning those jobs run with the default (potentially broad) token permissions.

- `eslint.yml`: No top-level permissions; the `eslint` job has no `permissions:` block.
- `ship.yml`: No top-level permissions; the `concurrency-group` job has no `permissions:` block.
- `release.yml`: No top-level permissions; the `eslint` reusable-workflow call job and the `release` job both lack `permissions:` blocks.

Locations:

- `.github/workflows/eslint.yml:1`
- `.github/workflows/ship.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 7 workflow files (build.yml, codeql.yml, dependabot.yml, eslint.yml, release.yml, ship.yml, test.yml) by replacing mutable tag references with full 40-character SHA digests. Added missing permissions blocks: eslint.yml's eslint job got `contents: read, pull-requests: write`; ship.yml's concurrency-group job got `permissions: {}`; release.yml's eslint reusable-workflow call got `contents: read, pull-requests: write` and the release job got `contents: write, pull-requests: write`. All SHAs were resolved via lookup_action_sha.

