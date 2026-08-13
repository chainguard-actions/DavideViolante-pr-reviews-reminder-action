<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.8.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tags/branches instead of pinned 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tags are moved or overwritten.

.github/workflows/ci.yml:
  - uses: actions/checkout@v3
  - uses: actions/setup-node@v3

.github/workflows/coverage.yml:
  - uses: actions/checkout@v3
  - uses: actions/setup-node@v3
  - uses: coverallsapp/github-action@master  (branch ref — especially risky)

Locations:

- `.github/workflows/ci.yml:9`
- `.github/workflows/ci.yml:10`
- `.github/workflows/coverage.yml:9`
- `.github/workflows/coverage.yml:10`
- `.github/workflows/coverage.yml:18`

### missing-permissions (severity: medium)

Neither .github/workflows/ci.yml nor .github/workflows/coverage.yml defines a top-level `permissions:` block, and no job-level `permissions:` blocks are present either. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/coverage.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-char commit SHAs — actions/checkout@v3 → f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, coverallsapp/github-action@master → 09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d — with original tags preserved as inline comments. (2) Added top-level `permissions: {}` to both ci.yml and coverage.yml to enforce least-privilege; coverage.yml also has a job-level `permissions: contents: read` for the checkout step.

