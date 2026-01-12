```yaml
number: 20391
title: "[`ruff`] `FURB164` Replace -nan with nan when using the value to construct `Decimal`"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-negative-nan-issue-for-furb164
created_at: 2025-09-15T00:47:38Z
updated_at: 2025-09-19T18:46:42Z
url: https://github.com/astral-sh/ruff/pull/20391
synced_at: 2026-01-12T15:57:00Z
```

# [`ruff`] `FURB164` Replace -nan with nan when using the value to construct `Decimal`

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19699

Normalize `Decimal(float("-nan"))` to `Decimal("nan")`.

The same handling is implemented in:
https://github.com/astral-sh/ruff/blob/c3e873dd827b9ec8dd49cff723089852ce2301e1/crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs#L165

## Test Plan

<!-- How was it tested? -->

I've updated `ruff_linter__rules__refurb__tests__FURB164_FURB164.py.snap`.


---

_@TaKO8Ki reviewed on 2025-09-15 00:51_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:299 on 2025-09-15 00:51_

The same handling is implemented in: https://github.com/astral-sh/ruff/blob/c3e873dd827b9ec8dd49cff723089852ce2301e1/crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs#L165

Is it better to embed this handling into `as_non_finite_float_string_literal`? Since Apart from unnecessary_from_float, the function is used only in the following place, it does not affect other parts, but I feel it's too specific.
https://github.com/astral-sh/ruff/blob/c3e873dd827b9ec8dd49cff723089852ce2301e1/crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs#L159

---

_Comment by @github-actions[bot] on 2025-09-15 01:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-15 13:27_

---

_@dylwil3 approved on 2025-09-19 18:03_

Thank you! I think the small amount of duplication is okay for now.

---

_Merged by @dylwil3 on 2025-09-19 18:04_

---

_Closed by @dylwil3 on 2025-09-19 18:04_

---

_Branch deleted on 2025-09-19 18:13_

---

_Label `bug` added by @dylwil3 on 2025-09-19 18:46_

---

_Label `fixes` added by @dylwil3 on 2025-09-19 18:46_

---
