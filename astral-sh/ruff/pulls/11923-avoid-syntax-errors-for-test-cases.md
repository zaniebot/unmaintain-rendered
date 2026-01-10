```yaml
number: 11923
title: Avoid syntax errors for test cases
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/syntax-error-0
created_at: 2024-06-18T10:56:02Z
updated_at: 2024-06-18T11:55:38Z
url: https://github.com/astral-sh/ruff/pull/11923
synced_at: 2026-01-10T21:56:00Z
```

# Avoid syntax errors for test cases

---

_Pull request opened by @dhruvmanila on 2024-06-18 10:56_

## Summary

This PR removes most of the syntax errors from the test cases. This would create noise when https://github.com/astral-sh/ruff/pull/11901 is complete. These syntax errors are also just noise for the test itself.

## Test Plan

Update the snapshots and verify that they're still the same.


---

_Label `internal` added by @dhruvmanila on 2024-06-18 10:56_

---

_@dhruvmanila reviewed on 2024-06-18 10:57_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:416 on 2024-06-18 10:57_

`async` is a keyword. I guess this test case assumed that `async` is a soft keyword.

---

_Comment by @dhruvmanila on 2024-06-18 10:58_

I'm going to merge this as it's not a logic change but feel free to look at the change (mainly the Python files).

---

_Comment by @github-actions[bot] on 2024-06-18 11:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:422 on 2024-06-18 11:42_

Do you think it would make sense to replace this test with another soft keyword like `type` or `match`?

---

_@MichaReiser approved on 2024-06-18 11:42_

---

_@dhruvmanila reviewed on 2024-06-18 11:43_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_commas/COM81.py`:453 on 2024-06-18 11:43_

Currently, our parser raises syntax error for duplicate parameter.

---

_@dhruvmanila reviewed on 2024-06-18 11:43_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pandas_vet/mod.rs`:299 on 2024-06-18 11:43_

I suspect this was a copy paste error.

---

_@dhruvmanila reviewed on 2024-06-18 11:46_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pycodestyle/E30.py`:422 on 2024-06-18 11:46_

I don't think that's going to matter as the `E30` rules are testing blank lines.

---

_Merged by @dhruvmanila on 2024-06-18 11:46_

---

_Closed by @dhruvmanila on 2024-06-18 11:46_

---

_Branch deleted on 2024-06-18 11:46_

---
