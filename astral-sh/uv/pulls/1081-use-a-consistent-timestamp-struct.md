```yaml
number: 1081
title: "Use a consistent `Timestamp` struct"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tz
created_at: 2024-01-24T18:25:04Z
updated_at: 2024-01-24T19:21:33Z
url: https://github.com/astral-sh/uv/pull/1081
synced_at: 2026-01-12T16:04:24Z
```

# Use a consistent `Timestamp` struct

---

_@charliermarsh_

## Summary

This PR uses `ctime` consistently on Unix as a more conservative approach to change detection. It also ensures that our timestamp abstraction is entirely internal, so we can change the representation and logic easily across the codebase in the future.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-24 18:25_

---

_Label `internal` added by @charliermarsh on 2024-01-24 18:25_

---

_@BurntSushi approved on 2024-01-24 19:13_

> It also ensures that our timestamp abstraction is entirely internal

Me like.

---

_Merged by @charliermarsh on 2024-01-24 19:21_

---

_Closed by @charliermarsh on 2024-01-24 19:21_

---

_Branch deleted on 2024-01-24 19:21_

---
