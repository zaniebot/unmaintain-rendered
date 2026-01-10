```yaml
number: 11766
title: Use a Boxed slice for version specifiers
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p-3
created_at: 2025-02-25T06:19:34Z
updated_at: 2025-02-25T06:30:57Z
url: https://github.com/astral-sh/uv/pull/11766
synced_at: 2026-01-10T11:10:38Z
```

# Use a Boxed slice for version specifiers

---

_Pull request opened by @charliermarsh on 2025-02-25 06:19_

## Summary

These are never modified, and we create _tons_ of them.


---

_Label `performance` added by @charliermarsh on 2025-02-25 06:19_

---

_Review requested from @konstin by @charliermarsh on 2025-02-25 06:20_

---

_Comment by @charliermarsh on 2025-02-25 06:24_

We could also consider using a `SmallVec` here, but this is an easy change.

---

_Merged by @charliermarsh on 2025-02-25 06:30_

---

_Closed by @charliermarsh on 2025-02-25 06:30_

---

_Branch deleted on 2025-02-25 06:30_

---
