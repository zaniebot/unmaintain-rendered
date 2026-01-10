```yaml
number: 1218
title: Remove double-download for source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/double
created_at: 2024-02-01T04:36:25Z
updated_at: 2024-02-01T04:41:30Z
url: https://github.com/astral-sh/uv/pull/1218
synced_at: 2026-01-10T15:39:03Z
```

# Remove double-download for source distributions

---

_Pull request opened by @charliermarsh on 2024-02-01 04:36_

## Summary

Oops -- this was using a different cache key than the route above (this is the wheel _metadata_ route vs. the wheel build route), so we were saving and building source distributions twice in `pip install`.

---

_Label `bug` added by @charliermarsh on 2024-02-01 04:36_

---

_Merged by @charliermarsh on 2024-02-01 04:41_

---

_Closed by @charliermarsh on 2024-02-01 04:41_

---

_Branch deleted on 2024-02-01 04:41_

---
