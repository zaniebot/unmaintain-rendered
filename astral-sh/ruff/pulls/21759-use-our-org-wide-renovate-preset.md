```yaml
number: 21759
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
created_at: 2025-12-02T17:34:06Z
updated_at: 2025-12-02T18:05:28Z
url: https://github.com/astral-sh/ruff/pull/21759
synced_at: 2026-01-12T15:57:32Z
```

# Use our org-wide Renovate preset

---

_@woodruffw_

## Summary

Supersedes #21757.

## Test Plan

Same as there 🙂 

---

_Review requested from @ntBre by @woodruffw on 2025-12-02 17:34_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-02 17:34_

---

_Label `internal` added by @woodruffw on 2025-12-02 17:34_

---

_Comment by @renovate[bot] on 2025-12-02 17:35_

### Action Required: Fix Renovate Configuration

There is an error with this repository's Renovate configuration that needs to be fixed.

Location: `.github/renovate.json5`
Message: `Invalid configuration option: npm.fileMatch, Invalid configuration option: pep621.fileMatch`


---

_Comment by @woodruffw on 2025-12-02 17:41_

[`d07701a` (#21759)](https://github.com/astral-sh/ruff/pull/21759/commits/d07701a9367a45a23a3bae4dca0954d88da3635e) should fix that config error. I'm tempted to switch away from JSON5 too (since VS Code doesn't schematize it or even highlight it by default), which is why I didn't notice the change.

---

_Comment by @renovate[bot] on 2025-12-02 17:41_

### Reconfigure PR Results

This is a reconfigure PR comment to help you understand and re-configure your renovate bot settings. If this Reconfigure PR were to be merged, we'd expect to see the following outcome:


---
### Detected Package Files

 * `Cargo.toml` (cargo)
 * `crates/ruff/Cargo.toml` (cargo)
 * `crates/ruff_annotate_snippets/Cargo.toml` (cargo)
 * `crates/ruff_benchmark/Cargo.toml` (cargo)
 * `crates/ruff_cache/Cargo.toml` (cargo)
 * `crates/ruff_db/Cargo.toml` (cargo)
 * `crates/ruff_dev/Cargo.toml` (cargo)
 * `crates/ruff_diagnostics/Cargo.toml` (cargo)
 * `crates/ruff_formatter/Cargo.toml` (cargo)
 * `crates/ruff_graph/Cargo.toml` (cargo)
 * `crates/ruff_index/Cargo.toml` (cargo)
 * `crates/ruff_linter/Cargo.toml` (cargo)
 * `crates/ruff_macros/Cargo.toml` (cargo)
 * `crates/ruff_memory_usage/Cargo.toml` (cargo)
 * `crates/ruff_notebook/Cargo.toml` (cargo)
 * `crates/ruff_options_metadata/Cargo.toml` (cargo)
 * `crates/ruff_python_ast/Cargo.toml` (cargo)
 * `crates/ruff_python_ast_integration_tests/Cargo.toml` (cargo)
 * `crates/ruff_python_codegen/Cargo.toml` (cargo)
 * `crates/ruff_python_formatter/Cargo.toml` (cargo)
 * `crates/ruff_python_importer/Cargo.toml` (cargo)
 * `crates/ruff_python_index/Cargo.toml` (cargo)
 * `crates/ruff_python_literal/Cargo.toml` (cargo)
 * `crates/ruff_python_parser/Cargo.toml` (cargo)
 * `crates/ruff_python_semantic/Cargo.toml` (cargo)
 * `crates/ruff_python_stdlib/Cargo.toml` (cargo)
 * `crates/ruff_python_trivia/Cargo.toml` (cargo)
 * `crates/ruff_python_trivia_integration_tests/Cargo.toml` (cargo)
 * `crates/ruff_server/Cargo.toml` (cargo)
 * `crates/ruff_source_file/Cargo.toml` (cargo)
 * `crates/ruff_text_size/Cargo.toml` (cargo)
 * `crates/ruff_wasm/Cargo.toml` (cargo)
 * `crates/ruff_workspace/Cargo.toml` (cargo)
 * `crates/ty/Cargo.toml` (cargo)
 * `crates/ty_combine/Cargo.toml` (cargo)
 * `crates/ty_completion_eval/Cargo.toml` (cargo)
 * `crates/ty_ide/Cargo.toml` (cargo)
 * `crates/ty_project/Cargo.toml` (cargo)
 * `crates/ty_python_semantic/Cargo.toml` (cargo)
 * `crates/ty_server/Cargo.toml` (cargo)
 * `crates/ty_static/Cargo.toml` (cargo)
 * `crates/ty_test/Cargo.toml` (cargo)
 * `crates/ty_vendored/Cargo.toml` (cargo)
 * `crates/ty_wasm/Cargo.toml` (cargo)
 * `fuzz/Cargo.toml` (cargo)
 * `.github/workflows/build-binaries.yml` (github-actions)
 * `.github/workflows/build-docker.yml` (github-actions)
 * `.github/workflows/ci.yaml` (github-actions)
 * `.github/workflows/daily_fuzz.yaml` (github-actions)
 * `.github/workflows/mypy_primer.yaml` (github-actions)
 * `.github/workflows/notify-dependents.yml` (github-actions)
 * `.github/workflows/publish-docs.yml` (github-actions)
 * `.github/workflows/publish-playground.yml` (github-actions)
 * `.github/workflows/publish-pypi.yml` (github-actions)
 * `.github/workflows/publish-ty-playground.yml` (github-actions)
 * `.github/workflows/publish-wasm.yml` (github-actions)
 * `.github/workflows/release.yml` (github-actions)
 * `.github/workflows/sync_typeshed.yaml` (github-actions)
 * `.github/workflows/ty-ecosystem-analyzer.yaml` (github-actions)
 * `.github/workflows/ty-ecosystem-report.yaml` (github-actions)
 * `.github/workflows/typing_conformance.yaml` (github-actions)
 * `playground/api/package.json` (npm)
 * `playground/package.json` (npm)
 * `playground/ruff/package.json` (npm)
 * `playground/shared/package.json` (npm)
 * `playground/ty/package.json` (npm)
 * `scripts/ty_benchmark/package.json` (npm)
 * `crates/ty_completion_eval/truth/auto-import-skips-current-module/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/fstring-completions/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/higher-level-symbols-preferred/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/import-deprioritizes-dunder/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/import-deprioritizes-sunder/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/import-deprioritizes-type_check_only/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/import-keyword-completion/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/internal-typeshed-hidden/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/none-completion/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/numpy-array/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/object-attr-instance-methods/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/pass-keyword-completion/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/raise-uses-base-exception/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/scope-existing-over-new-import/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/scope-prioritize-closer/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/scope-simple-long-identifier/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/tstring-completions/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/ty-extensions-lower-stdlib/pyproject.toml` (pep621)
 * `crates/ty_completion_eval/truth/type-var-typing-over-ast/pyproject.toml` (pep621)
 * `pyproject.toml` (pep621)
 * `python/py-fuzzer/pyproject.toml` (pep621)
 * `python/ruff-ecosystem/pyproject.toml` (pep621)
 * `scripts/benchmarks/pyproject.toml` (pep621)
 * `scripts/pyproject.toml` (pep621)
 * `scripts/ty_benchmark/pyproject.toml` (pep621)
 * `docs/requirements-insiders.txt` (pip_requirements)
 * `docs/requirements.txt` (pip_requirements)
 * `.pre-commit-config.yaml` (pre-commit)

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
  - Run Renovate on following schedule: before 4am on Monday


---

### What to Expect

With your current configuration, Renovate will create 27 Pull Requests:

<details>
<summary>Update actions/checkout digest to 8e8c483</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/actions-checkout-digest`
  - Merge into: `main`
  - Upgrade actions/checkout to `8e8c483db84b4bee98b60c0593521ed34d9990e8`


</details>

<details>
<summary>Update actions/download-artifact digest to f093f21</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/actions-download-artifact-digest`
  - Merge into: `main`
  - Upgrade actions/download-artifact to `f093f21ca4cfa7c75ccbbc2be54da76a0c7e1f05`


</details>

<details>
<summary>Update lsp-types digest to ddc7dc8</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/lsp-types-digest`
  - Merge into: `main`
  - Upgrade lsp-types to `ddc7dc8b374dedd947abf14bdb4a37f257a53e92`


</details>

<details>
<summary>Update Rust crate libc to v0.2.178</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/libc-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [libc](https://redirect.github.com/rust-lang/libc) to `0.2.178`


</details>

<details>
<summary>Update Rust crate unicode-normalization to v0.1.25</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/unicode-normalization-0.x-lockfile`
  - Merge into: `main`
  - Upgrade [unicode-normalization](https://redirect.github.com/unicode-rs/unicode-normalization) to `0.1.25`


</details>

<details>
<summary>Update rust-wasm-bindgen monorepo</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/rust-wasm-bindgen-monorepo`
  - Merge into: `main`
  - Upgrade [js-sys](https://redirect.github.com/wasm-bindgen/wasm-bindgen) to `0.3.83`
  - Upgrade [wasm-bindgen](https://redirect.github.com/wasm-bindgen/wasm-bindgen) to `0.2.106`
  - Upgrade [wasm-bindgen-test](https://redirect.github.com/wasm-bindgen/wasm-bindgen) to `0.3.56`


</details>

<details>
<summary>Update taiki-e/install-action action to v2.62.61</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/taiki-e-install-action-2.x`
  - Merge into: `main`
  - Upgrade [taiki-e/install-action](https://redirect.github.com/taiki-e/install-action) to `92e6dd1c202153a204d471a3c509bf1e03dcfa44`


</details>

<details>
<summary>Update dependency monaco-editor to ^0.55.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/monaco-editor-0.x`
  - Merge into: `main`
  - Upgrade [monaco-editor](https://redirect.github.com/microsoft/monaco-editor) to `^0.55.0`


</details>

<details>
<summary>Update dependency python to 3.14</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/python-3.x`
  - Merge into: `main`
  - Upgrade [python](https://redirect.github.com/actions/python-versions) to `3.14`


</details>

<details>
<summary>Update docker/metadata-action action to v5.9.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/docker-metadata-action-5.x`
  - Merge into: `main`
  - Upgrade [docker/metadata-action](https://redirect.github.com/docker/metadata-action) to `318604b99e75e41977312d83839a89be02ca4893`


</details>

<details>
<summary>Update Rust crate codspeed-criterion-compat to v4.1.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/codspeed-criterion-compat-4.x-lockfile`
  - Merge into: `main`
  - Upgrade [codspeed-criterion-compat](https://redirect.github.com/CodSpeedHQ/codspeed-rust) to `4.1.0`


</details>

<details>
<summary>Update Rust crate criterion to 0.8.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/criterion-0.x`
  - Merge into: `main`
  - Upgrade [criterion](https://redirect.github.com/criterion-rs/criterion.rs) to `0.8.0`


</details>

<details>
<summary>Update Rust crate divan to v4.1.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/divan-4.x-lockfile`
  - Merge into: `main`
  - Upgrade [divan](https://redirect.github.com/CodSpeedHQ/codspeed-rust) to `4.1.0`


</details>

<details>
<summary>Update Rust crate imara-diff to 0.2.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/imara-diff-0.x`
  - Merge into: `main`
  - Upgrade [imara-diff](https://redirect.github.com/pascalkuthe/imara-diff) to `0.2.0`


</details>

<details>
<summary>Update Rust crate insta to v1.44.1</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/insta-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [insta](https://redirect.github.com/mitsuhiko/insta) to `1.44.1`


</details>

<details>
<summary>Update Rust crate schemars to v1.1.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/schemars-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [schemars](https://redirect.github.com/GREsau/schemars) to `1.1.0`


</details>

<details>
<summary>Update Rust crate serde_with to v3.16.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/serde_with-3.x-lockfile`
  - Merge into: `main`
  - Upgrade [serde_with](https://redirect.github.com/jonasbb/serde_with) to `3.16.0`


</details>

<details>
<summary>Update Rust crate uuid to v1.19.0</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/uuid-1.x-lockfile`
  - Merge into: `main`
  - Upgrade [uuid](https://redirect.github.com/uuid-rs/uuid) to `1.19.0`


</details>

<details>
<summary>Update actions/checkout action to v6</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/actions-checkout-6.x`
  - Merge into: `main`
  - Upgrade [actions/checkout](https://redirect.github.com/actions/checkout) to `1af3b93b6815bc44a9784bd300feb67ff0d1eeb3`


</details>

<details>
<summary>Update dependency mdformat to v1</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/mdformat-1.x`
  - Merge into: `main`
  - Upgrade [mdformat](https://redirect.github.com/hukkin/mdformat) to `==1.0.0`


</details>

<details>
<summary>Update dependency mdformat-mkdocs to v5</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/mdformat-mkdocs-5.x`
  - Merge into: `main`
  - Upgrade [mdformat-mkdocs](https://redirect.github.com/kyleking/mdformat-mkdocs) to `==5.0.0`


</details>

<details>
<summary>Update dependency node to v24</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/node-24.x`
  - Merge into: `main`
  - Upgrade [node](https://redirect.github.com/actions/node-versions) to `24`


</details>

<details>
<summary>Update GitHub Artifact Actions</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/github-artifact-actions`
  - Merge into: `main`
  - Upgrade [actions/download-artifact](https://redirect.github.com/actions/download-artifact) to `018cc2cf5baa6db3ef3c5f8a56943fffe632ef53`
  - Upgrade [actions/upload-artifact](https://redirect.github.com/actions/upload-artifact) to `330a01c490aca151604b8cf639adc76d48f6c5d4`


</details>

<details>
<summary>Update NPM Development dependencies</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/npm-development-dependencies`
  - Merge into: `main`
  - Upgrade [@cloudflare/workers-types](https://redirect.github.com/cloudflare/workerd) to `4.20251125.0`
  - Upgrade [@eslint/js](https://redirect.github.com/eslint/eslint) to `9.39.1`
  - Upgrade [@tailwindcss/vite](https://redirect.github.com/tailwindlabs/tailwindcss) to `4.1.17`
  - Upgrade [@types/react](https://redirect.github.com/DefinitelyTyped/DefinitelyTyped) to `19.2.7`
  - Upgrade [@types/react-dom](https://redirect.github.com/DefinitelyTyped/DefinitelyTyped) to `19.2.3`
  - Upgrade [@vitejs/plugin-react-swc](https://redirect.github.com/vitejs/vite-plugin-react) to `^4.0.0`
  - Upgrade [eslint](https://redirect.github.com/eslint/eslint) to `9.39.1`
  - Upgrade [eslint-plugin-react-hooks](https://redirect.github.com/facebook/react) to `^7.0.0`
  - Upgrade [miniflare](https://redirect.github.com/cloudflare/workers-sdk) to `4.20251118.1`
  - Upgrade [tailwindcss](https://redirect.github.com/tailwindlabs/tailwindcss) to `4.1.17`
  - Upgrade [typescript](https://redirect.github.com/microsoft/TypeScript) to `5.9.3`
  - Upgrade [typescript-eslint](https://redirect.github.com/typescript-eslint/typescript-eslint) to `8.48.0`
  - Upgrade [vite](https://redirect.github.com/vitejs/vite) to `7.2.4`
  - Upgrade [wrangler](https://redirect.github.com/cloudflare/workers-sdk) to `4.50.0`


</details>

<details>
<summary>Update pre-commit dependencies</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/pre-commit-dependencies`
  - Merge into: `main`
  - Upgrade [adamchainz/blacken-docs](https://redirect.github.com/adamchainz/blacken-docs) to `1.20.0`
  - Upgrade [astral-sh/ruff-pre-commit](https://redirect.github.com/astral-sh/ruff-pre-commit) to `v0.14.6`
  - Upgrade [black](https://redirect.github.com/psf/black) to `==25.11.0`
  - Upgrade [crate-ci/typos](https://redirect.github.com/crate-ci/typos) to `v1.39.2`
  - Upgrade [executablebooks/mdformat](https://redirect.github.com/executablebooks/mdformat) to `1.0.0`
  - Upgrade [igorshubovych/markdownlint-cli](https://redirect.github.com/igorshubovych/markdownlint-cli) to `v0.46.0`
  - Upgrade [mdformat-footnote](https://redirect.github.com/gaige/mdformat-footnote) to `==0.1.2`
  - Upgrade [mdformat-mkdocs](https://redirect.github.com/kyleking/mdformat-mkdocs) to `==5.0.0`
  - Upgrade [pre-commit/pre-commit-hooks](https://redirect.github.com/pre-commit/pre-commit-hooks) to `v6.0.0`
  - Upgrade [python-jsonschema/check-jsonschema](https://redirect.github.com/python-jsonschema/check-jsonschema) to `0.35.0`
  - Upgrade [rhysd/actionlint](https://redirect.github.com/rhysd/actionlint) to `v1.7.9`
  - Upgrade [zizmorcore/zizmor-pre-commit](https://redirect.github.com/zizmorcore/zizmor-pre-commit) to `v1.16.3`


</details>

<details>
<summary>Update Rust crate ordermap to v1</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/ordermap-1.x`
  - Merge into: `main`
  - Upgrade [ordermap](https://redirect.github.com/indexmap-rs/ordermap) to `1.0.0`


</details>

<details>
<summary>Update Rust crate unicode_names2 to v2</summary>

  - Schedule: ["before 4am on Monday"]
  - Branch name: `renovate/unicode_names2-2.x`
  - Merge into: `main`
  - Upgrade [unicode_names2](https://redirect.github.com/progval/unicode_names2) to `2.0.0`


</details>


---
> 
> [!WARNING]
> Please correct - or verify that you can safely ignore - these dependency lookup failures before you merge this PR.
> 
> -   `Failed to look up git-tags package git@github.com:astral-sh/mkdocs-material-insiders.git`
> 
> Files affected: `docs/requirements-insiders.txt`




---

_@ntBre approved on 2025-12-02 17:45_

Thank you!

Interesting about the schema change. I skimmed the renovate comment, looks plausible to me!

---

_Comment by @woodruffw on 2025-12-02 17:47_

Thanks! I'm kind of concerned that the change summary there shows updates that are definitely below the cooldown. I'll try and get to the bottom of that.

(Sorry for the noise here...)

---

_Comment by @woodruffw on 2025-12-02 18:00_

Okay, I think https://github.com/astral-sh/renovate-config/pull/2 will ensure that the cooldown is fully honored. 

---

_Merged by @woodruffw on 2025-12-02 18:05_

---

_Closed by @woodruffw on 2025-12-02 18:05_

---

_Branch deleted on 2025-12-02 18:05_

---
