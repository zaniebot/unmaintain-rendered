```yaml
number: 12925
title: "Fix `PythonDownloadRequest` parsing for partial keys"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-down-parse
created_at: 2025-04-16T19:20:28Z
updated_at: 2025-04-18T16:25:54Z
url: https://github.com/astral-sh/uv/pull/12925
synced_at: 2026-01-10T11:10:40Z
```

# Fix `PythonDownloadRequest` parsing for partial keys

---

_Pull request opened by @zanieb on 2025-04-16 19:20_

In #12909, I noticed we failed to parse partial download keys with `any` placeholders. Here, parsing for that is fixed.

---

_Label `bug` added by @zanieb on 2025-04-16 19:20_

---

_@zanieb reviewed on 2025-04-16 19:39_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:86 on 2025-04-16 19:39_

Previously, this would have been treated as an executable name request.

---

_Review requested from @Gankra by @zanieb on 2025-04-16 22:23_

---

_@Gankra approved on 2025-04-17 14:23_

Nice catch!

---

_Merged by @zanieb on 2025-04-18 16:25_

---

_Closed by @zanieb on 2025-04-18 16:25_

---

_Branch deleted on 2025-04-18 16:25_

---
