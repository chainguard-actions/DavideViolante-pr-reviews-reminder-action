<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.4.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned full-length SHA digests, making the workflow vulnerable to supply-chain attacks if the referenced tag or branch is updated maliciously. Failing references: actions/checkout@v3, actions/setup-node@v3 (ci.yml lines 13–14); actions/checkout@v3, actions/setup-node@v3, coverallsapp/github-action@master (coverage.yml lines 9–10, 21); davideviolante/pr-reviews-reminder-action@master (pr-reviews-reminder-msteams-mention.yml line 9); davideviolante/pr-reviews-reminder-action@master (pr-reviews-reminder-msteams.yml line 9).

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:14`
- `.github/workflows/coverage.yml:9`
- `.github/workflows/coverage.yml:10`
- `.github/workflows/coverage.yml:21`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:9`
- `.github/workflows/pr-reviews-reminder-msteams.yml:9`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no job within them defines job-level permissions either. Without explicit permissions, workflows run with the default (potentially write) token permissions, violating the principle of least privilege. All four workflow files are affected: ci.yml, coverage.yml, pr-reviews-reminder-msteams-mention.yml, and pr-reviews-reminder-msteams.yml.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/coverage.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:1`
- `.github/workflows/pr-reviews-reminder-msteams.yml:1`

### hardcoded-credentials (severity: high)

Two workflow files contain a hardcoded Microsoft Teams incoming webhook URL assigned literally to `webhook-url:`. The URL embeds sensitive GUIDs and tokens (e.g. `webhookb2/a8d77ee9-e1d9-44ef-931d-a72a1bd94db4@962d0904-...`) that grant the ability to post messages to the Teams channel. These should be stored as GitHub Actions secrets and referenced via `${{ secrets.MSTEAMS_WEBHOOK_URL }}` instead of being committed in plaintext.

Locations:

- `.github/workflows/pr-reviews-reminder-msteams-mention.yml:11`
- `.github/workflows/pr-reviews-reminder-msteams.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, hardcoded-credentials

**Notes:**

Fixed all three findings across four workflow files: (1) Pinned all action references to full commit SHAs: actions/checkout@v3→a37ce91, actions/setup-node@v3→3235b87, coverallsapp/github-action@master→09b709c, davideviolante/pr-reviews-reminder-action@master→8966f84. (2) Added `permissions: {}` top-level block to all four workflow files. (3) Replaced hardcoded MS Teams webhook URL in both msteams workflow files with `${{ secrets.MSTEAMS_WEBHOOK_URL }}`.

