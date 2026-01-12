```yaml
number: 16935
title: Use our org-wide Renovate preset
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/reconfigure
created_at: 2025-12-02T18:02:59Z
updated_at: 2025-12-02T18:28:57Z
url: https://github.com/astral-sh/uv/pull/16935
synced_at: 2026-01-12T16:12:31Z
```

# Use our org-wide Renovate preset

---

_@woodruffw_

## Summary

This applies a superset of the `recommended` preset, including our own cooldown policy.

## Test Plan

This PR has a special branch name that Renovate knows how to check.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-02 18:02_

---

_Label `internal` added by @woodruffw on 2025-12-02 18:03_

---

_Comment by @renovate[bot] on 2025-12-02 18:04_

### Reconfigure PR Results

This is a reconfigure PR comment to help you understand and re-configure your renovate bot settings. If this Reconfigure PR were to be merged, we'd expect to see the following outcome:


---
### Detected Package Files

 * `Cargo.toml` (cargo)
 * `crates/uv-auth/Cargo.toml` (cargo)
 * `crates/uv-bench/Cargo.toml` (cargo)
 * `crates/uv-bin-install/Cargo.toml` (cargo)
 * `crates/uv-build-backend/Cargo.toml` (cargo)
 * `crates/uv-build-frontend/Cargo.toml` (cargo)
 * `crates/uv-build/Cargo.toml` (cargo)
 * `crates/uv-cache-info/Cargo.toml` (cargo)
 * `crates/uv-cache-key/Cargo.toml` (cargo)
 * `crates/uv-cache/Cargo.toml` (cargo)
 * `crates/uv-cli/Cargo.toml` (cargo)
 * `crates/uv-client/Cargo.toml` (cargo)
 * `crates/uv-configuration/Cargo.toml` (cargo)
 * `crates/uv-console/Cargo.toml` (cargo)
 * `crates/uv-dev/Cargo.toml` (cargo)
 * `crates/uv-dirs/Cargo.toml` (cargo)
 * `crates/uv-dispatch/Cargo.toml` (cargo)
 * `crates/uv-distribution-filename/Cargo.toml` (cargo)
 * `crates/uv-distribution-types/Cargo.toml` (cargo)
 * `crates/uv-distribution/Cargo.toml` (cargo)
 * `crates/uv-extract/Cargo.toml` (cargo)
 * `crates/uv-flags/Cargo.toml` (cargo)
 * `crates/uv-fs/Cargo.toml` (cargo)
 * `crates/uv-git-types/Cargo.toml` (cargo)
 * `crates/uv-git/Cargo.toml` (cargo)
 * `crates/uv-globfilter/Cargo.toml` (cargo)
 * `crates/uv-install-wheel/Cargo.toml` (cargo)
 * `crates/uv-installer/Cargo.toml` (cargo)
 * `crates/uv-keyring/Cargo.toml` (cargo)
 * `crates/uv-logging/Cargo.toml` (cargo)
 * `crates/uv-macros/Cargo.toml` (cargo)
 * `crates/uv-metadata/Cargo.toml` (cargo)
 * `crates/uv-normalize/Cargo.toml` (cargo)
 * `crates/uv-once-map/Cargo.toml` (cargo)
 * `crates/uv-options-metadata/Cargo.toml` (cargo)
 * `crates/uv-pep440/Cargo.toml` (cargo)
 * `crates/uv-pep508/Cargo.toml` (cargo)
 * `crates/uv-performance-memory-allocator/Cargo.toml` (cargo)
 * `crates/uv-platform-tags/Cargo.toml` (cargo)
 * `crates/uv-platform/Cargo.toml` (cargo)
 * `crates/uv-preview/Cargo.toml` (cargo)
 * `crates/uv-publish/Cargo.toml` (cargo)
 * `crates/uv-pypi-types/Cargo.toml` (cargo)
 * `crates/uv-python/Cargo.toml` (cargo)
 * `crates/uv-redacted/Cargo.toml` (cargo)
 * `crates/uv-requirements-txt/Cargo.toml` (cargo)
 * `crates/uv-requirements/Cargo.toml` (cargo)
 * `crates/uv-resolver/Cargo.toml` (cargo)
 * `crates/uv-scripts/Cargo.toml` (cargo)
 * `crates/uv-settings/Cargo.toml` (cargo)
 * `crates/uv-shell/Cargo.toml` (cargo)
 * `crates/uv-small-str/Cargo.toml` (cargo)
 * `crates/uv-state/Cargo.toml` (cargo)
 * `crates/uv-static/Cargo.toml` (cargo)
 * `crates/uv-tool/Cargo.toml` (cargo)
 * `crates/uv-torch/Cargo.toml` (cargo)
 * `crates/uv-trampoline-builder/Cargo.toml` (cargo)
 * `crates/uv-trampoline/Cargo.toml` (cargo)
 * `crates/uv-types/Cargo.toml` (cargo)
 * `crates/uv-virtualenv/Cargo.toml` (cargo)
 * `crates/uv-warnings/Cargo.toml` (cargo)
 * `crates/uv-workspace/Cargo.toml` (cargo)
 * `crates/uv/Cargo.toml` (cargo)
 * `.github/workflows/build-binaries.yml` (github-actions)
 * `.github/workflows/build-docker.yml` (github-actions)
 * `.github/workflows/ci.yml` (github-actions)
 * `.github/workflows/publish-crates.yml` (github-actions)
 * `.github/workflows/publish-docs.yml` (github-actions)
 * `.github/workflows/publish-pypi.yml` (github-actions)
 * `.github/workflows/release.yml` (github-actions)
 * `.github/workflows/sync-python-releases.yml` (github-actions)
 * `.github/workflows/zizmor.yml` (github-actions)
 * `.pre-commit-config.yaml` (pre-commit)
 * `.github/workflows/ci.yml` (regex)
 * `docs/guides/integration/github.md` (regex)
 * `Cargo.toml` (regex)
 * `rust-toolchain.toml` (regex)

### Configuration Summary

Based on the default config's presets, Renovate will:

  - Hopefully safe environment variables to allow users to configure.
  - Show all Merge Confidence badges for pull requests.
  - Enable Renovate Dependency Dashboard creation.
  - Use semantic commit type `fix` for dependencies and `chore` for all others if semantic commits are in use.
  - Ignore `node_modules`, `bower_components`, `vendor` and various test/tests (except for nuget) directories.
  - Group known monorepo packages together.
  - Use curated list of recommended non-monorepo package groupings.
  - Show only the Age and Confidence Merge Confidence badges for pull requests.
  - Apply crowd-sourced package replacement rules.
  - Apply crowd-sourced workarounds for known problems with packages.
  - Update `_VERSION` environment variables in GitHub Action files.
  - Run Renovate on following schedule: * 0-3 * * 1


---

### What to Expect

With your current configuration, Renovate will create 57 Pull Requests:

<details>
<summary>Update actions/attest-build-provenance digest to ca0aaa1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/actions-attest-build-provenance-digest`
  - Merge into: `main`
  - Upgrade actions/attest-build-provenance to `ca0aaa1889e301c8331fbdb338d9475431b75b13`


</details>

<details>
<summary>Update actions/checkout digest to 8e8c483</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/actions-checkout-digest`
  - Merge into: `main`
  - Upgrade actions/checkout to `8e8c483db84b4bee98b60c0593521ed34d9990e8`


</details>

<details>
<summary>Update dependency astral-sh/uv to v0.9.14</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/astral-sh-uv-0.x`
  - Merge into: `main`
  - Upgrade [astral-sh/uv](https://redirect.github.com/astral-sh/uv) to `0.9.14`


</details>

<details>
<summary>Update EmbarkStudios/cargo-deny-action action to v2.0.14</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/embarkstudios-cargo-deny-action-2.x`
  - Merge into: `main`
  - Upgrade [EmbarkStudios/cargo-deny-action](https://redirect.github.com/EmbarkStudios/cargo-deny-action) to `76cd80eb775d7bbbd2d80292136d74d39e1b4918`


</details>

<details>
<summary>Update peter-evans/create-pull-request action to v7.0.9</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/peter-evans-create-pull-request-7.x`
  - Merge into: `main`
  - Upgrade [peter-evans/create-pull-request](https://redirect.github.com/peter-evans/create-pull-request) to `84ae59a2cdc2258d6fa0732dd66352dddae2a412`


</details>

<details>
<summary>Update Rust crate async-compression to v0.4.34</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/async-compression-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [async-compression](https://redirect.github.com/Nullus157/async-compression) to `0.4.34`


</details>

<details>
<summary>Update Rust crate hyper-util to v0.1.18</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/hyper-util-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [hyper-util](https://redirect.github.com/hyperium/hyper-util) to `0.1.18`


</details>

<details>
<summary>Update Rust crate reqwest to v0.12.24</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/reqwest-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [reqwest](https://redirect.github.com/seanmonstar/reqwest) to `0.12.24`


</details>

<details>
<summary>Update Rust crate test-log to v0.2.19</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/test-log-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [test-log](https://redirect.github.com/d-e-s-o/test-log) to `0.2.19`


</details>

<details>
<summary>Update Rust crate tikv-jemallocator to v0.6.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/tikv-jemallocator-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [tikv-jemallocator](https://redirect.github.com/tikv/jemallocator) to `0.6.1`


</details>

<details>
<summary>Update Rust crate tokio-rustls to v0.26.4</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/tokio-rustls-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [tokio-rustls](https://redirect.github.com/rustls/tokio-rustls) to `0.26.4`


</details>

<details>
<summary>Update Rust crate version-ranges to v0.1.4</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/pubgrub`
  - Merge into: `main`
  - Upgrade [version-ranges](https://redirect.github.com/astral-sh/pubgrub) to `0.1.4`


</details>

<details>
<summary>Update Swatinem/rust-cache action to v2.8.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/swatinem-rust-cache-2.x`
  - Merge into: `main`
  - Upgrade [Swatinem/rust-cache](https://redirect.github.com/Swatinem/rust-cache) to `f13886b937689c021905a6b90929199931d60db1`


</details>

<details>
<summary>Update tokio-tracing monorepo</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/tokio-tracing-monorepo`
  - Merge into: `main`
  - Upgrade [tracing](https://redirect.github.com/tokio-rs/tracing) to `0.1.43`
  - Upgrade [tracing-subscriber](https://redirect.github.com/tokio-rs/tracing) to `0.3.22`


</details>

<details>
<summary>Update CodSpeedHQ/action action to v4.4.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/codspeedhq-action-4.x`
  - Merge into: `main`
  - Upgrade [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) to `346a2d8a8d9d38909abd0bc3d23f773110f076ad`


</details>

<details>
<summary>Update crate-ci/typos action to v1.39.2</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/crate-ci-typos-1.x`
  - Merge into: `main`
  - Upgrade [crate-ci/typos](https://redirect.github.com/crate-ci/typos) to `626c4bedb751ce0b7f03262ca97ddda9a076ae1c`


</details>

<details>
<summary>Update dependency python to v3.14.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/python-3.x`
  - Merge into: `main`
  - Upgrade [python](https://redirect.github.com/actions/python-versions) to `3.14`
  - Upgrade [python](https://redirect.github.com/actions/python-versions) to `3.14.0`


</details>

<details>
<summary>Update docker/login-action action to v3.6.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/docker-login-action-3.x`
  - Merge into: `main`
  - Upgrade [docker/login-action](https://redirect.github.com/docker/login-action) to `5e57cd118135c172c3672efd75eb46360885c0ef`


</details>

<details>
<summary>Update docker/metadata-action action to v5.9.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/docker-metadata-action-5.x`
  - Merge into: `main`
  - Upgrade [docker/metadata-action](https://redirect.github.com/docker/metadata-action) to `318604b99e75e41977312d83839a89be02ca4893`


</details>

<details>
<summary>Update MSRV to v1.91.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/msrv`
  - Merge into: `main`
  - Upgrade [msrv](https://redirect.github.com/rust-lang/rust) to `1.91.1`


</details>

<details>
<summary>Update pre-commit dependencies</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/pre-commit-dependencies`
  - Merge into: `main`
  - Upgrade [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) to `v0.14.6`
  - Upgrade [crate-ci/typos](https://redirect.github.com/crate-ci/typos) to `v1.39.2`


</details>

<details>
<summary>Update Rust crate assert_cmd to v2.1.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/assert_cmd-2.x-lockfile`
  - Merge into: `main`
  - Upgrade [assert_cmd](https://redirect.github.com/assert-rs/assert_cmd) to `2.1.1`


</details>

<details>
<summary>Update Rust crate backon to v1.6.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/backon-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [backon](https://redirect.github.com/Xuanwo/backon) to `1.6.0`


</details>

<details>
<summary>Update Rust crate bitflags to v2.10.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/bitflags-2.x-lockfile`
  - Merge into: `main`
  - Upgrade [bitflags](https://redirect.github.com/bitflags/bitflags) to `2.10.0`


</details>

<details>
<summary>Update Rust crate criterion to v4.1.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/criterion-4.x-lockfile`
  - Merge into: `main`
  - Upgrade [criterion](https://redirect.github.com/CodSpeedHQ/codspeed-rust) to `4.1.0`


</details>

<details>
<summary>Update Rust crate csv to v1.4.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/csv-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [csv](https://redirect.github.com/BurntSushi/rust-csv) to `1.4.0`


</details>

<details>
<summary>Update Rust crate embed-manifest to v1.5.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/embed-manifest-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [embed-manifest](https://codeberg.org/carey/embed-manifest) to `1.5.0`


</details>

<details>
<summary>Update Rust crate fs-err to v3.2.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/fs-err-3.x-lockfile`
  - Merge into: `main`
  - Upgrade [fs-err](https://redirect.github.com/andrewhickman/fs-err) to `3.2.0`


</details>

<details>
<summary>Update Rust crate http to v1.4.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/http-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [http](https://redirect.github.com/hyperium/http) to `1.4.0`


</details>

<details>
<summary>Update Rust crate hyper to v1.8.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/hyper-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [hyper](https://redirect.github.com/hyperium/hyper) to `1.8.1`


</details>

<details>
<summary>Update Rust crate insta to v1.44.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/insta-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [insta](https://redirect.github.com/mitsuhiko/insta) to `1.44.1`


</details>

<details>
<summary>Update Rust crate junction to v1.3.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/junction-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [junction](https://redirect.github.com/tesuji/junction) to `1.3.0`


</details>

<details>
<summary>Update Rust crate procfs to 0.18.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/procfs-0.x`
  - Merge into: `main`
  - Upgrade [procfs](https://redirect.github.com/eminence/procfs) to `0.18.0`


</details>

<details>
<summary>Update Rust crate rayon to v1.11.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/rayon-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [rayon](https://redirect.github.com/rayon-rs/rayon) to `1.11.0`


</details>

<details>
<summary>Update Rust crate regex to v1.12.2</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/regex-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [regex](https://redirect.github.com/rust-lang/regex) to `1.12.2`


</details>

<details>
<summary>Update Rust crate resvg to 0.45.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/resvg-0.x`
  - Merge into: `main`
  - Upgrade [resvg](https://redirect.github.com/linebender/resvg) to `0.45.0`


</details>

<details>
<summary>Update Rust crate rustix to v1.1.2</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/rustix-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [rustix](https://redirect.github.com/bytecodealliance/rustix) to `1.1.2`


</details>

<details>
<summary>Update Rust crate schemars to v1.1.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/schemars-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [schemars](https://redirect.github.com/GREsau/schemars) to `1.1.0`


</details>

<details>
<summary>Update Rust crate secret-service to v5.1.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/secret-service-5.x-lockfile`
  - Merge into: `main`
  - Upgrade [secret-service](https://redirect.github.com/hwchen/secret-service-rs) to `5.1.0`


</details>

<details>
<summary>Update Rust crate security-framework to v3.5.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/security-framework-3.x-lockfile`
  - Merge into: `main`
  - Upgrade [security-framework](https://redirect.github.com/kornelski/rust-security-framework) to `3.5.1`


</details>

<details>
<summary>Update Rust crate spdx to 0.13.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/spdx-0.x`
  - Merge into: `main`
  - Upgrade [spdx](https://redirect.github.com/EmbarkStudios/spdx) to `0.13.0`


</details>

<details>
<summary>Update Rust crate tempfile to v3.23.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/tempfile-3.x-lockfile`
  - Merge into: `main`
  - Upgrade [tempfile](https://redirect.github.com/Stebalien/tempfile) to `3.23.0`


</details>

<details>
<summary>Update Rust crate tokio to v1.48.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/tokio-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [tokio](https://redirect.github.com/tokio-rs/tokio) to `1.48.0`


</details>

<details>
<summary>Update Rust crate uuid to v1.18.1</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/uuid-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [uuid](https://redirect.github.com/uuid-rs/uuid) to `1.18.1`


</details>

<details>
<summary>Update Rust crate windows to 0.62.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/windows-0.x`
  - Merge into: `main`
  - Upgrade [windows](https://redirect.github.com/microsoft/windows-rs) to `0.62.0`


</details>

<details>
<summary>Update Rust crate windows-registry to 0.6.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/windows-registry-0.x`
  - Merge into: `main`
  - Upgrade [windows-registry](https://redirect.github.com/microsoft/windows-rs) to `0.6.0`


</details>

<details>
<summary>Update Rust crate wmi to 0.18.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/wmi-0.x`
  - Merge into: `main`
  - Upgrade [wmi](https://redirect.github.com/ohadravid/wmi-rs) to `0.18.0`


</details>

<details>
<summary>Update taiki-e/install-action action to v2.62.57</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/taiki-e-install-action-2.x`
  - Merge into: `main`
  - Upgrade [taiki-e/install-action](https://redirect.github.com/taiki-e/install-action) to `763e3324d4fd026c9bd284c504378585777a87d5`


</details>

<details>
<summary>Update zizmorcore/zizmor-action action to v0.3.0</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/zizmorcore-zizmor-action-0.x`
  - Merge into: `main`
  - Upgrade [zizmorcore/zizmor-action](https://redirect.github.com/zizmorcore/zizmor-action) to `e639db99335bc9038abc0e066dfcd72e23d26fb4`


</details>

<details>
<summary>Update actions/attest-build-provenance action to v3</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/actions-attest-build-provenance-3.x`
  - Merge into: `main`
  - Upgrade [actions/attest-build-provenance](https://redirect.github.com/actions/attest-build-provenance) to `977bb373ede98d70efdf65b84cb5f73e068dcc2a`


</details>

<details>
<summary>Update actions/checkout action to v6</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/actions-checkout-6.x`
  - Merge into: `main`
  - Upgrade [actions/checkout](https://redirect.github.com/actions/checkout) to `1af3b93b6815bc44a9784bd300feb67ff0d1eeb3`


</details>

<details>
<summary>Update actions/setup-python action to v6</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/actions-setup-python-6.x`
  - Merge into: `main`
  - Upgrade [actions/setup-python](https://redirect.github.com/actions/setup-python) to `83679a892e2d95755f2dac6acb0bfd1e9ac5d548`


</details>

<details>
<summary>Update Artifact GitHub Actions dependencies</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/artifact-github-actions-dependencies`
  - Merge into: `main`
  - Upgrade actions/download-artifact to `f093f21ca4cfa7c75ccbbc2be54da76a0c7e1f05`
  - Upgrade [actions/download-artifact](https://redirect.github.com/actions/download-artifact) to `018cc2cf5baa6db3ef3c5f8a56943fffe632ef53`
  - Upgrade actions/upload-artifact to `330a01c490aca151604b8cf639adc76d48f6c5d4`
  - Upgrade [actions/upload-artifact](https://redirect.github.com/actions/upload-artifact) to `330a01c490aca151604b8cf639adc76d48f6c5d4`


</details>

<details>
<summary>Update astral-sh/setup-uv action to v7</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/astral-sh-setup-uv-7.x`
  - Merge into: `main`
  - Upgrade [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) to `1e862dfacbd1d6d858c55d9b792c756523627244`


</details>

<details>
<summary>Update debian Docker tag to v13</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/debian-13.x`
  - Merge into: `main`
  - Upgrade debian to `trixie`


</details>

<details>
<summary>Update documentation references to actions/checkout to v6</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/docs-actions-checkout-6.x`
  - Merge into: `main`
  - Upgrade [actions/checkout](https://redirect.github.com/actions/checkout) to `v6`


</details>

<details>
<summary>Update fedora Docker tag to v44</summary>

  - Schedule: ["* 0-3 * * 1"]
  - Branch name: `renovate/fedora-44.x`
  - Merge into: `main`
  - Upgrade fedora to `44`


</details>




---

_Comment by @woodruffw on 2025-12-02 18:04_

> * Hopefully safe environment variables to allow users to configure.

wut

---

_Comment by @woodruffw on 2025-12-02 18:08_

For comparison, the same change on Ruff appears to have made the correct dashboard changes (the dashboard now shows appropriate pending sections for cooldowns): https://github.com/astral-sh/ruff/pull/21759

---

_Review requested from @zanieb by @woodruffw on 2025-12-02 18:08_

---

_@zanieb approved on 2025-12-02 18:11_

---

_@konstin approved on 2025-12-02 18:11_

---

_@konstin reviewed on 2025-12-02 18:12_

---

_Review comment by @konstin on `.github/renovate.json5`:14 on 2025-12-02 18:12_

We can remove this now

---

_@woodruffw reviewed on 2025-12-02 18:15_

---

_Review comment by @woodruffw on `.github/renovate.json5`:14 on 2025-12-02 18:15_

Removed! I probably need to do a larger pass once the preset config has more stuff in it.

---

_Merged by @woodruffw on 2025-12-02 18:28_

---

_Closed by @woodruffw on 2025-12-02 18:28_

---

_Branch deleted on 2025-12-02 18:28_

---
