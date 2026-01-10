```yaml
number: 1758
title: Add musl to python bootstrapping script
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/boostrap-python-on-musl
created_at: 2024-02-20T15:07:08Z
updated_at: 2024-02-22T09:46:49Z
url: https://github.com/astral-sh/uv/pull/1758
synced_at: 2026-01-10T14:54:43Z
```

# Add musl to python bootstrapping script

---

_Pull request opened by @konstin on 2024-02-20 15:07_

Previously, only glibc builds were tracked in the bootstrap script. A new field `libc` tracks if `gnu` or `musl` are used on linux, while it is `"none"` everywhere else. I've confirmed that the updated script works on ubuntu and alpine.


---

_@zanieb approved on 2024-02-20 15:56_

---

_Comment by @zanieb on 2024-02-21 00:29_

Yes this works on my machine.

---

_Comment by @konstin on 2024-02-22 09:46_

Zanie tested on mac, i tested on ubuntu and windows

---

_Merged by @konstin on 2024-02-22 09:46_

---

_Closed by @konstin on 2024-02-22 09:46_

---

_Branch deleted on 2024-02-22 09:46_

---
