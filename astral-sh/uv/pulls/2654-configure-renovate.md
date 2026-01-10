```yaml
number: 2654
title: Configure Renovate
type: pull_request
state: closed
author: renovate
labels: []
assignees: []
base: main
head: renovate/configure
created_at: 2024-03-25T19:06:19Z
updated_at: 2024-03-25T19:27:57Z
url: https://github.com/astral-sh/uv/pull/2654
synced_at: 2026-01-10T14:49:08Z
```

# Configure Renovate

---

_Pull request opened by @renovate on 2024-03-25 19:06_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

Welcome to [Renovate](https://togithub.com/renovatebot/renovate)! This is an onboarding PR to help you understand and configure settings before regular Pull Requests begin.

🚦 To activate Renovate, merge this Pull Request. To disable Renovate, simply close this Pull Request unmerged.



---
### Detected Package Files

 * `Cargo.toml` (cargo)
 * `crates/bench/Cargo.toml` (cargo)
 * `crates/cache-key/Cargo.toml` (cargo)
 * `crates/distribution-filename/Cargo.toml` (cargo)
 * `crates/distribution-types/Cargo.toml` (cargo)
 * `crates/install-wheel-rs/Cargo.toml` (cargo)
 * `crates/once-map/Cargo.toml` (cargo)
 * `crates/pep440-rs/Cargo.toml` (cargo)
 * `crates/pep508-rs/Cargo.toml` (cargo)
 * `crates/platform-tags/Cargo.toml` (cargo)
 * `crates/pypi-types/Cargo.toml` (cargo)
 * `crates/requirements-txt/Cargo.toml` (cargo)
 * `crates/uv-auth/Cargo.toml` (cargo)
 * `crates/uv-build/Cargo.toml` (cargo)
 * `crates/uv-cache/Cargo.toml` (cargo)
 * `crates/uv-client/Cargo.toml` (cargo)
 * `crates/uv-dev/Cargo.toml` (cargo)
 * `crates/uv-dispatch/Cargo.toml` (cargo)
 * `crates/uv-distribution/Cargo.toml` (cargo)
 * `crates/uv-extract/Cargo.toml` (cargo)
 * `crates/uv-fs/Cargo.toml` (cargo)
 * `crates/uv-git/Cargo.toml` (cargo)
 * `crates/uv-installer/Cargo.toml` (cargo)
 * `crates/uv-interpreter/Cargo.toml` (cargo)
 * `crates/uv-normalize/Cargo.toml` (cargo)
 * `crates/uv-requirements/Cargo.toml` (cargo)
 * `crates/uv-resolver/Cargo.toml` (cargo)
 * `crates/uv-traits/Cargo.toml` (cargo)
 * `crates/uv-trampoline/Cargo.toml` (cargo)
 * `crates/uv-virtualenv/Cargo.toml` (cargo)
 * `crates/uv-warnings/Cargo.toml` (cargo)
 * `crates/uv/Cargo.toml` (cargo)
 * `scripts/packages/maturin_editable/Cargo.toml` (cargo)
 * `Dockerfile` (dockerfile)
 * `crates/uv-dev/builder.dockerfile` (dockerfile)
 * `.github/workflows/build-binaries.yml` (github-actions)
 * `.github/workflows/build-docker.yml` (github-actions)
 * `.github/workflows/ci.yml` (github-actions)
 * `.github/workflows/publish-pypi.yml` (github-actions)
 * `.github/workflows/release.yml` (github-actions)
 * `pyproject.toml` (pep621)
 * `scripts/packages/black_editable/pyproject.toml` (pep621)
 * `scripts/packages/deptry_reproducer/pyproject.toml` (pep621)
 * `scripts/packages/hatchling_editable/pyproject.toml` (pep621)
 * `scripts/packages/maturin_editable/pyproject.toml` (pep621)
 * `scripts/packages/poetry_editable/pyproject.toml` (pep621)
 * `scripts/packages/root_editable/pyproject.toml` (pep621)
 * `scripts/packages/setuptools_editable/pyproject.toml` (pep621)
 * `scripts/bench/requirements.txt` (pip_requirements)
 * `scripts/benchmarks/requirements-large.txt` (pip_requirements)
 * `scripts/benchmarks/requirements.txt` (pip_requirements)
 * `scripts/release/requirements.txt` (pip_requirements)
 * `scripts/scenarios/requirements.txt` (pip_requirements)
 * `scripts/packages/poetry_editable/pyproject.toml` (poetry)

### Configuration Summary

Based on the default config's presets, Renovate will:

  - Start dependency updates only once this onboarding PR is merged
  - Show all Merge Confidence badges for pull requests.
  - Enable Renovate Dependency Dashboard creation.
  - Use semantic commit type `fix` for dependencies and `chore` for all others if semantic commits are in use.
  - Ignore `node_modules`, `bower_components`, `vendor` and various test/tests directories.
  - Group known monorepo packages together.
  - Use curated list of recommended non-monorepo package groupings.
  - Apply crowd-sourced package replacement rules.
  - Apply crowd-sourced workarounds for known problems with packages.

🔡 Do you want to change how Renovate upgrades your dependencies? Add your custom config to `renovate.json` in this branch. Renovate will update the Pull Request description the next time it runs.

---

### What to Expect

With your current configuration, Renovate will create 108 Pull Requests:

<details>
<summary>Update dependency jinja2 to v3.1.3 [SECURITY]</summary>

  - Branch name: `renovate/pypi-jinja2-vulnerability`
  - Merge into: `main`
  - Upgrade jinja2 to `==3.1.3`


</details>

<details>
<summary>Update dependency black to v24 [SECURITY]</summary>

  - Branch name: `renovate/pypi-black-vulnerability`
  - Merge into: `main`
  - Upgrade [black](https://togithub.com/psf/black) to `==24.3.0`


</details>

<details>
<summary>Update Rust crate anyhow to 1.0.81</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/anyhow-1.x`
  - Merge into: `main`
  - Upgrade [anyhow](https://togithub.com/dtolnay/anyhow) to `1.0.81`


</details>

<details>
<summary>Update Rust crate assert_fs to 1.1.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/assert_fs-1.x`
  - Merge into: `main`
  - Upgrade [assert_fs](https://togithub.com/assert-rs/assert_fs) to `1.1.1`


</details>

<details>
<summary>Update Rust crate async-trait to 0.1.79</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/async-trait-0.x`
  - Merge into: `main`
  - Upgrade [async-trait](https://togithub.com/dtolnay/async-trait) to `0.1.79`


</details>

<details>
<summary>Update Rust crate axoupdater to 0.3.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/axoupdater-0.x`
  - Merge into: `main`
  - Upgrade [axoupdater](https://togithub.com/axodotdev/axoupdater) to `0.3.3`


</details>

<details>
<summary>Update Rust crate cargo-util to 0.2.10</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/cargo-util-0.x`
  - Merge into: `main`
  - Upgrade [cargo-util](https://togithub.com/rust-lang/cargo) to `0.2.10`


</details>

<details>
<summary>Update Rust crate chrono to 0.4.35</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/chrono-0.x`
  - Merge into: `main`
  - Upgrade [chrono](https://togithub.com/chronotope/chrono) to `0.4.35`


</details>

<details>
<summary>Update Rust crate either to 1.10.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/either-1.x`
  - Merge into: `main`
  - Upgrade [either](https://togithub.com/rayon-rs/either) to `1.10.0`


</details>

<details>
<summary>Update Rust crate git2 to 0.18.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/git2-0.x`
  - Merge into: `main`
  - Upgrade [git2](https://togithub.com/rust-lang/git2-rs) to `0.18.3`


</details>

<details>
<summary>Update Rust crate indexmap to 2.2.6</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/indexmap-2.x`
  - Merge into: `main`
  - Upgrade [indexmap](https://togithub.com/indexmap-rs/indexmap) to `2.2.6`


</details>

<details>
<summary>Update Rust crate indicatif to 0.17.8</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/indicatif-0.x`
  - Merge into: `main`
  - Upgrade [indicatif](https://togithub.com/console-rs/indicatif) to `0.17.8`


</details>

<details>
<summary>Update Rust crate indoc to 2.0.5</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/indoc-2.x`
  - Merge into: `main`
  - Upgrade [indoc](https://togithub.com/dtolnay/indoc) to `2.0.5`


</details>

<details>
<summary>Update Rust crate mailparse to 0.14.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/mailparse-0.x`
  - Merge into: `main`
  - Upgrade [mailparse](https://togithub.com/staktrace/mailparse) to `0.14.1`


</details>

<details>
<summary>Update Rust crate predicates to 3.1.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/predicates-3.x`
  - Merge into: `main`
  - Upgrade [predicates](https://togithub.com/assert-rs/predicates-rs) to `3.1.0`


</details>

<details>
<summary>Update Rust crate pyo3 to 0.20.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pyo3-0.x`
  - Merge into: `main`
  - Upgrade [pyo3](https://togithub.com/pyo3/pyo3) to `0.20.3`


</details>

<details>
<summary>Update Rust crate regex to 1.10.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/regex-1.x`
  - Merge into: `main`
  - Upgrade [regex](https://togithub.com/rust-lang/regex) to `1.10.4`


</details>

<details>
<summary>Update Rust crate reqwest-middleware to 0.2.5</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/reqwest-middleware-0.x`
  - Merge into: `main`
  - Upgrade [reqwest-middleware](https://togithub.com/TrueLayer/reqwest-middleware) to `0.2.5`


</details>

<details>
<summary>Update Rust crate rkyv to 0.7.44</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rkyv-0.x`
  - Merge into: `main`
  - Upgrade [rkyv](https://togithub.com/rkyv/rkyv) to `0.7.44`


</details>

<details>
<summary>Update Rust crate tempfile to 3.10.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/tempfile-3.x`
  - Merge into: `main`
  - Upgrade [tempfile](https://togithub.com/Stebalien/tempfile) to `3.10.1`


</details>

<details>
<summary>Update Rust crate thiserror to 1.0.58</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/thiserror-1.x`
  - Merge into: `main`
  - Upgrade [thiserror](https://togithub.com/dtolnay/thiserror) to `1.0.58`


</details>

<details>
<summary>Update Rust crate tl to 0.7.8</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/tl-0.x`
  - Merge into: `main`
  - Upgrade [tl](https://togithub.com/y21/tl) to `0.7.8`


</details>

<details>
<summary>Update Rust crate tokio to 1.36.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/tokio-1.x`
  - Merge into: `main`
  - Upgrade [tokio](https://togithub.com/tokio-rs/tokio) to `1.36.0`


</details>

<details>
<summary>Update Rust crate tokio-stream to 0.1.15</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/tokio-stream-0.x`
  - Merge into: `main`
  - Upgrade [tokio-stream](https://togithub.com/tokio-rs/tokio) to `0.1.15`


</details>

<details>
<summary>Update Rust crate which to 6.0.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/which-6.x`
  - Merge into: `main`
  - Upgrade [which](https://togithub.com/harryfei/which-rs) to `6.0.1`


</details>

<details>
<summary>Update dependency dill to v0.3.8</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/dill-0.x`
  - Merge into: `main`
  - Upgrade [dill](https://togithub.com/uqfoundation/dill) to `==0.3.8`


</details>

<details>
<summary>Update dependency django to v5.0.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/django-5.x`
  - Merge into: `main`
  - Upgrade [django](https://togithub.com/django/django) to `==5.0.3`


</details>

<details>
<summary>Update dependency filelock to v3.13.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/filelock-3.x`
  - Merge into: `main`
  - Upgrade [filelock](https://togithub.com/tox-dev/py-filelock) to `==3.13.3`


</details>

<details>
<summary>Update dependency hishel to v0.0.24</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/hishel-0.x`
  - Merge into: `main`
  - Upgrade [hishel](https://togithub.com/karpetrosyan/hishel) to `==0.0.24`


</details>

<details>
<summary>Update dependency jaraco-classes to v3.3.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/jaraco-classes-3.x`
  - Merge into: `main`
  - Upgrade [jaraco-classes](https://togithub.com/jaraco/jaraco.classes) to `==3.3.1`


</details>

<details>
<summary>Update dependency keyring to v24.3.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/keyring-24.x`
  - Merge into: `main`
  - Upgrade [keyring](https://togithub.com/jaraco/keyring) to `==24.3.1`


</details>

<details>
<summary>Update dependency lsprotocol to v2023.0.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/lsprotocol-2023.x`
  - Merge into: `main`
  - Upgrade [lsprotocol](https://togithub.com/microsoft/lsprotocol) to `==2023.0.1`


</details>

<details>
<summary>Update dependency markupsafe to v2.1.5</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/markupsafe-2.x`
  - Merge into: `main`
  - Upgrade markupsafe to `==2.1.5`


</details>

<details>
<summary>Update dependency msgpack to v1.0.8</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/msgpack-1.x`
  - Merge into: `main`
  - Upgrade [msgpack](https://togithub.com/msgpack/msgpack-python) to `==1.0.8`


</details>

<details>
<summary>Update dependency numpy to v1.26.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/numpy-1.x`
  - Merge into: `main`
  - Upgrade [numpy](https://togithub.com/numpy/numpy) to `==1.26.4`


</details>

<details>
<summary>Update dependency pdm to v2.12.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pdm-2.x`
  - Merge into: `main`
  - Upgrade [pdm](https://togithub.com/pdm-project/pdm) to `==2.12.4`


</details>

<details>
<summary>Update dependency pycodestyle to v2.11.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pycodestyle-2.x`
  - Merge into: `main`
  - Upgrade pycodestyle to `==2.11.1`


</details>

<details>
<summary>Update dependency pydantic to v2.6.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pydantic-2.x`
  - Merge into: `main`
  - Upgrade [pydantic](https://togithub.com/pydantic/pydantic) to `==2.6.4`


</details>

<details>
<summary>Update dependency pytest to v7.4.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pytest-7.x`
  - Merge into: `main`
  - Upgrade [pytest](https://togithub.com/pytest-dev/pytest) to `==7.4.4`


</details>

<details>
<summary>Update dependency pyupgrade to v3.15.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pyupgrade-3.x`
  - Merge into: `main`
  - Upgrade [pyupgrade](https://togithub.com/asottile/pyupgrade) to `==3.15.2`


</details>

<details>
<summary>Update dependency rooster-blue to v0.0.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rooster-blue-0.x`
  - Merge into: `main`
  - Upgrade [rooster-blue](https://togithub.com/zanieb/rooster) to `==0.0.4`


</details>

<details>
<summary>Update dependency sniffio to v1.3.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/sniffio-1.x`
  - Merge into: `main`
  - Upgrade [sniffio](https://togithub.com/python-trio/sniffio) to `==1.3.1`


</details>

<details>
<summary>Update dependency tomlkit to v0.12.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/tomlkit-0.x`
  - Merge into: `main`
  - Upgrade [tomlkit](https://togithub.com/sdispater/tomlkit) to `==0.12.4`


</details>

<details>
<summary>Update dependency virtualenv to v20.25.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/virtualenv-20.x`
  - Merge into: `main`
  - Upgrade [virtualenv](https://togithub.com/pypa/virtualenv) to `==20.25.1`


</details>

<details>
<summary>Update Rust crate async-recursion to 1.1.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/async-recursion-1.x`
  - Merge into: `main`
  - Upgrade [async-recursion](https://togithub.com/dcchut/async-recursion) to `1.1.0`


</details>

<details>
<summary>Update Rust crate base64 to 0.22.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/base64-0.x`
  - Merge into: `main`
  - Upgrade [base64](https://togithub.com/marshallpierce/rust-base64) to `0.22.0`


</details>

<details>
<summary>Update Rust crate os_info to 3.8.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/os_info-3.x`
  - Merge into: `main`
  - Upgrade [os_info](https://togithub.com/stanislav-tkach/os_info) to `3.8.2`


</details>

<details>
<summary>Update Rust crate rayon to 1.10.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rayon-1.x`
  - Merge into: `main`
  - Upgrade [rayon](https://togithub.com/rayon-rs/rayon) to `1.10.0`


</details>

<details>
<summary>Update Rust crate reqwest to 0.12.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/reqwest-0.x`
  - Merge into: `main`
  - Upgrade [reqwest](https://togithub.com/seanmonstar/reqwest) to `0.12.2`


</details>

<details>
<summary>Update Rust crate reqwest-retry to 0.4.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/reqwest-retry-0.x`
  - Merge into: `main`
  - Upgrade [reqwest-retry](https://togithub.com/TrueLayer/reqwest-middleware) to `0.4.0`


</details>

<details>
<summary>Update Rust crate resvg to 0.40.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/resvg-0.x`
  - Merge into: `main`
  - Upgrade [resvg](https://togithub.com/RazrFalcon/resvg) to `0.40.0`


</details>

<details>
<summary>Update Rust crate rustls to 0.23.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rustls-0.x`
  - Merge into: `main`
  - Upgrade [rustls](https://togithub.com/rustls/rustls) to `0.23.4`


</details>

<details>
<summary>Update Rust crate rustls-native-certs to 0.7.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rustls-native-certs-0.x`
  - Merge into: `main`
  - Upgrade [rustls-native-certs](https://togithub.com/rustls/rustls-native-certs) to `0.7.0`


</details>

<details>
<summary>Update Rust crate webpki-roots to 0.26.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/webpki-roots-0.x`
  - Merge into: `main`
  - Upgrade [webpki-roots](https://togithub.com/rustls/webpki-roots) to `0.26.1`


</details>

<details>
<summary>Update dependency asgiref to v3.8.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/asgiref-3.x`
  - Merge into: `main`
  - Upgrade [asgiref](https://togithub.com/django/asgiref) to `==3.8.1`


</details>

<details>
<summary>Update dependency astroid to v3.1.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/astroid-3.x`
  - Merge into: `main`
  - Upgrade [astroid](https://togithub.com/pylint-dev/astroid) to `==3.1.0`


</details>

<details>
<summary>Update dependency attrs to v23.2.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/attrs-23.x`
  - Merge into: `main`
  - Upgrade [attrs](https://togithub.com/python-attrs/attrs) to `==23.2.0`


</details>

<details>
<summary>Update dependency build to v1.1.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/build-1.x`
  - Merge into: `main`
  - Upgrade [build](https://togithub.com/pypa/build) to `==1.1.1`


</details>

<details>
<summary>Update dependency cachecontrol to v0.14.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/cachecontrol-0.x`
  - Merge into: `main`
  - Upgrade [cachecontrol](https://togithub.com/psf/cachecontrol) to `==0.14.0`


</details>

<details>
<summary>Update dependency cattrs to v23.2.3</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/cattrs-23.x`
  - Merge into: `main`
  - Upgrade [cattrs](https://togithub.com/python-attrs/cattrs) to `==23.2.3`


</details>

<details>
<summary>Update dependency dep-logic to v0.2.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/dep-logic-0.x`
  - Merge into: `main`
  - Upgrade dep-logic to `==0.2.0`


</details>

<details>
<summary>Update dependency findpython to v0.5.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/findpython-0.x`
  - Merge into: `main`
  - Upgrade [findpython](https://togithub.com/frostming/findpython) to `==0.5.1`


</details>

<details>
<summary>Update dependency hatchling to v1.22.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/hatchling-1.x`
  - Merge into: `main`
  - Upgrade [hatchling](https://togithub.com/pypa/hatch) to `==1.22.4`


</details>

<details>
<summary>Update dependency httpx to v0.27.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/httpx-0.x`
  - Merge into: `main`
  - Upgrade [httpx](https://togithub.com/encode/httpx) to `==0.27.0`


</details>

<details>
<summary>Update dependency importlib-metadata to v7.1.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/importlib-metadata-7.x`
  - Merge into: `main`
  - Upgrade [importlib-metadata](https://togithub.com/python/importlib_metadata) to `==7.1.0`


</details>

<details>
<summary>Update dependency isort to v5.13.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/isort-5.x`
  - Merge into: `main`
  - Upgrade [isort](https://togithub.com/pycqa/isort) to `==5.13.2`


</details>

<details>
<summary>Update dependency more-itertools to v10.2.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/more-itertools-10.x`
  - Merge into: `main`
  - Upgrade [more-itertools](https://togithub.com/more-itertools/more-itertools) to `==10.2.0`


</details>

<details>
<summary>Update dependency mypy to v1.9.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/mypy-1.x`
  - Merge into: `main`
  - Upgrade [mypy](https://togithub.com/python/mypy) to `==1.9.0`


</details>

<details>
<summary>Update dependency pathspec to v0.12.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pathspec-0.x`
  - Merge into: `main`
  - Upgrade [pathspec](https://togithub.com/cpburnz/python-pathspec) to `==0.12.1`


</details>

<details>
<summary>Update dependency pip-tools to v7.4.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pip-tools-7.x`
  - Merge into: `main`
  - Upgrade [pip-tools](https://togithub.com/jazzband/pip-tools) to `==7.4.1`


</details>

<details>
<summary>Update dependency pkginfo to v1.10.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pkginfo-1.x`
  - Merge into: `main`
  - Upgrade pkginfo to `==1.10.0`


</details>

<details>
<summary>Update dependency pluggy to v1.4.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pluggy-1.x`
  - Merge into: `main`
  - Upgrade [pluggy](https://togithub.com/pytest-dev/pluggy) to `==1.4.0`


</details>

<details>
<summary>Update dependency poetry to v1.8.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/poetry-1.x`
  - Merge into: `main`
  - Upgrade [poetry](https://togithub.com/python-poetry/poetry) to `==1.8.2`


</details>

<details>
<summary>Update dependency poetry-core to v1.9.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/poetry-core-1.x`
  - Merge into: `main`
  - Upgrade [poetry-core](https://togithub.com/python-poetry/poetry-core) to `==1.9.0`


</details>

<details>
<summary>Update dependency poetry-plugin-export to v1.7.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/poetry-plugin-export-1.x`
  - Merge into: `main`
  - Upgrade [poetry-plugin-export](https://togithub.com/python-poetry/poetry-plugin-export) to `==1.7.1`


</details>

<details>
<summary>Update dependency pydantic-core to v2.17.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pydantic-core-2.x`
  - Merge into: `main`
  - Upgrade [pydantic-core](https://togithub.com/pydantic/pydantic-core) to `==2.17.0`


</details>

<details>
<summary>Update dependency pyflakes to v3.2.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pyflakes-3.x`
  - Merge into: `main`
  - Upgrade [pyflakes](https://togithub.com/PyCQA/pyflakes) to `==3.2.0`


</details>

<details>
<summary>Update dependency pygls to v1.3.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pygls-1.x`
  - Merge into: `main`
  - Upgrade [pygls](https://togithub.com/openlawlibrary/pygls) to `==1.3.0`


</details>

<details>
<summary>Update dependency pygments to v2.17.2</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pygments-2.x`
  - Merge into: `main`
  - Upgrade [pygments](https://togithub.com/pygments/pygments) to `==2.17.2`


</details>

<details>
<summary>Update dependency pylint to v3.1.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pylint-3.x`
  - Merge into: `main`
  - Upgrade [pylint](https://togithub.com/pylint-dev/pylint) to `==3.1.0`


</details>

<details>
<summary>Update dependency rapidfuzz to v3.7.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rapidfuzz-3.x`
  - Merge into: `main`
  - Upgrade [rapidfuzz](https://togithub.com/rapidfuzz/RapidFuzz) to `==3.7.0`


</details>

<details>
<summary>Update dependency rich to v13.7.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/rich-13.x`
  - Merge into: `main`
  - Upgrade [rich](https://togithub.com/Textualize/rich) to `==13.7.1`


</details>

<details>
<summary>Update dependency ruff to v0.3.4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/ruff-0.x`
  - Merge into: `main`
  - Upgrade [ruff](https://togithub.com/astral-sh/ruff) to `==0.3.4`


</details>

<details>
<summary>Update dependency scikit-learn to v1.4.1.post1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/scikit-learn-1.x`
  - Merge into: `main`
  - Upgrade [scikit-learn](https://togithub.com/scikit-learn/scikit-learn) to `==1.4.1.post1`


</details>

<details>
<summary>Update dependency scipy to v1.12.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/scipy-1.x`
  - Merge into: `main`
  - Upgrade [scipy](https://togithub.com/scipy/scipy) to `==1.12.0`


</details>

<details>
<summary>Update dependency setuptools to v69.2.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/setuptools-69.x`
  - Merge into: `main`
  - Upgrade [setuptools](https://togithub.com/pypa/setuptools) to `==69.2.0`


</details>

<details>
<summary>Update dependency threadpoolctl to v3.4.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/threadpoolctl-3.x`
  - Merge into: `main`
  - Upgrade [threadpoolctl](https://togithub.com/joblib/threadpoolctl) to `==3.4.0`


</details>

<details>
<summary>Update dependency typer to v0.10.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/typer-0.x`
  - Merge into: `main`
  - Upgrade [typer](https://togithub.com/tiangolo/typer) to `==0.10.0`


</details>

<details>
<summary>Update dependency typing-extensions to v4.10.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/typing-extensions-4.x`
  - Merge into: `main`
  - Upgrade [typing-extensions](https://togithub.com/python/typing_extensions) to `==4.10.0`


</details>

<details>
<summary>Update dependency unearth to v0.15.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/unearth-0.x`
  - Merge into: `main`
  - Upgrade [unearth](https://togithub.com/frostming/unearth) to `==0.15.0`


</details>

<details>
<summary>Update dependency urllib3 to v2.2.1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/urllib3-2.x`
  - Merge into: `main`
  - Upgrade [urllib3](https://togithub.com/urllib3/urllib3) to `==2.2.1`


</details>

<details>
<summary>Update dependency wheel to v0.43.0</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/wheel-0.x`
  - Merge into: `main`
  - Upgrade [wheel](https://togithub.com/pypa/wheel) to `==0.43.0`


</details>

<details>
<summary>Update Rust crate http to v1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/http-1.x`
  - Merge into: `main`
  - Upgrade [http](https://togithub.com/hyperium/http) to `1.1.0`


</details>

<details>
<summary>Update Rust crate hyper to v1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/hyper-1.x`
  - Merge into: `main`
  - Upgrade [hyper](https://togithub.com/hyperium/hyper) to `1.2.0`


</details>

<details>
<summary>Update debian Docker tag to v12</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/debian-12.x`
  - Merge into: `main`
  - Upgrade debian to `bookworm`


</details>

<details>
<summary>Update dependency certifi to v2024</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/certifi-2024.x`
  - Merge into: `main`
  - Upgrade [certifi](https://togithub.com/certifi/python-certifi) to `==2024.2.2`


</details>

<details>
<summary>Update dependency flake8 to v7</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/flake8-7.x`
  - Merge into: `main`
  - Upgrade [flake8](https://togithub.com/pycqa/flake8) to `==7.0.0`


</details>

<details>
<summary>Update dependency keyring to v25</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/keyring-25.x`
  - Merge into: `main`
  - Upgrade [keyring](https://togithub.com/jaraco/keyring) to `==25.0.0`


</details>

<details>
<summary>Update dependency packaging to v24</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/packaging-24.x`
  - Merge into: `main`
  - Upgrade [packaging](https://togithub.com/pypa/packaging) to `==24.0`


</details>

<details>
<summary>Update dependency pip to v24</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pip-24.x`
  - Merge into: `main`
  - Upgrade [pip](https://togithub.com/pypa/pip) to `==24.0`


</details>

<details>
<summary>Update dependency platformdirs to v4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/platformdirs-4.x`
  - Merge into: `main`
  - Upgrade [platformdirs](https://togithub.com/platformdirs/platformdirs) to `==4.2.0`


</details>

<details>
<summary>Update dependency pytest to v8</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/pytest-8.x`
  - Merge into: `main`
  - Upgrade [pytest](https://togithub.com/pytest-dev/pytest) to `==8.1.1`


</details>

<details>
<summary>Update dependency trove-classifiers to v2024.3.25</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/trove-classifiers-2024.x`
  - Merge into: `main`
  - Upgrade [trove-classifiers](https://togithub.com/pypa/trove-classifiers) to `==2024.3.25`


</details>

<details>
<summary>Update dependency twine to v5</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/twine-5.x`
  - Merge into: `main`
  - Upgrade [twine](https://togithub.com/pypa/twine) to `==5.0.0`


</details>

<details>
<summary>Update dependency typeguard to v4</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/typeguard-4.x`
  - Merge into: `main`
  - Upgrade [typeguard](https://togithub.com/agronholm/typeguard) to `==4.2.1`


</details>

<details>
<summary>Update dependency ubuntu to v22</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/ubuntu-22.x`
  - Merge into: `main`
  - Upgrade [ubuntu](https://togithub.com/actions/runner-images) to `22.04`


</details>

<details>
<summary>Update dependency xattr to v1</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/xattr-1.x`
  - Merge into: `main`
  - Upgrade [xattr](https://togithub.com/xattr/xattr) to `==1.1.0`


</details>

<details>
<summary>Update fedora Docker tag to v41</summary>

  - Schedule: ["at any time"]
  - Branch name: `renovate/fedora-41.x`
  - Merge into: `main`
  - Upgrade fedora to `41`


</details>

<br />

🚸 Branch creation will be limited to maximum 2 per hour, so it doesn't swamp any CI resources or overwhelm the project. See docs for `prhourlylimit` for details.


---

❓ Got questions? Check out Renovate's [Docs](https://docs.renovatebot.com/), particularly the Getting Started section.
If you need any further assistance then you can also [request help here](https://togithub.com/renovatebot/renovate/discussions).


---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).

<!--renovate-config-hash:e80b4e42a3043bc12fa0640db4bac392d2bf770acf841360d7c8ceeeac2ec1a9-->


---

_Comment by @zanieb on 2024-03-25 19:13_

#2653 is the relevant pull request

---

_Closed by @zanieb on 2024-03-25 19:13_

---

_Comment by @renovate[bot] on 2024-03-25 19:13_

### Renovate is disabled

Renovate is disabled because there is no Renovate configuration file. To enable Renovate, you can either (a) change this PR's title to get a new onboarding PR, and merge the new onboarding PR, or (b) create a Renovate config file, and commit that file to your base branch.

---

_Branch deleted on 2024-03-25 19:27_

---
