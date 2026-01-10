```yaml
number: 12387
title: "Split the `publish_wasm` job into a build and publish step"
type: issue
state: open
author: MichaReiser
labels:
  - help wanted
  - ci
assignees: []
created_at: 2024-07-18T17:10:48Z
updated_at: 2026-01-08T13:03:09Z
url: https://github.com/astral-sh/ruff/issues/12387
synced_at: 2026-01-10T11:09:54Z
```

# Split the `publish_wasm` job into a build and publish step

---

_Issue opened by @MichaReiser on 2024-07-18 17:10_

https://github.com/astral-sh/ruff/pull/12317 added a publishing pipeline for our WASM crate to NPM. We should split that pipeline into two workflows:

1. Builds the binaries
2. Publish step to NPM



---

_Label `help wanted` added by @MichaReiser on 2024-07-18 17:10_

---

_Label `ci` added by @MichaReiser on 2024-07-18 17:10_

---

_Comment by @bybrooks on 2026-01-08 11:54_

Hi!
I'd like to work on this issue. 
Before I start implementing, I want to confirm my understanding is correct.

### My Understanding

Currently, `publish-wasm.yml` performs both building and publishing to NPM within a single job. 
I'm proposing to split this into:

1. **`build-wasm.yml` (new)**: Build WASM binaries and upload as artifacts
2. **`publish-wasm.yml` (modified)**: Download artifacts and publish to NPM

### Additional Features Under Consideration

Following the pattern of `build-binaries.yml`, I'm also considering adding these features:

| Feature | Description |
|---------|-------------|
| **PR trigger** | Automatically run build tests when `crates/ruff_wasm/**` or `.github/workflows/build-wasm.yml` is changed |
| **concurrency** | Cancel older runs when duplicate runs occur on the same branch |
| **Least privilege principle** | Set `permissions: {}` at workflow level, granting only necessary permissions at job level |
| **`no-build` label support** | Skip build when PR has the `no-build` label |

### Design Questions

1. Should I implement these additional features, or should I keep the changes minimal (just the split)?

2. Are there any requirements or considerations I might have missed?

---

_Comment by @MichaReiser on 2026-01-08 13:03_

Awesome!

> Currently, publish-wasm.yml performs both building and publishing to NPM within a single job.
I'm proposing to split this into:

This sounds about right to me

> Automatically run build tests when crates/ruff_wasm/** or .github/workflows/build-wasm.yml is changed

Changes to `build-wasm` do make sense to me. I'm not sure we want to run it on every `ruff_wasm` change. Maybe limit it to changes to the `package.json` `package-lock`.json`

---
