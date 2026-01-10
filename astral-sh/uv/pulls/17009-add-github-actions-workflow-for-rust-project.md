---
number: 17009
title: Add GitHub Actions workflow for Rust project
type: pull_request
state: closed
author: pohi99999
labels: []
assignees: []
base: main
head: main
created_at: 2025-12-06T00:49:54Z
updated_at: 2025-12-06T01:19:19Z
url: https://github.com/astral-sh/uv/pull/17009
synced_at: 2026-01-10T01:26:19Z
---

# Add GitHub Actions workflow for Rust project

---

_Pull request opened by @pohi99999 on 2025-12-06 00:49_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @Copilot by @pohi99999 on 2025-12-06 00:49_

---

_@github-advanced-security[bot] reviewed on 2025-12-06 00:50_

---

_Review comment by @github-advanced-security[bot] on `.github/workflows/rust.yml`:18 on 2025-12-06 00:50_

credential persistence through GitHub Actions artifacts

[Show more details](https://github.com/astral-sh/uv/security/code-scanning/187)

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:18 on 2025-12-06 00:53_

This GitHub Action should be pinned to a specific commit SHA rather than using a mutable version tag. Following the repository's security best practices, use the format:
```yaml
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
```
This prevents potential supply chain attacks where a tag could be moved to point to malicious code. All other workflows in this repository (ci.yml, publish-crates.yml, etc.) follow this pattern.
```suggestion
    - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:18 on 2025-12-06 00:53_

Missing Rust caching which significantly slows down CI runs. Add the Swatinem/rust-cache action after the checkout step:
```yaml
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
  with:
    persist-credentials: false
- uses: Swatinem/rust-cache@779680da715d629ac1d338a641029a2f4372abb5 # v2.8.2
  with:
    save-if: ${{ github.ref == 'refs/heads/main' }}
```
This caching strategy is consistently used in all Rust jobs in the repository's ci.yml workflow and can dramatically improve build times.
```suggestion
    - uses: actions/checkout@v4
    - uses: Swatinem/rust-cache@779680da715d629ac1d338a641029a2f4372abb5 # v2.8.2
      with:
        save-if: ${{ github.ref == 'refs/heads/main' }}
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:14 on 2025-12-06 00:53_

Missing timeout configuration for the build job. Without a timeout, jobs can hang indefinitely and waste CI resources. Add a timeout at the job level:
```yaml
jobs:
  build:
    timeout-minutes: 10
    runs-on: ubuntu-latest
```
The repository's ci.yml workflow uses timeout-minutes consistently across all jobs (e.g., `timeout-minutes: 10` for lint and clippy jobs).
```suggestion

    timeout-minutes: 10
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:18 on 2025-12-06 00:53_

The checkout action is missing the `persist-credentials: false` security setting. This prevents the GITHUB_TOKEN from being persisted in the git config, which could be exposed in subsequent steps:
```yaml
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
  with:
    persist-credentials: false
```
This security practice is consistently applied in all workflows in this repository (ci.yml, publish-crates.yml, etc.).
```suggestion
    - uses: actions/checkout@v4
      with:
        persist-credentials: false
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:10 on 2025-12-06 00:53_

[nitpick] Missing important environment variables used in the repository's CI. Consider adding these environment variables that are set in ci.yml for better reliability:
```yaml
env:
  CARGO_INCREMENTAL: 0
  CARGO_NET_RETRY: 10
  CARGO_TERM_COLOR: always
  RUSTUP_MAX_RETRIES: 10
  RUST_BACKTRACE: 1
```
These variables improve CI reliability by disabling incremental compilation (not useful in CI), enabling network retries, and providing better error diagnostics.
```suggestion
env:
  CARGO_INCREMENTAL: 0
  CARGO_NET_RETRY: 10
  CARGO_TERM_COLOR: always
  RUSTUP_MAX_RETRIES: 10
  RUST_BACKTRACE: 1
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:22 on 2025-12-06 00:53_

This workflow appears to be redundant with the existing comprehensive `.github/workflows/ci.yml` workflow, which already includes:
- Rust building and testing (cargo-test-linux, cargo-test-macos, cargo-test-windows jobs)
- Clippy linting (cargo-clippy jobs)
- Formatting checks (lint job with rustfmt)
- Multiple platform support
- Advanced features like cargo-nextest, Rust caching, and proper security controls

The ci.yml workflow is much more comprehensive and follows the repository's established patterns. Consider whether this new workflow serves a distinct purpose, or if it should be removed to avoid maintaining duplicate CI logic.
```suggestion
# (File deleted)
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:11 on 2025-12-06 00:53_

Missing concurrency controls to cancel in-progress workflow runs. This wastes CI resources when multiple commits are pushed in quick succession. Add concurrency configuration after the `env:` section:
```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref_name }}-${{ github.event.pull_request.number || github.sha }}
  cancel-in-progress: true
```
This pattern is used in the repository's ci.yml workflow to ensure efficient resource usage.
```suggestion

concurrency:
  group: ${{ github.workflow }}-${{ github.ref_name }}-${{ github.event.pull_request.number || github.sha }}
  cancel-in-progress: true
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:20 on 2025-12-06 00:53_

The cargo build command should include the `--locked` flag to ensure builds use the exact dependency versions from Cargo.lock. This prevents unexpected behavior from dependency updates during CI:
```yaml
run: cargo build --verbose --locked
```
This is a best practice for reproducible builds in CI environments.
```suggestion
      run: cargo build --verbose --locked
```

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:22 on 2025-12-06 00:53_

[nitpick] This workflow uses `cargo test` but the repository's standard testing tool is `cargo-nextest`, which provides better performance and output formatting. Consider using cargo-nextest instead:
```yaml
- name: Install cargo nextest
  uses: taiki-e/install-action@a416ddeedbd372e614cc1386e8b642692f66865e # v2.57.1
  with:
    tool: cargo-nextest
- name: Run tests
  run: cargo nextest run --verbose --workspace
```
All test jobs in ci.yml (cargo-test-linux, cargo-test-macos, cargo-test-windows) use cargo-nextest.
```suggestion
    - name: Install cargo nextest
      uses: taiki-e/install-action@a416ddeedbd372e614cc1386e8b642692f66865e # v2.57.1
      with:
        tool: cargo-nextest
    - name: Run tests
      run: cargo nextest run --verbose --workspace
```

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-12-06 00:53_

## Pull request overview

This PR adds a new GitHub Actions workflow for building and testing Rust code. However, the repository already has a comprehensive CI workflow (`.github/workflows/ci.yml`) that handles Rust building, testing, linting, and formatting across multiple platforms with advanced features and security controls.

### Key Observations
- The new workflow duplicates functionality already present in the existing ci.yml workflow
- Missing several security best practices (SHA-pinned actions, explicit permissions, credential persistence settings)
- Lacks performance optimizations (Rust caching, concurrency controls) that are standard in the repository





---

💡 <a href="/astral-sh/uv/new/main/.github/instructions?filename=*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

_Review comment by @Copilot on `.github/workflows/rust.yml`:8 on 2025-12-06 00:53_

Missing explicit permissions configuration. Following security best practices, workflows should use the principle of least privilege by explicitly declaring permissions. Add after the `on:` section:
```yaml
permissions: {}
```
The repository's ci.yml workflow uses `permissions: {}` to explicitly deny all permissions by default, only granting specific permissions where needed. This prevents accidental misuse of GitHub tokens.
```suggestion

permissions: {}
```

---

_Closed by @zanieb on 2025-12-06 01:19_

---
