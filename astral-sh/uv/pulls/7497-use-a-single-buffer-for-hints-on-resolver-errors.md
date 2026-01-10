```yaml
number: 7497
title: Use a single buffer for hints on resolver errors
type: pull_request
state: merged
author: lucab
labels:
  - performance
assignees: []
merged: true
base: main
head: ups/resolver-recursive-hints
created_at: 2024-09-18T13:11:25Z
updated_at: 2024-09-18T14:16:26Z
url: https://github.com/astral-sh/uv/pull/7497
synced_at: 2026-01-10T12:53:48Z
```

# Use a single buffer for hints on resolver errors

---

_Pull request opened by @lucab on 2024-09-18 13:11_

This changes the structure of the hints generator in the resolver when encountering solution errors, so that it re-uses a single output buffer owned by the caller.
It avoids repeated allocations of a temporary buffer within each recursive function call.

---

_@zanieb approved on 2024-09-18 13:15_

Seems reasonable to me. There's a lot of unoptimized code in the no solution error path.

---

_Label `performance` added by @zanieb on 2024-09-18 13:15_

---

_Marked ready for review by @lucab on 2024-09-18 13:22_

---

_Merged by @lucab on 2024-09-18 14:16_

---

_Closed by @lucab on 2024-09-18 14:16_

---

_Branch deleted on 2024-09-18 14:16_

---
