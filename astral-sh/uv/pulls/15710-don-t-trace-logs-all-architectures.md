```yaml
number: 15710
title: "Don't trace logs all architectures"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/dont-trace-log-all-architectures
created_at: 2025-09-07T11:08:52Z
updated_at: 2025-09-08T13:20:56Z
url: https://github.com/astral-sh/uv/pull/15710
synced_at: 2026-01-12T16:11:53Z
```

# Don't trace logs all architectures

---

_@konstin_

On my machine, this statement print over 500 lines for `uv python list -vv` of evident statements.


---

_Label `internal` added by @konstin on 2025-09-07 11:08_

---

_@charliermarsh approved on 2025-09-07 15:06_

---

_Merged by @konstin on 2025-09-07 15:31_

---

_Closed by @konstin on 2025-09-07 15:31_

---

_Branch deleted on 2025-09-07 15:31_

---

_Comment by @zanieb on 2025-09-07 16:57_

Related https://github.com/astral-sh/uv/issues/15268



---

_Comment by @zanieb on 2025-09-07 16:57_

We should find some way to retain the relevant information here without being so verbose.

---

_Comment by @konstin on 2025-09-08 06:48_

What's the information we want to show?

---

_Comment by @zanieb on 2025-09-08 13:20_

The reason we don't consider an architecture compatible, e.g., when selecting a Python interpreter or download.

---

_Comment by @zanieb on 2025-09-08 13:20_

e.g., as we do for the OS and Libc logs.

---
