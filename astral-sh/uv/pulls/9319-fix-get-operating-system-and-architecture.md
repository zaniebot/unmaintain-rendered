```yaml
number: 9319
title: Fix get_operating_system_and_architecture
type: pull_request
state: merged
author: jooon
labels: []
assignees: []
merged: true
base: main
head: fix_interpreter_info
created_at: 2024-11-21T14:23:37Z
updated_at: 2024-11-22T23:23:24Z
url: https://github.com/astral-sh/uv/pull/9319
synced_at: 2026-01-12T16:08:45Z
```

# Fix get_operating_system_and_architecture

---

_@jooon_

_get_glibc_version() can after #9005 return either (0, 0) if glibc
string is missing or (-1, -1) if the string can't be parsed. There was
no need to change missing string to (0, 0).

Also, move back indentation to make it easier to understand.


---

_Review requested from @konstin by @zanieb on 2024-11-21 14:24_

---

_@charliermarsh reviewed on 2024-11-21 14:25_

---

_Review comment by @charliermarsh on `crates/uv-python/python/packaging/_manylinux.py`:176 on 2024-11-21 14:25_

Yeah I don't think this should have changed. \cc @konstin 

---

_@charliermarsh approved on 2024-11-21 14:25_

---

_@charliermarsh reviewed on 2024-11-21 14:29_

---

_Review comment by @charliermarsh on `crates/uv-python/python/packaging/_manylinux.py`:176 on 2024-11-21 14:29_

(In part because this is vendored.)

---

_Merged by @charliermarsh on 2024-11-21 20:05_

---

_Closed by @charliermarsh on 2024-11-21 20:05_

---

_@samypr100 reviewed on 2024-11-21 23:44_

---

_Review comment by @samypr100 on `crates/uv-python/python/packaging/_manylinux.py`:176 on 2024-11-21 23:44_

Yea, I implied that on my comment in the PR but I wasn't clear enough.

---

_Branch deleted on 2024-11-22 23:23_

---
