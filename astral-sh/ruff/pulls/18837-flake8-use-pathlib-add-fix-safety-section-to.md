```yaml
number: 18837
title: "[`flake8-use-pathlib`] Add fix safety section to `PTH201`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-path_constructor_current_directory
created_at: 2025-06-20T22:09:26Z
updated_at: 2025-06-23T16:58:01Z
url: https://github.com/astral-sh/ruff/pull/18837
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-use-pathlib`] Add fix safety section to `PTH201`

---

_Pull request opened by @MeGaGiGaGon on 2025-06-20 22:09_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #15584

This adds a `Fix safety` section to [path-constructor-current-directory (PTH201)](https://docs.astral.sh/ruff/rules/path-constructor-current-directory/#path-constructor-current-directory-pth201)

I could not track down the original PR as this rule is so old it has gone through several large ruff refactors.
The unsafety is determined here:
https://github.com/astral-sh/ruff/blob/d9266284df170cd70908761b5d5ae45b25523a40/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs#L55-L59
Unsafe code example:
[playground](https://play.ruff.rs/76da532a-c7ad-4ef9-bba3-4626296e5317)
```py
from pathlib import Path
Path(#
    "."#
)
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-20 22:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-21 16:18_

---

_@dylwil3 approved on 2025-06-23 13:21_

Perfect, thank you!

---

_Merged by @dylwil3 on 2025-06-23 13:22_

---

_Closed by @dylwil3 on 2025-06-23 13:22_

---

_Label `documentation` added by @dylwil3 on 2025-06-23 13:23_

---

_Branch deleted on 2025-06-23 16:58_

---
