```yaml
number: 20213
title: "[`ruff`] Recognize t-strings, generators, and lambdas in `invalid-index-type` (`RUF016`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: invalid-index-type-for-bool-generator-lambda
created_at: 2025-09-03T14:09:39Z
updated_at: 2025-09-12T18:37:02Z
url: https://github.com/astral-sh/ruff/pull/20213
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Recognize t-strings, generators, and lambdas in `invalid-index-type` (`RUF016`)

---

_Pull request opened by @TaKO8Ki on 2025-09-03 14:09_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20204

Recognize t-strings, generators, and lambdas in RUF016

- Accept boolean literals as valid index and slice bounds.
- Add TString, Generator, and Lambda to `CheckableExprType`.
- Expand RUF016.py fixture and update snapshots accordingly.

## Test Plan

<!-- How was it tested? -->

I've added test cases for t-string, generator and lambda to RUF016.py.

---

_Comment by @github-actions[bot] on 2025-09-03 14:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-09 12:55_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/invalid_index_type.rs`:186 on 2025-09-12 13:48_

t-strings do not have type `str` but rather type `Template` (or `string.templatelib.Template`)

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/ruff/RUF016.py`:1 on 2025-09-12 13:50_

Can you move the new fixtures to the end and put a comment with a link to the issue? It makes the diff in the snapshot a little easier to review since it doesn't change all the line numbers

---

_@dylwil3 requested changes on 2025-09-12 13:50_

Thanks! I think this looks right, but it's hard to review the snapshot until the fixtures get moved to the end, so I'll take a second look once you've done that 😄 

---

_@TaKO8Ki reviewed on 2025-09-12 18:19_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/ruff/rules/invalid_index_type.rs`:186 on 2025-09-12 18:19_

I changed it to `Template`.

---

_@TaKO8Ki reviewed on 2025-09-12 18:19_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/resources/test/fixtures/ruff/RUF016.py`:1 on 2025-09-12 18:19_

I've moved them to the end of the file.

---

_Review requested from @dylwil3 by @TaKO8Ki on 2025-09-12 18:20_

---

_@dylwil3 approved on 2025-09-12 18:35_

Excellent, thank you!

---

_Renamed from "[`ruff`] Recognize t-strings, generators, and lambdas in RUF016" to "[`ruff`] Recognize t-strings, generators, and lambdas in `invalid-index-type` (`RUF016`)" by @dylwil3 on 2025-09-12 18:36_

---

_Label `bug` added by @dylwil3 on 2025-09-12 18:36_

---

_Label `rule` added by @dylwil3 on 2025-09-12 18:36_

---

_Merged by @dylwil3 on 2025-09-12 18:37_

---

_Closed by @dylwil3 on 2025-09-12 18:37_

---
