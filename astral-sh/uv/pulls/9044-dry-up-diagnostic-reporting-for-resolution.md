```yaml
number: 9044
title: DRY up diagnostic reporting for resolution failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/op
created_at: 2024-11-12T04:11:15Z
updated_at: 2024-11-12T14:46:04Z
url: https://github.com/astral-sh/uv/pull/9044
synced_at: 2026-01-10T12:00:00Z
```

# DRY up diagnostic reporting for resolution failures

---

_Pull request opened by @charliermarsh on 2024-11-12 04:11_

## Summary

Not thrilled with this but helps for now. I feel like this error-handling should happen at the top-level, rather than on all these individual commands. But we don't have a unified result type at the top-level of the CLI -- all these commands return `anyhow::Result`.


---

_Label `internal` added by @charliermarsh on 2024-11-12 04:12_

---

_Merged by @charliermarsh on 2024-11-12 14:46_

---

_Closed by @charliermarsh on 2024-11-12 14:46_

---

_Branch deleted on 2024-11-12 14:46_

---
