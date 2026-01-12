```yaml
number: 7179
title: "build: add libcst from crates"
type: pull_request
state: merged
author: manmartgarc
labels:
  - internal
assignees: []
merged: true
base: main
head: add-libcst-from-crates
created_at: 2023-09-06T05:43:18Z
updated_at: 2023-09-06T18:07:12Z
url: https://github.com/astral-sh/ruff/pull/7179
synced_at: 2026-01-12T02:45:38Z
```

# build: add libcst from crates

---

_Pull request opened by @manmartgarc on 2023-09-06 05:43_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is related to https://github.com/astral-sh/ruff/issues/43 and https://github.com/Instagram/LibCST/issues/967. The good people over at LibCST are in the process of making their packages publishable to `crates.io` and have already uploaded the version for which `ruff` is dependent on, namely `v1.0.1`. This PR removes the git dependency and replaces it for its [crates.io](https://crates.io/crates/libcst) distribution.

## Test Plan

Ran `cargo build` locally.


---

_@dhruvmanila reviewed on 2023-09-06 05:52_

---

_Review comment by @dhruvmanila on `Cargo.toml`:57 on 2023-09-06 05:52_

The comment is outdated as I forgot to update it when I updated LibCST to the latest commit in https://github.com/astral-sh/ruff/pull/7062 (Sorry!). So, we'll have to wait until there's another release for us to update here.

---

_@dhruvmanila reviewed on 2023-09-06 06:01_

---

_Review comment by @dhruvmanila on `Cargo.toml`:57 on 2023-09-06 06:01_

Or am I mistaken? Does `0.1.0` indicate the latest version of main as of now i.e., does it include this commit (instagram/libcst@9c263aa8977962a870ce2770d2aa18ee0dacb344)? I think it does as the tests seems to be passing.

---

_Review comment by @MichaReiser on `Cargo.toml`:57 on 2023-09-06 06:06_

We should to keep the `default-features=false` because we don't need the pyo3 bindings

```suggestion
libcst = { version = "0.1.0", default-features = false }
```

This should also fix the build failures

---

_Review comment by @MichaReiser on `Cargo.toml`:57 on 2023-09-06 06:08_

It seems to be `main`. Or at least any commit not older than https://github.com/Instagram/LibCST/commit/377a292d0d43a71b1e9704799d7e46b621a97018 where they added the metadata. But I agree, the versioning is a bit confusing

---

_@MichaReiser requested changes on 2023-09-06 06:10_

Yeahy. One less git dependency! 

This looks good to me but we'll need to remove the default-features to get the build passing 



---

_@manmartgarc reviewed on 2023-09-06 07:55_

---

_Review comment by @manmartgarc on `Cargo.toml`:57 on 2023-09-06 07:55_

Added a new commit with this setting

---

_@MichaReiser approved on 2023-09-06 08:10_

---

_Label `internal` added by @MichaReiser on 2023-09-06 08:10_

---

_Merged by @MichaReiser on 2023-09-06 08:24_

---

_Closed by @MichaReiser on 2023-09-06 08:24_

---

_Comment by @github-actions[bot] on 2023-09-06 08:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Branch deleted on 2023-09-06 18:07_

---
