<!-- markdownlint-disable -->

# Hardening Report: DavideViolante--pr-reviews-reminder-action/v2.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **DavideViolante--pr-reviews-reminder-action/v2.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tag-based refs instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the repository is compromised.

In ci.yml:
  - uses: actions/checkout@v3
  - uses: actions/setup-node@v3

In coverage.yml:
  - uses: actions/checkout@v3
  - uses: actions/setup-node@v3
  - uses: coverallsapp/github-action@master  (branch ref — especially dangerous)

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:11`
- `.github/workflows/coverage.yml:9`
- `.github/workflows/coverage.yml:10`
- `.github/workflows/coverage.yml:18`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` key, and no individual job within them declares a `permissions:` key either. Without explicit permissions, GitHub Actions defaults to broad repository permissions (read/write on most scopes for GITHUB_TOKEN), violating the principle of least privilege. Each workflow should declare the minimal set of permissions required, e.g.:

```yaml
permissions:
  contents: read
```

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/coverage.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-char SHAs — actions/checkout@v3 → a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610, coverallsapp/github-action@master → 09b709cf6a16e30b0808ba050c7a6e8a5ef13f8d — with original tag/branch preserved as inline comments. (2) Added top-level `permissions: contents: read` to both ci.yml and coverage.yml to enforce least-privilege access for GITHUB_TOKEN.

