```yaml
number: 10480
title: "Consider raw source code for `W605`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/fix-w605
created_at: 2024-03-19T18:23:07Z
updated_at: 2024-03-19T18:46:36Z
url: https://github.com/astral-sh/ruff/pull/10480
synced_at: 2026-01-10T22:47:02Z
```

# Consider raw source code for `W605`

---

_Pull request opened by @dhruvmanila on 2024-03-19 18:23_

## Summary

This PR fixes a panic in the linter for `W605`.

Consider the following f-string:
```python
f"{{}}ab"
```

The `FStringMiddle` token would contain `{}ab`. Notice that the escaped braces have _reduced_ the string. This means we cannot use the text value from the token to determine the location of the escape sequence but need to extract it from the source code.

fixes: #10434 

## Test Plan

Add new test cases and update the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-03-19 18:23_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-03-19 18:23_

---

_@dhruvmanila reviewed on 2024-03-19 18:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:75 on 2024-03-19 18:24_

Just to be explicit that this is the entire f-string range.

---

_@charliermarsh approved on 2024-03-19 18:25_

---

_Comment by @github-actions[bot] on 2024-03-19 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-03-19 18:46_

---

_Closed by @dhruvmanila on 2024-03-19 18:46_

---

_Branch deleted on 2024-03-19 18:46_

---
