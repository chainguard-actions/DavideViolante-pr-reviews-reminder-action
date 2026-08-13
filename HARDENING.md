<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names (@v3, @master) instead of pinned full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved. Affected references: ci.yml — actions/checkout@v3 (line 13), actions/setup-node@v3 (line 14); coverage.yml — actions/checkout@v3 (line 9), actions/setup-node@v3 (line 10), coverallsapp/github-action@master (line 21); pr-reviews-reminder-msteams-mention.yml — davideviolante/pr-reviews-reminder-action@master (line 9); pr-reviews-reminder-msteams.yml — davideviolante/pr-reviews-reminder-action@master (line 9).

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:14`
- `.github/workflows/coverage.yml:9`
- `.github/workflows/coverage.yml:10`
- `.github/workflows/coverage.yml:21`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:9`
- `.github/workflows/pr-reviews-reminder-msteams.yml:9`

### missing-permissions (severity: medium)

None of the four workflow files declare a top-level `permissions:` block, and no job within them declares job-level permissions either. Without explicit permissions, workflows run with the default (often write-all) token permissions, violating the principle of least privilege. Each workflow should declare the minimal permissions required (e.g., `permissions: contents: read`).

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/coverage.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four workflow files: (1) Pinned all action references to full 40-char commit SHAs — actions/checkout@v3 → f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, coverallsapp/github-action@master → 09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d, davideviolante/pr-reviews-reminder-action@master → aa9d0a3077dfda0c4e3d0d3dd7bb936f1a5c5311. Original tags/branches preserved as inline comments. (2) Added top-level permissions blocks to all four workflows: ci.yml and coverage.yml get `contents: read`; the two pr-reviews-reminder workflows get `contents: read` and `pull-requests: read` since they need to read PR data.

