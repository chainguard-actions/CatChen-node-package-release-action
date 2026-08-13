<!-- markdownlint-disable -->

# Hardening Report: CatChen--node-package-release-action/v2.3.18

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **CatChen--node-package-release-action/v2.3.18** was hardened automatically. 4 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across every workflow file use mutable tag-based refs (e.g., @v3, @v4, @v6, @v0.8) instead of full 40-character SHA commit hashes. This exposes the workflows to supply-chain attacks if any referenced action's tag is moved to a malicious commit. Affected references include: actions/create-github-app-token@v3, actions/checkout@v6, actions/setup-node@v6, CatChen/check-git-status-action@v2, github/codeql-action/init@v4, github/codeql-action/analyze@v4, dependabot/fetch-metadata@v3, CatChen/eslint-suggestion-action@v4, CatChen/accept-to-ship-action@v0.8.

Locations:

- `.github/workflows/build.yml:20`
- `.github/workflows/build.yml:25`
- `.github/workflows/build.yml:33`
- `.github/workflows/build.yml:50`
- `.github/workflows/codeql.yml:55`
- `.github/workflows/codeql.yml:59`
- `.github/workflows/codeql.yml:73`
- `.github/workflows/dependabot.yml:16`
- `.github/workflows/eslint.yml:19`
- `.github/workflows/eslint.yml:23`
- `.github/workflows/eslint.yml:30`
- `.github/workflows/release.yml:57`
- `.github/workflows/release.yml:64`
- `.github/workflows/release.yml:71`
- `.github/workflows/release.yml:93`
- `.github/workflows/release.yml:99`
- `.github/workflows/release.yml:107`
- `.github/workflows/ship.yml:55`
- `.github/workflows/ship.yml:57`
- `.github/workflows/ship.yml:63`
- `.github/workflows/ship.yml:84`
- `.github/workflows/ship.yml:86`
- `.github/workflows/ship.yml:92`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:27`
- `.github/workflows/test.yml:67`
- `.github/workflows/test.yml:72`

### missing-permissions (severity: medium)

eslint.yml has no top-level `permissions:` block and its only job (`eslint`) also has no job-level `permissions:` block. Without explicit permissions, the job inherits the default token permissions (which can be write-all in some repository configurations), violating the principle of least privilege.

Locations:

- `.github/workflows/eslint.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` block, and the `release` job (which checks out code and runs the action with a GitHub App token) has no job-level `permissions:` block. The job inherits default token permissions without restriction.

Locations:

- `.github/workflows/release.yml:55`

### missing-permissions (severity: medium)

ship.yml has no top-level `permissions:` block, and the `concurrency-group` job has no job-level `permissions:` block. The job inherits default token permissions without restriction.

Locations:

- `.github/workflows/ship.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full SHA hashes: actions/create-github-app-token@v3→bcd2ba49..., actions/checkout@v6→d23441a4..., actions/setup-node@v6→24997072..., CatChen/check-git-status-action@v2→cc5a7973..., github/codeql-action/init@v4→d1ba80a1..., github/codeql-action/analyze@v4→d1ba80a1..., dependabot/fetch-metadata@v3→25dd0e34..., CatChen/eslint-suggestion-action@v4→2b07b898..., CatChen/accept-to-ship-action@v0.8→f408fe03.... Added top-level permissions: {} and job-level permissions to eslint.yml (contents: read, pull-requests: write) and ship.yml (concurrency-group: {}). Added job-level permissions (contents: write, pull-requests: write) to the release job in release.yml.

### Iteration 2

**Fixes applied:** missing-permissions

**Notes:**

Added `permissions: {}` to the `eslint` job in `.github/workflows/release.yml` (line 61). The job uses a reusable workflow (`.github/workflows/eslint.yml`) and requires no special permissions, so an empty permissions block is appropriate. This prevents the job from inheriting potentially broad default permissions from the GitHub Actions runtime.

