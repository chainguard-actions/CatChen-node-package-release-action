<!-- markdownlint-disable -->

# Hardening Report: CatChen--node-package-release-action/v2.3.22

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **CatChen--node-package-release-action/v2.3.22** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across every workflow file use mutable tags or version strings instead of full 40-character SHA commit digests. This exposes the workflows to supply-chain attacks if any referenced action is compromised or its tag is moved. Affected references include: actions/create-github-app-token@v3, actions/checkout@v7, actions/setup-node@v7, CatChen/check-git-status-action@v2, CatChen/accept-to-ship-action@v0.8, CatChen/eslint-suggestion-action@v4, dependabot/fetch-metadata@v3, github/codeql-action/init@v4.37.9, github/codeql-action/analyze@v4.37.9. None of these are pinned to a full SHA digest.

Locations:

- `.github/workflows/build.yml:19`
- `.github/workflows/build.yml:24`
- `.github/workflows/build.yml:35`
- `.github/workflows/build.yml:52`
- `.github/workflows/codeql.yml:57`
- `.github/workflows/codeql.yml:63`
- `.github/workflows/codeql.yml:72`
- `.github/workflows/dependabot.yml:17`
- `.github/workflows/eslint.yml:20`
- `.github/workflows/eslint.yml:25`
- `.github/workflows/eslint.yml:35`
- `.github/workflows/release.yml:56`
- `.github/workflows/release.yml:62`
- `.github/workflows/release.yml:73`
- `.github/workflows/release.yml:88`
- `.github/workflows/release.yml:97`
- `.github/workflows/release.yml:107`
- `.github/workflows/ship.yml:47`
- `.github/workflows/ship.yml:51`
- `.github/workflows/ship.yml:68`
- `.github/workflows/ship.yml:83`
- `.github/workflows/ship.yml:87`
- `.github/workflows/ship.yml:104`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:27`
- `.github/workflows/test.yml:62`
- `.github/workflows/test.yml:67`

### missing-permissions (severity: medium)

eslint.yml has no top-level `permissions:` key and its only job (`eslint`) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the default broad token permissions, which may include write access to repository contents and other resources.

Locations:

- `.github/workflows/eslint.yml:1`

### missing-permissions (severity: medium)

ship.yml has no top-level `permissions:` key and the `concurrency-group` job has no job-level `permissions:` key. The other two jobs (`accept-to-ship` and `pass-to-ship`) do have job-level permissions, but the `concurrency-group` job inherits default broad token permissions.

Locations:

- `.github/workflows/ship.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references across 7 workflow files by pinning them to full 40-character SHA digests: actions/create-github-app-token@v3 → bcd2ba49218906704ab6c1aa796996da409d3eb1, actions/checkout@v7 → 3d3c42e5aac5ba805825da76410c181273ba90b1, actions/setup-node@v7 → 820762786026740c76f36085b0efc47a31fe5020, CatChen/check-git-status-action@v2 → cc5a79733c441f67cd0cd076de116cd2eebcebfe, CatChen/accept-to-ship-action@v0.8 → 0cb7fba98f2dda7b7af98e118e8e5f82dbbc2e1c, CatChen/eslint-suggestion-action@v4 → aecb87aaa6d425dc2ac23c49fffbcbad7d5ce3b2, dependabot/fetch-metadata@v3 → 25dd0e34f4fe68f24cc83900b1fe3fe149efef98, github/codeql-action/init@v4.37.9 → cdf488f595d80d6e07e03d4674febd5ab45fa938, github/codeql-action/analyze@v4.37.9 → cdf488f595d80d6e07e03d4674febd5ab45fa938. Added `permissions: {}` at top-level and `permissions: { contents: read, pull-requests: write }` at job-level in eslint.yml. Added `permissions: {}` to the concurrency-group job in ship.yml.

### Iteration 2

**Fixes applied:** missing-permissions

**Notes:**

Added explicit permissions blocks to the two jobs that were missing them in .github/workflows/release.yml: (1) the `eslint` job now has `permissions: contents: read, pull-requests: write` to match what the called eslint.yml reusable workflow requires; (2) the `release` job now has `permissions: contents: read` as the minimum needed for checkout — the actual release operations (creating releases, pushing tags) are performed via the GitHub App token passed explicitly as the `github-token` input to the action, not via the job's GITHUB_TOKEN.

