```yaml
number: 5456
title: "Infer missing `.exe` in Windows Python discovery"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/exe
created_at: 2024-07-25T18:09:14Z
updated_at: 2024-07-25T21:05:48Z
url: https://github.com/astral-sh/uv/pull/5456
synced_at: 2026-01-12T16:06:49Z
```

# Infer missing `.exe` in Windows Python discovery

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5445.


---

_Label `enhancement` added by @charliermarsh on 2024-07-25 18:09_

---

_Marked ready for review by @charliermarsh on 2024-07-25 18:09_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-25 18:23_

---

_@zanieb reviewed on 2024-07-25 19:01_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1104 on 2024-07-25 19:01_

We'll take this branch if we have "python.foo" with this logic, right? (which seems incorrect)

---

_@charliermarsh reviewed on 2024-07-25 20:42_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1104 on 2024-07-25 20:42_

(Changed to `is_none`.)

---

_Merged by @charliermarsh on 2024-07-25 20:57_

---

_Closed by @charliermarsh on 2024-07-25 20:57_

---

_Branch deleted on 2024-07-25 20:57_

---

_@zanieb reviewed on 2024-07-25 21:05_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1104 on 2024-07-25 21:05_

Great thanks!

---
