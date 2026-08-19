<!-- markdownlint-disable -->

# Hardening Report: CatChen--node-package-release-action/v2.3.21-7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **CatChen--node-package-release-action/v2.3.21-7** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if a tag is moved or an action is compromised. Failing references include: actions/create-github-app-token@v3, actions/checkout@v7, actions/setup-node@v7, CatChen/check-git-status-action@v2, github/codeql-action/init@v4.37.6, github/codeql-action/analyze@v4.37.6, dependabot/fetch-metadata@v3, CatChen/eslint-suggestion-action@v4, CatChen/accept-to-ship-action@v0.8.

Locations:

- `.github/workflows/build.yml:21`
- `.github/workflows/build.yml:27`
- `.github/workflows/build.yml:33`
- `.github/workflows/build.yml:49`
- `.github/workflows/codeql.yml:55`
- `.github/workflows/codeql.yml:59`
- `.github/workflows/codeql.yml:76`
- `.github/workflows/dependabot.yml:14`
- `.github/workflows/eslint.yml:21`
- `.github/workflows/eslint.yml:24`
- `.github/workflows/eslint.yml:31`
- `.github/workflows/release.yml:52`
- `.github/workflows/release.yml:60`
- `.github/workflows/release.yml:67`
- `.github/workflows/release.yml:90`
- `.github/workflows/release.yml:94`
- `.github/workflows/release.yml:101`
- `.github/workflows/ship.yml:57`
- `.github/workflows/ship.yml:58`
- `.github/workflows/ship.yml:64`
- `.github/workflows/ship.yml:88`
- `.github/workflows/ship.yml:89`
- `.github/workflows/ship.yml:95`
- `.github/workflows/test.yml:25`
- `.github/workflows/test.yml:29`
- `.github/workflows/test.yml:64`
- `.github/workflows/test.yml:68`

### missing-permissions (severity: medium)

eslint.yml has no top-level permissions block and its only job (eslint) has no job-level permissions block. Without explicit permissions, the job inherits the default repository permissions (which may be read/write for contents), violating the principle of least privilege.

Locations:

- `.github/workflows/eslint.yml:1`

### missing-permissions (severity: medium)

ship.yml has no top-level permissions block and the concurrency-group job has no job-level permissions block. The other jobs (accept-to-ship, pass-to-ship) do have job-level permissions, but the concurrency-group job does not, meaning it runs with default (potentially broad) permissions.

Locations:

- `.github/workflows/ship.yml:26`

### missing-permissions (severity: medium)

release.yml has no top-level permissions block. The release job and the eslint reusable-workflow call job have no job-level permissions block, meaning they run with default (potentially broad) permissions.

Locations:

- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 workflow files:

1. **unpinned-uses**: Pinned all action references to full 40-char SHAs:
   - actions/create-github-app-token@v3 → @bcd2ba49218906704ab6c1aa796996da409d3eb1
   - actions/checkout@v7 → @3d3c42e5aac5ba805825da76410c181273ba90b1
   - actions/setup-node@v7 → @820762786026740c76f36085b0efc47a31fe5020
   - CatChen/check-git-status-action@v2 → @cc5a79733c441f67cd0cd076de116cd2eebcebfe
   - github/codeql-action/init@v4.37.6 → @5595ccaf912efad79be6eef63a5619ff05969be3
   - github/codeql-action/analyze@v4.37.6 → @5595ccaf912efad79be6eef63a5619ff05969be3
   - dependabot/fetch-metadata@v3 → @25dd0e34f4fe68f24cc83900b1fe3fe149efef98
   - CatChen/eslint-suggestion-action@v4 → @86451179c10930875b0a29067fef90611e75d86d
   - CatChen/accept-to-ship-action@v0.8 → @c9dbb2e63814cfe28476809239bb487995a718e9

2. **missing-permissions (eslint.yml)**: Added top-level `permissions: {}` and job-level `permissions: contents: read, pull-requests: write`.

3. **missing-permissions (ship.yml)**: Added `permissions: {}` to the concurrency-group job.

4. **missing-permissions (release.yml)**: Added top-level `permissions: {}`, `permissions: {}` to the eslint reusable workflow call job, and `permissions: contents: write` to the release job.

