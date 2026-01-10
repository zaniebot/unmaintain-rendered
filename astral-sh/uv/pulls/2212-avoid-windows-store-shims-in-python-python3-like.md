```yaml
number: 2212
title: "Avoid Windows Store shims in `--python python3`-like invocations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-03-05T18:39:58Z
updated_at: 2024-03-05T18:47:39Z
url: https://github.com/astral-sh/uv/pull/2212
synced_at: 2026-01-10T14:54:43Z
```

# Avoid Windows Store shims in `--python python3`-like invocations

---

_Pull request opened by @charliermarsh on 2024-03-05 18:39_

## Summary

We have logic in `python_query.rs` to filter out Windows Store shims when you use invocations like `-p 3.10`, but not `--python python3`, which is uncommon but allowed on Windows.

Closes #2211.


---

_Label `bug` added by @charliermarsh on 2024-03-05 18:40_

---

_Label `windows` added by @charliermarsh on 2024-03-05 18:40_

---

_Marked ready for review by @charliermarsh on 2024-03-05 18:40_

---

_Merged by @charliermarsh on 2024-03-05 18:47_

---

_Closed by @charliermarsh on 2024-03-05 18:47_

---

_Branch deleted on 2024-03-05 18:47_

---
