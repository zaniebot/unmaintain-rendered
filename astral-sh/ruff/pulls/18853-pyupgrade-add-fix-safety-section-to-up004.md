```yaml
number: 18853
title: "[`pyupgrade`] Add fix safety section to `UP004`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-06-21T19:56:22Z
updated_at: 2025-06-23T16:58:16Z
url: https://github.com/astral-sh/ruff/pull/18853
synced_at: 2026-01-10T18:39:09Z
```

# [`pyupgrade`] Add fix safety section to `UP004`

---

_Pull request opened by @MeGaGiGaGon on 2025-06-21 19:56_

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

This adds a `Fix safety` section to [useless-object-inheritance (UP004)](https://docs.astral.sh/ruff/rules/useless-object-inheritance/#useless-object-inheritance-up004)

I could not track down the original PR as this rule is so old it has gone through several large ruff refactors.
No reasoning is given on the unsafety in the PR/code.
The unsafety is determined here:
https://github.com/astral-sh/ruff/blob/f24e650dfd9d20c3d01c2615da3f4778da3cf036/crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs#L76-L80

Unsafe fix demonstration:
[playground](https://play.ruff.rs/12b24eb4-d7a5-4ae0-93bb-492d64967ae3)
```py
class A(  # will be deleted
    object
):
    ...
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-21 20:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-22 07:51_

---

_Label `documentation` added by @dylwil3 on 2025-06-23 13:22_

---

_@dylwil3 approved on 2025-06-23 13:22_

Perfect, thank you!

---

_Merged by @dylwil3 on 2025-06-23 13:22_

---

_Closed by @dylwil3 on 2025-06-23 13:22_

---

_Branch deleted on 2025-06-23 16:58_

---
