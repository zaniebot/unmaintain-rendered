```yaml
number: 11714
title: Use boxed slices for hash vector
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p-5
created_at: 2025-02-22T22:13:21Z
updated_at: 2025-02-24T17:11:47Z
url: https://github.com/astral-sh/uv/pull/11714
synced_at: 2026-01-12T16:09:57Z
```

# Use boxed slices for hash vector

---

_@charliermarsh_

## Summary

We never resize these, and they're stored everywhere (on `File`, etc.). Seems useful to use a more efficient structure for them.


---

_Label `performance` added by @charliermarsh on 2025-02-22 22:13_

---

_@konstin approved on 2025-02-24 17:04_

---

_Merged by @zanieb on 2025-02-24 17:11_

---

_Closed by @zanieb on 2025-02-24 17:11_

---

_Branch deleted on 2025-02-24 17:11_

---
