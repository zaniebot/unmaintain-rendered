```yaml
number: 6955
title: "Avoid panic when `typename` is provided as a keyword argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/UP014
created_at: 2023-08-28T20:51:24Z
updated_at: 2023-08-28T21:11:11Z
url: https://github.com/astral-sh/ruff/pull/6955
synced_at: 2026-01-12T15:55:22Z
```

# Avoid panic when `typename` is provided as a keyword argument

---

_@charliermarsh_

## Summary

The `typename` argument to `NamedTuple` and `TypedDict` is a required positional argument. We assumed as much, but panicked if it was provided as a keyword argument or otherwise omitted. This PR handles the case gracefully.

Closes https://github.com/astral-sh/ruff/issues/6953.

---

_Renamed from "Avoid panic when typename is provided as a keyword argument" to "Avoid panic when `typename` is provided as a keyword argument" by @charliermarsh on 2023-08-28 20:51_

---

_Label `bug` added by @charliermarsh on 2023-08-28 20:51_

---

_@zanieb approved on 2023-08-28 20:54_

---

_Merged by @charliermarsh on 2023-08-28 21:03_

---

_Closed by @charliermarsh on 2023-08-28 21:03_

---

_Branch deleted on 2023-08-28 21:03_

---

_Comment by @github-actions[bot] on 2023-08-28 21:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
