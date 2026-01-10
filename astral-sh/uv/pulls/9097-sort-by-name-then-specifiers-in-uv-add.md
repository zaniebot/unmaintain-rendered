```yaml
number: 9097
title: "Sort by name, then specifiers in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-11-13T20:12:41Z
updated_at: 2024-11-13T20:47:59Z
url: https://github.com/astral-sh/uv/pull/9097
synced_at: 2026-01-10T12:00:00Z
```

# Sort by name, then specifiers in `uv add`

---

_Pull request opened by @charliermarsh on 2024-11-13 20:12_

## Summary

This PR ensures that `pylint>=3.2.6` followed by `pylint-module-boundaries>=1.3.1` is considered sorted, despite the fact that `>` is later in the alphabetic than `-`. By purely comparing strings, they would _not_ be sorted; but by considering the name, then the specifiers, they are.

Closes https://github.com/astral-sh/uv/issues/9076.


---

_Label `bug` added by @charliermarsh on 2024-11-13 20:12_

---

_Merged by @charliermarsh on 2024-11-13 20:47_

---

_Closed by @charliermarsh on 2024-11-13 20:47_

---

_Branch deleted on 2024-11-13 20:47_

---
