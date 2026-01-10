```yaml
number: 13330
title: Fix discovery of pre-release managed Python versions in range requests
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/range-contains
created_at: 2025-05-07T13:42:51Z
updated_at: 2025-05-07T14:27:28Z
url: https://github.com/astral-sh/uv/pull/13330
synced_at: 2026-01-10T11:10:41Z
```

# Fix discovery of pre-release managed Python versions in range requests

---

_Pull request opened by @zanieb on 2025-05-07 13:42_

We have test coverage for this elsewhere, but managed Python versions are a distinct case because we know the _full_ version before querying the interpreter (whereas, when we find them on the `PATH`, we usually only know `X.y` from the file name).

This pre-filter logic now matches our subsequent logic at

https://github.com/astral-sh/uv/blob/060be9cef197d83e5546fb1be10e8e8373a1cfc6/crates/uv-python/src/discovery.rs#L2146-L2149


https://github.com/astral-sh/uv/pull/13330/commits/060be9cef197d83e5546fb1be10e8e8373a1cfc6 shows the snapshot change.

---

_Review requested from @konstin by @zanieb on 2025-05-07 13:45_

---

_@charliermarsh approved on 2025-05-07 14:24_

---

_Merged by @zanieb on 2025-05-07 14:27_

---

_Closed by @zanieb on 2025-05-07 14:27_

---

_Branch deleted on 2025-05-07 14:27_

---

_Label `bug` added by @zanieb on 2025-05-07 14:27_

---
