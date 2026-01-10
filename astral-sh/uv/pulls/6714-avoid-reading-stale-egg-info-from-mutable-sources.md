```yaml
number: 6714
title: "Avoid reading stale `.egg-info` from mutable sources"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/stale
created_at: 2024-08-27T19:10:23Z
updated_at: 2024-08-27T19:23:27Z
url: https://github.com/astral-sh/uv/pull/6714
synced_at: 2026-01-10T13:09:51Z
```

# Avoid reading stale `.egg-info` from mutable sources

---

_Pull request opened by @charliermarsh on 2024-08-27 19:10_

## Summary

In theory this problem already existed for `PKG-INFO`, but `egg-info` would be more common, I think, since it's built in the source tree.

Closes https://github.com/astral-sh/uv/issues/6712.


---

_Label `bug` added by @charliermarsh on 2024-08-27 19:10_

---

_Comment by @charliermarsh on 2024-08-27 19:11_

Writing tests...

---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 19:16_

---

_@zanieb approved on 2024-08-27 19:18_

---

_Merged by @charliermarsh on 2024-08-27 19:23_

---

_Closed by @charliermarsh on 2024-08-27 19:23_

---

_Branch deleted on 2024-08-27 19:23_

---
