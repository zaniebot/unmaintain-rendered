```yaml
number: 15482
title: Move preview features into a dedicated crate
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pre
created_at: 2025-08-24T13:17:22Z
updated_at: 2025-08-24T15:02:57Z
url: https://github.com/astral-sh/uv/pull/15482
synced_at: 2026-01-10T06:44:33Z
```

# Move preview features into a dedicated crate

---

_Pull request opened by @charliermarsh on 2025-08-24 13:17_

## Summary

This is causing some cyclic dependencies issues for me, because these can be used in virtually _any_ crate (like `uv-install-wheel`), which then means that all of `uv-configuration` becomes a dependency, etc. I think this should be a leaf crate so that we can safely depend on it anywhere.


---

_Review requested from @zanieb by @charliermarsh on 2025-08-24 13:17_

---

_Label `internal` added by @charliermarsh on 2025-08-24 13:17_

---

_Marked ready for review by @charliermarsh on 2025-08-24 13:18_

---

_Merged by @charliermarsh on 2025-08-24 13:55_

---

_Closed by @charliermarsh on 2025-08-24 13:55_

---

_Branch deleted on 2025-08-24 13:55_

---

_Comment by @zanieb on 2025-08-24 15:02_

I _sort_ of think it makes sense to lower the preview value into a crate-specific setting if it needs to be propagated somewhere, but this is also fine. 

---
