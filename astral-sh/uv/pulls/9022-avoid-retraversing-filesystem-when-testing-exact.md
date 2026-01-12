```yaml
number: 9022
title: Avoid retraversing filesystem when testing exact glob matches
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-11-11T17:40:55Z
updated_at: 2024-11-11T17:54:37Z
url: https://github.com/astral-sh/uv/pull/9022
synced_at: 2026-01-12T16:08:36Z
```

# Avoid retraversing filesystem when testing exact glob matches

---

_@charliermarsh_

## Summary

When testing for exact inclusion, we can just test the glob directly. There's no need to re-traverse the filesystem to find it.


---

_Label `performance` added by @charliermarsh on 2024-11-11 17:42_

---

_Merged by @charliermarsh on 2024-11-11 17:54_

---

_Closed by @charliermarsh on 2024-11-11 17:54_

---

_Branch deleted on 2024-11-11 17:54_

---
