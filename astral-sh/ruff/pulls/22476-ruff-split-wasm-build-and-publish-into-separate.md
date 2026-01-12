```yaml
number: 22476
title: "[ruff] Split WASM build and publish into separate workflows (#12387)"
type: pull_request
state: merged
author: bybrooks
labels:
  - ci
assignees: []
merged: true
base: main
head: bybrooks/gh-12387-split-wasm-workflow
created_at: 2026-01-09T12:27:19Z
updated_at: 2026-01-09T17:21:32Z
url: https://github.com/astral-sh/ruff/pull/22476
synced_at: 2026-01-12T15:57:50Z
```

# [ruff] Split WASM build and publish into separate workflows (#12387)

---

_@bybrooks_

## Summary

Issue：https://github.com/astral-sh/ruff/issues/12387

This PR splits the WASM workflow into separate build and publish workflows, following the same pattern used for binaries (`build-binaries.yml` and `release.yml`).

### Changes

**New file: `.github/workflows/build-wasm.yml`**
- Triggers on `workflow_call` and `pull_request`
- Builds WASM packages for three targets: `web`, `bundler`, `nodejs` (matrix strategy)
- Uploads build artifacts via `actions/upload-artifact`

**Modified: `.github/workflows/publish-wasm.yml`**
- Removed build steps (wasm-pack, rust toolchain setup, etc.)
- Added `actions/download-artifact` to fetch pre-built artifacts
- Kept existing npm publish logic (dry-run and production)

**Modified: `.github/workflows/release.yml`**
- Added `custom-build-wasm` job that runs after `plan`
- Updated `build-global-artifacts` to depend on `custom-build-wasm`

## Test Plan

### Automated Tests (triggered by this PR)
- [x] `Build wasm` workflow is triggered via `pull_request`
- [x] 3 matrix jobs (web, bundler, nodejs) run in parallel
- [x] Artifacts (`artifacts-wasm-*`) are uploaded for each target

### Manual Verification (after merge)
1. Run `Release` workflow via `workflow_dispatch` with `dry-run` tag
2. Verify:
   - [x] `custom-build-wasm` job runs after `plan`
   - [x] `build-global-artifacts` waits for `custom-build-wasm` completion
   - [ ] `custom-publish-wasm` downloads artifacts and runs `npm publish --dry-run`
   - [ ] Job dependency chain works correctly

### Local Validation (completed)
- [x] `pre-commit run check-github-workflows -a` - Passed
- [x] `pre-commit run actionlint -a --hook-stage=manual` - Passed
- [x] `pre-commit run prettier -a` - Passed


---

_Marked ready for review by @bybrooks on 2026-01-09 12:27_

---

_Label `ci` added by @MichaReiser on 2026-01-09 12:44_

---

_@MichaReiser reviewed on 2026-01-09 12:46_

---

_Review comment by @MichaReiser on `.github/workflows/release.yml`:122 on 2026-01-09 12:46_

This file is auto generated. I believe you have to add the job here 

https://github.com/astral-sh/ruff/blob/69999e49cf3026f85ab99445a14d8f30d088b126/dist-workspace.toml#L53

and then run `cargo dist init`

---

_Comment by @MichaReiser on 2026-01-09 16:23_

This is great. It will make our release workflows much faster. Thank you

Hmm, I don't think that I can trigger the release workflow from your branch. Do you know if you can trigger it on your fork?

---

_Comment by @MichaReiser on 2026-01-09 16:40_

Let's give this a try

---

_Merged by @MichaReiser on 2026-01-09 16:41_

---

_Closed by @MichaReiser on 2026-01-09 16:41_

---

_Comment by @MichaReiser on 2026-01-09 16:57_

Running https://github.com/astral-sh/ruff/actions/runs/20858802478

---

_Comment by @MichaReiser on 2026-01-09 17:20_

Looks like we'll only find out when doing our next release

---
