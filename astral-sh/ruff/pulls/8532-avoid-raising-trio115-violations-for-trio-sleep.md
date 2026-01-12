```yaml
number: 8532
title: "Avoid raising `TRIO115` violations for `trio.sleep(...)` calls with non-number values"
type: pull_request
state: merged
author: qdegraaf
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/fixTRIO115
created_at: 2023-11-06T22:29:51Z
updated_at: 2023-11-06T22:49:13Z
url: https://github.com/astral-sh/ruff/pull/8532
synced_at: 2026-01-12T15:55:26Z
```

# Avoid raising `TRIO115` violations for `trio.sleep(...)` calls with non-number values

---

_@qdegraaf_

## Summary

Fixes bug in `TRIO115` where it would not `return` for values that were not a `NumberLiteral` so 
```python
x = "bla"
trio.sleep(x)
```
would set off a false positive

## Test Plan

Added test case to fixture


---

_Renamed from "[`TRIO115`] Return if assigned value of arg is not an Expr::NumberLiteral in TRIO115" to "Avoid raising `TRIO115` violations for `trio.sleep(...)` calls with non-number values" by @zanieb on 2023-11-06 22:46_

---

_Label `bug` added by @zanieb on 2023-11-06 22:46_

---

_@zanieb approved on 2023-11-06 22:46_

Thanks!

---

_Comment by @github-actions[bot] on 2023-11-06 22:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @zanieb on 2023-11-06 22:49_

---

_Closed by @zanieb on 2023-11-06 22:49_

---
