```yaml
number: 7029
title: "Fixed panic in `missing_copyright_notice`"
type: pull_request
state: merged
author: WindowGenerator
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_missing_copyright_notice_panic
created_at: 2023-08-31T16:36:15Z
updated_at: 2023-09-02T07:18:55Z
url: https://github.com/astral-sh/ruff/pull/7029
synced_at: 2026-01-12T02:45:38Z
```

# Fixed panic in `missing_copyright_notice`

---

_Pull request opened by @WindowGenerator on 2023-08-31 16:36_

## Summary

Fix "not a character boundary" panic in `missing_copyright_notice`

## Test Plan

`cargo test` + added fixture file for CPY001.py


---

_Renamed from "Fixed panic in 'missing_copyright_notice'" to "WIP Fixed panic in 'missing_copyright_notice'" by @WindowGenerator on 2023-08-31 16:49_

---

_Comment by @zanieb on 2023-08-31 16:50_

Hey! You'll need to add the test case in https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/flake8_copyright/mod.rs

Here's an example adding a test for a file https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/flake8_bugbear/mod.rs#L17

---

_Converted to draft by @zanieb on 2023-08-31 18:26_

---

_Comment by @github-actions[bot] on 2023-09-01 10:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Renamed from "WIP Fixed panic in 'missing_copyright_notice'" to "Fixed panic in 'missing_copyright_notice'" by @WindowGenerator on 2023-09-01 13:22_

---

_Label `bug` added by @charliermarsh on 2023-09-01 13:35_

---

_@charliermarsh reviewed on 2023-09-01 13:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_copyright/mod.rs`:162 on 2023-09-01 13:35_

I moved this to `test_snippet` to match the pattern used in the other tests in this module. (We typically use dedicated test files like you had here, but it's nice to use the same convention within a given test suite.)

---

_@charliermarsh reviewed on 2023-09-01 13:36_

---

_Review comment by @charliermarsh on `crates/ruff_source_file/src/locator.rs`:442 on 2023-09-01 13:36_

I moved this into a dedicated method on `Locator`, based on the `str#floor_char_boundary` API which is unstable (so we can't use it) but has this rough pattern.

---

_Renamed from "Fixed panic in 'missing_copyright_notice'" to "Fixed panic in `missing_copyright_notice`" by @charliermarsh on 2023-09-01 13:36_

---

_@charliermarsh approved on 2023-09-01 13:36_

Awesome, thank you!

---

_Marked ready for review by @charliermarsh on 2023-09-01 13:36_

---

_Merged by @charliermarsh on 2023-09-01 13:58_

---

_Closed by @charliermarsh on 2023-09-01 13:58_

---

_@WindowGenerator reviewed on 2023-09-01 15:18_

---

_Review comment by @WindowGenerator on `crates/ruff_source_file/src/locator.rs`:442 on 2023-09-01 15:18_

Thank you for advice!

---

_Branch deleted on 2023-09-02 07:18_

---
