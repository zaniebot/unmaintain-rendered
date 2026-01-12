```yaml
number: 5918
title: "Prefetch metadata in `--no-deps` mode"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/prefetch
created_at: 2024-08-08T16:15:23Z
updated_at: 2024-08-08T16:35:17Z
url: https://github.com/astral-sh/uv/pull/5918
synced_at: 2026-01-12T16:07:06Z
```

# Prefetch metadata in `--no-deps` mode

---

_@charliermarsh_

## Summary

This _used_ to be true but we now require fetching metadata for all distributions even with `--no-deps` since, e.g., we validate that any declared extras exist.


---

_Label `performance` added by @charliermarsh on 2024-08-08 16:15_

---

_Marked ready for review by @charliermarsh on 2024-08-08 16:15_

---

_@zanieb approved on 2024-08-08 16:23_

---

_Merged by @charliermarsh on 2024-08-08 16:35_

---

_Closed by @charliermarsh on 2024-08-08 16:35_

---

_Branch deleted on 2024-08-08 16:35_

---
