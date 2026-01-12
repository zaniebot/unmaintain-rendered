```yaml
number: 10651
title: "Allow f-strings with `%z` for `DTZ007`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/f-strings-for-DTZ007
created_at: 2024-03-29T03:20:57Z
updated_at: 2024-03-29T04:27:14Z
url: https://github.com/astral-sh/ruff/pull/10651
synced_at: 2026-01-12T15:55:32Z
```

# Allow f-strings with `%z` for `DTZ007`

---

_@dhruvmanila_

## Summary

This PR fixes the bug for `DTZ007` rule where it didn't consider to check for the presence of `%z` in f-strings. It also considers the string parts of an implicitly concatenated f-strings for which I want to find a better solution (#10308).

fixes: #10601 

## Test Plan

Add test cases and update the snapshots.

---

_Label `bug` added by @dhruvmanila on 2024-03-29 03:20_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-03-29 03:22_

---

_@zanieb reviewed on 2024-03-29 03:34_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_strptime_without_zone.rs`:114 on 2024-03-29 03:34_

Should we check in a test case for this anyway?

---

_@zanieb approved on 2024-03-29 03:34_

---

_Comment by @github-actions[bot] on 2024-03-29 03:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-03-29 04:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_strptime_without_zone.rs`:114 on 2024-03-29 04:03_

Ruff will raise a violation in this case even though it shouldn't. Maybe checking string literal isn't too bad 🤔 

---

_@zanieb reviewed on 2024-03-29 04:07_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_strptime_without_zone.rs`:114 on 2024-03-29 04:07_

I think it's fine to have an "incorrect" test case as long as it's clearly documented, then we have a test demonstrating the fix when you get to it.

---

_@dhruvmanila reviewed on 2024-03-29 04:08_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_strptime_without_zone.rs`:114 on 2024-03-29 04:08_

I see. I've added a commit which also checks for the implicitly concatenated case.

---

_Merged by @dhruvmanila on 2024-03-29 04:27_

---

_Closed by @dhruvmanila on 2024-03-29 04:27_

---

_Branch deleted on 2024-03-29 04:27_

---
