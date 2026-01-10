```yaml
number: 10078
title: Preserve sort when deciding on requirement placement
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/so
created_at: 2024-12-21T13:55:23Z
updated_at: 2024-12-21T14:43:45Z
url: https://github.com/astral-sh/uv/pull/10078
synced_at: 2026-01-10T11:44:33Z
```

# Preserve sort when deciding on requirement placement

---

_Pull request opened by @charliermarsh on 2024-12-21 13:55_

## Summary

We had the right logic for determining whether the list is already sorted, but we forgot to apply the same logic when deciding where to insert the requirement, which made the list _unsorted_ for future operations.

Closes https://github.com/astral-sh/uv/issues/10076.


---

_Label `bug` added by @charliermarsh on 2024-12-21 13:55_

---

_Merged by @charliermarsh on 2024-12-21 14:43_

---

_Closed by @charliermarsh on 2024-12-21 14:43_

---

_Branch deleted on 2024-12-21 14:43_

---
