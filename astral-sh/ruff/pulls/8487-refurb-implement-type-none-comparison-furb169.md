```yaml
number: 8487
title: "[`refurb`] Implement `type-none-comparison` (`FURB169`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: type-none-comparison
created_at: 2023-11-04T14:50:02Z
updated_at: 2023-11-06T09:21:55Z
url: https://github.com/astral-sh/ruff/pull/8487
synced_at: 2026-01-10T23:40:55Z
```

# [`refurb`] Implement `type-none-comparison` (`FURB169`)

---

_Pull request opened by @tjkuson on 2023-11-04 14:50_

## Summary

Implement [`no-is-type-none`](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/no_is_type_none.py) as `type-none-comparison` (`FURB169`).

Auto-fixes comparisons that use `type` to compare the type of an object to `type(None)` to a `None` identity check. For example,

```python
type(foo) is type(None)
```

becomes

```python
foo is None
```

Related to #1348.

## Test Plan

`cargo test`


---

_Renamed from "Implement rule" to "[`refurb`] Implement `type-none-comparison` (`FURB169`)" by @tjkuson on 2023-11-04 14:51_

---

_Comment by @github-actions[bot] on 2023-11-04 15:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @tjkuson on 2023-11-04 15:15_

---

_@charliermarsh approved on 2023-11-05 19:55_

Looks great, thanks!

---

_Label `rule` added by @charliermarsh on 2023-11-05 19:55_

---

_Label `preview` added by @charliermarsh on 2023-11-05 19:55_

---

_Merged by @charliermarsh on 2023-11-06 00:56_

---

_Closed by @charliermarsh on 2023-11-06 00:56_

---

_Branch deleted on 2023-11-06 09:21_

---
