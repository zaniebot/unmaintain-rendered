```yaml
number: 9595
title: Normalize paths when lowering Git dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/back-path
created_at: 2024-12-03T04:59:14Z
updated_at: 2024-12-03T05:41:27Z
url: https://github.com/astral-sh/uv/pull/9595
synced_at: 2026-01-12T16:08:53Z
```

# Normalize paths when lowering Git dependencies

---

_@charliermarsh_

## Summary

Discovered while working on https://github.com/astral-sh/uv/issues/9516. In the linked repo, the root uses a `../dependency` path for the workspace member, which we weren't normalizing.


---

_Label `bug` added by @charliermarsh on 2024-12-03 04:59_

---

_Merged by @charliermarsh on 2024-12-03 05:41_

---

_Closed by @charliermarsh on 2024-12-03 05:41_

---

_Branch deleted on 2024-12-03 05:41_

---
