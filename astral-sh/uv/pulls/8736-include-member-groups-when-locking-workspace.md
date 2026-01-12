```yaml
number: 8736
title: Include member groups when locking workspace
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/g
created_at: 2024-10-31T23:51:28Z
updated_at: 2024-11-01T00:04:35Z
url: https://github.com/astral-sh/uv/pull/8736
synced_at: 2026-01-12T16:08:28Z
```

# Include member groups when locking workspace

---

_@charliermarsh_

## Summary

It turns out that when locking, we were only taking the groups from the root `pyproject.toml` into account, and ignoring groups that were only defined in a workspace member.


---

_Label `bug` added by @charliermarsh on 2024-10-31 23:51_

---

_Merged by @charliermarsh on 2024-11-01 00:04_

---

_Closed by @charliermarsh on 2024-11-01 00:04_

---

_Branch deleted on 2024-11-01 00:04_

---
