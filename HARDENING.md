<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.9.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.9.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in ci.yml use mutable version tags instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced tags are moved. Failing references: `actions/checkout@v5` and `actions/setup-node@v5`.

Locations:

- `.github/workflows/ci.yml:11`
- `.github/workflows/ci.yml:12`

### unpinned-uses (severity: high)

All `uses:` references in coverage.yml use mutable version tags or branch names instead of pinned 40-character SHA digests, making the workflow vulnerable to supply-chain attacks. Failing references: `actions/checkout@v5`, `actions/setup-node@v5`, and `coverallsapp/github-action@master` (branch ref — especially dangerous).

Locations:

- `.github/workflows/coverage.yml:8`
- `.github/workflows/coverage.yml:9`
- `.github/workflows/coverage.yml:18`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and the single job (`build`) also has no `permissions:` key. Without explicit permissions, the workflow runs with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

coverage.yml has no top-level `permissions:` key and the single job (`build`) also has no `permissions:` key. The workflow uses `secrets.GITHUB_TOKEN` (passed to coverallsapp) but never restricts what scopes that token carries, violating the principle of least privilege.

Locations:

- `.github/workflows/coverage.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across ci.yml and coverage.yml:

1. ci.yml - Pinned actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 and actions/setup-node@v5 → @a0853c24544627f65ddf259abe73b1d18a591444. Added top-level `permissions: {}` for least privilege.

2. coverage.yml - Pinned actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09, actions/setup-node@v5 → @a0853c24544627f65ddf259abe73b1d18a591444, and coverallsapp/github-action@master → @09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d. Added top-level `permissions: {}` and job-level `permissions: contents: read` (minimum needed for checkout and Coveralls reporting).

