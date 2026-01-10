```yaml
number: 8234
title: Avoid showing lower-bound warning outside of explicit lock and sync
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/bounds
created_at: 2024-10-16T00:32:12Z
updated_at: 2024-10-16T00:49:42Z
url: https://github.com/astral-sh/uv/pull/8234
synced_at: 2026-01-10T12:54:05Z
```

# Avoid showing lower-bound warning outside of explicit lock and sync

---

_Pull request opened by @charliermarsh on 2024-10-16 00:32_

## Summary

We shouldn't show these in `uv add`, especially when the thing we're adding is about to have a lower-bound put on it. Now, we only show these when the user runs `uv lock` or `uv sync`.


---

_Label `error messages` added by @charliermarsh on 2024-10-16 00:32_

---

_Merged by @charliermarsh on 2024-10-16 00:49_

---

_Closed by @charliermarsh on 2024-10-16 00:49_

---

_Branch deleted on 2024-10-16 00:49_

---
