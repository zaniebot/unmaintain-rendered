```yaml
number: 3443
title: Remove unconstrained version error from requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/const
created_at: 2024-05-07T23:45:46Z
updated_at: 2024-05-08T07:03:55Z
url: https://github.com/astral-sh/uv/pull/3443
synced_at: 2026-01-12T16:05:39Z
```

# Remove unconstrained version error from requirements

---

_@charliermarsh_

## Summary

It's not clear to me that this should exist at all, but it's causing errors in projects that don't use `tool.uv.sources`, so we should definitely remove it for now.


---

_Label `bug` added by @charliermarsh on 2024-05-07 23:45_

---

_Review requested from @konstin by @charliermarsh on 2024-05-07 23:53_

---

_Merged by @charliermarsh on 2024-05-08 00:04_

---

_Closed by @charliermarsh on 2024-05-08 00:04_

---

_Branch deleted on 2024-05-08 00:04_

---

_Comment by @henryiii on 2024-05-08 02:47_

The Rust ecosystem pushes constraints, but they are an anti-pattern in the Python ecosystem: https://iscinumpy.dev/post/bound-version-constraints

---

_Comment by @konstin on 2024-05-08 07:03_

This should have only been applied to files with tool.uv.sources to ensure people put at least a lower bound

---
