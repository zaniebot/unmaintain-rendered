```yaml
number: 8308
title: "[`refurb`] Implement `isinstance-type-none` (`FURB168`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: isinstance-type-none
created_at: 2023-10-28T16:03:21Z
updated_at: 2023-10-30T09:04:15Z
url: https://github.com/astral-sh/ruff/pull/8308
synced_at: 2026-01-10T23:40:55Z
```

# [`refurb`] Implement `isinstance-type-none` (`FURB168`)

---

_Pull request opened by @tjkuson on 2023-10-28 16:03_

## Summary

Implement [`no-isinstance-type-none`](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/no_isinstance_type_none.py) as `isinstance-type-none` (`FURB168`).

Auto-fixes calls to `isinstance` to check if an object is `None` to a `None` identity check. For example,

```python
isinstance(foo, type(None))
```

becomes

```python
foo is None
```

Related to #1348.

## Test Plan

`cargo test`


---

_Marked ready for review by @tjkuson on 2023-10-28 16:17_

---

_Comment by @github-actions[bot] on 2023-10-28 16:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-28 22:01_

---

_Label `rule` added by @charliermarsh on 2023-10-28 22:01_

---

_@charliermarsh approved on 2023-10-28 22:34_

Looks great -- thank you!

---

_Merged by @charliermarsh on 2023-10-28 22:37_

---

_Closed by @charliermarsh on 2023-10-28 22:37_

---

_Branch deleted on 2023-10-30 09:04_

---
