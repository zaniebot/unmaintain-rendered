```yaml
number: 20486
title: "[`ruff`] Fix `B004` to skip invalid `hasattr`/`getattr` calls"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: b004-invalid-args-check
created_at: 2025-09-19T17:42:21Z
updated_at: 2025-09-19T18:45:37Z
url: https://github.com/astral-sh/ruff/pull/20486
synced_at: 2026-01-12T15:57:03Z
```

# [`ruff`] Fix `B004` to skip invalid `hasattr`/`getattr` calls

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

Fixes #20440

Fix B004 to skip invalid hasattr/getattr calls

- Add argument validation for `hasattr` and `getattr`
- Skip B004 rule when function calls have invalid argument patterns

## Test Plan

<!-- How was it tested? -->

Add comprehensive test cases for invalid argument patterns including unpacking to `B004.py`


---

_Comment by @github-actions[bot] on 2025-09-19 17:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-19 18:05_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/unreliable_callable_check.rs`:122 on 2025-09-19 18:19_

We may as well move the `keywords` check all the way to the beginning of the function. (You could even leave the original function unmodified and do the `keywords` check at the call site - but it's a little more self-documenting to keep the logic inside the function, I guess).

---

_@dylwil3 requested changes on 2025-09-19 18:19_

One nit and otherwise looks good, thank you!

---

_@TaKO8Ki reviewed on 2025-09-19 18:37_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flake8_bugbear/rules/unreliable_callable_check.rs`:122 on 2025-09-19 18:37_

I've moved the keywords count check to the beginning of `unreliable_callable_check`.

---

_@dylwil3 approved on 2025-09-19 18:44_

---

_Merged by @dylwil3 on 2025-09-19 18:44_

---

_Closed by @dylwil3 on 2025-09-19 18:44_

---

_Renamed from "[`ruff`] Fix B004 to skip invalid hasattr/getattr calls" to "[`ruff`] Fix `B004` to skip invalid `hasattr`/`getattr` calls" by @dylwil3 on 2025-09-19 18:45_

---

_Label `bug` added by @dylwil3 on 2025-09-19 18:45_

---

_Label `rule` added by @dylwil3 on 2025-09-19 18:45_

---
