```yaml
number: 1104
title: Use a consolidated error for distribution failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/src
created_at: 2024-01-25T19:46:37Z
updated_at: 2024-01-25T19:49:12Z
url: https://github.com/astral-sh/uv/pull/1104
synced_at: 2026-01-12T16:04:26Z
```

# Use a consolidated error for distribution failures

---

_@charliermarsh_

## Summary

Use a single error type in `puffin_distribution`, rather than two confusingly similar types between `DistributionDatabase` and the source distribution module.

Also removes the `#[from]` for IO errors and replaces with explicit wrapping, which is verbose but removes a bunch of incorrect error messages.

---

_Renamed from "Use a consolidated error" to "Use a consolidated error for distribution failures" by @charliermarsh on 2024-01-25 19:46_

---

_Label `bug` added by @charliermarsh on 2024-01-25 19:46_

---

_Merged by @charliermarsh on 2024-01-25 19:49_

---

_Closed by @charliermarsh on 2024-01-25 19:49_

---

_Branch deleted on 2024-01-25 19:49_

---
