```yaml
number: 9441
title: "RUF013 false-positive: Hashable includes None"
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2024-01-09T03:03:06Z
updated_at: 2024-01-09T03:38:35Z
url: https://github.com/astral-sh/ruff/issues/9441
synced_at: 2026-01-10T11:09:51Z
```

# RUF013 false-positive: Hashable includes None

---

_Issue opened by @twoertwein on 2024-01-09 03:03_

`def test(x: Hashable = None)` triggers RUF013 but None is Hashable.

---

_Label `bug` added by @charliermarsh on 2024-01-09 03:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-09 03:09_

---

_Closed by @charliermarsh on 2024-01-09 03:38_

---
