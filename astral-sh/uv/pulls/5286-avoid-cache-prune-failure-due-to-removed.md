```yaml
number: 5286
title: Avoid cache prune failure due to removed interpreter
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache2
created_at: 2024-07-22T13:30:15Z
updated_at: 2024-07-22T13:46:26Z
url: https://github.com/astral-sh/uv/pull/5286
synced_at: 2026-01-10T13:42:52Z
```

# Avoid cache prune failure due to removed interpreter

---

_Pull request opened by @charliermarsh on 2024-07-22 13:30_

## Summary

`uv cache prune` can fail if an ephemeral environment includes a non-existent interpreter, since we then fail to read the symlink.


---

_Label `bug` added by @charliermarsh on 2024-07-22 13:30_

---

_Merged by @charliermarsh on 2024-07-22 13:46_

---

_Closed by @charliermarsh on 2024-07-22 13:46_

---

_Branch deleted on 2024-07-22 13:46_

---
