```yaml
number: 3616
title: "Add `Combine` implementations for non-`Vec` types"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/combine
created_at: 2024-05-15T18:25:10Z
updated_at: 2024-05-15T18:40:56Z
url: https://github.com/astral-sh/uv/pull/3616
synced_at: 2026-01-10T14:37:54Z
```

# Add `Combine` implementations for non-`Vec` types

---

_Pull request opened by @charliermarsh on 2024-05-15 18:25_

## Summary

Now, we just call `.combine` on all types, rather than needing to be aware of whether or not it's a vector. Previously, only `Option<Vec<T>>` had an implementation, so we had to specifically call `.combine` on vector types and `.or` on non-vector types. If you called `.or` on a non-vector type, the code would compile, but with the wrong behavior. This way, we get clear guidance from the compiler and a guarantee of correct behavior as we introduce new settings.


---

_Label `internal` added by @charliermarsh on 2024-05-15 18:25_

---

_Marked ready for review by @charliermarsh on 2024-05-15 18:25_

---

_Merged by @charliermarsh on 2024-05-15 18:40_

---

_Closed by @charliermarsh on 2024-05-15 18:40_

---

_Branch deleted on 2024-05-15 18:40_

---
