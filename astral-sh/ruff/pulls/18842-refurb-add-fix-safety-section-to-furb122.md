```yaml
number: 18842
title: "[`refurb`] Add fix safety section to `FURB122`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-4
created_at: 2025-06-20T23:58:12Z
updated_at: 2025-06-23T16:58:21Z
url: https://github.com/astral-sh/ruff/pull/18842
synced_at: 2026-01-10T18:39:09Z
```

# [`refurb`] Add fix safety section to `FURB122`

---

_Pull request opened by @MeGaGiGaGon on 2025-06-20 23:58_

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

This adds a `Fix safety` section to [for-loop-writes (FURB122)](https://docs.astral.sh/ruff/rules/for-loop-writes/#for-loop-writes-furb122).

The fix/lint was introduced in #10630
No reasoning is given on the unsafety in the PR/code.
The unsafety is determined here:
https://github.com/astral-sh/ruff/blob/ea812d0813faf6e0ed2d39354e85d08f9b857c90/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs#L200-L204
Unsafe fix demonstration:
[playground](https://play.ruff.rs/06592f33-10b9-4a77-b31e-0d3a98f402f4)
```py
with open("issue.txt", "w") as f:
    for i in range(10):
        # will be deleted
        f.write(str(i))
```

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-21 00:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-21 15:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:41 on 2025-06-23 13:17_

```suggestion
/// This fix is marked as unsafe if it would cause comments to be deleted.
```


---

_@dylwil3 approved on 2025-06-23 13:17_

Thank you!

---

_Merged by @dylwil3 on 2025-06-23 13:21_

---

_Closed by @dylwil3 on 2025-06-23 13:21_

---

_Label `documentation` added by @dylwil3 on 2025-06-23 13:23_

---

_Branch deleted on 2025-06-23 16:58_

---
