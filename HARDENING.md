<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag or branch refs instead of immutable 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action is compromised or the tag is moved. Failing references:
- ci.yml: actions/checkout@v3, actions/setup-node@v3
- coverage.yml: actions/checkout@v3, actions/setup-node@v3, coverallsapp/github-action@master
- pr-reviews-reminder-msteams-mention.yml: davideviolante/pr-reviews-reminder-action@master
- pr-reviews-reminder-msteams.yml: davideviolante/pr-reviews-reminder-action@master
All should be pinned to a full SHA, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3

Locations:

- `.github/workflows/ci.yml:12`
- `.github/workflows/ci.yml:13`
- `.github/workflows/coverage.yml:10`
- `.github/workflows/coverage.yml:11`
- `.github/workflows/coverage.yml:19`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:8`
- `.github/workflows/pr-reviews-reminder-msteams.yml:8`

### missing-permissions (severity: medium)

None of the workflow files declare a top-level `permissions:` block, and none of the individual jobs declare job-level permissions either. Without explicit permissions, workflows run with the default repository permissions (which may include write access to contents, packages, etc.), violating the principle of least privilege. Each workflow should declare the minimal permissions required, e.g. `permissions: read-all` at the top level and then grant only what each job needs.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/coverage.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 workflow files:

1. ci.yml: Pinned actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 # v3. Added top-level `permissions: contents: read`.

2. coverage.yml: Same checkout/setup-node pins. Pinned coverallsapp/github-action@master → @09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d # master. Added top-level `permissions: contents: read`.

3. pr-reviews-reminder-msteams-mention.yml: Pinned davideviolante/pr-reviews-reminder-action@master → @8966f84f476a05de86089b98b361d9c5114bc1c0 # master. Added top-level `permissions: contents: read, pull-requests: read`.

4. pr-reviews-reminder-msteams.yml: Same davideviolante pin. Added top-level `permissions: contents: read, pull-requests: read`.

All SHAs were resolved via lookup_action_sha and are immutable commit hashes. Original tag names preserved in comments for readability.

