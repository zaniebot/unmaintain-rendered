```yaml
number: 11890
title: Avoid silently dropping errors in directory enumeration
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2025-03-02T01:15:11Z
updated_at: 2025-03-03T02:39:18Z
url: https://github.com/astral-sh/uv/pull/11890
synced_at: 2026-01-12T16:10:02Z
```

# Avoid silently dropping errors in directory enumeration

---

_@charliermarsh_

## Summary

Right now, _all_ errors are dropped here, which seems wrong. We should only return an empty iterator if the directory doesn't exist.


---

_Label `bug` added by @charliermarsh on 2025-03-02 01:15_

---

_Marked ready for review by @charliermarsh on 2025-03-02 01:15_

---

_@zanieb reviewed on 2025-03-02 01:18_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:546 on 2025-03-02 01:18_

I worry a little about adding errors for permission errors, given how common it is for people to share the cache across users. I presume that would always fail later on write anyway though?

---

_@charliermarsh reviewed on 2025-03-02 01:21_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:546 on 2025-03-02 01:21_

We can add handling for that at the consumer site if we want, I'll look. But they should not be dropped by this method. Consumers should need to care, I think.

---

_Merged by @charliermarsh on 2025-03-03 02:39_

---

_Closed by @charliermarsh on 2025-03-03 02:39_

---

_Branch deleted on 2025-03-03 02:39_

---
