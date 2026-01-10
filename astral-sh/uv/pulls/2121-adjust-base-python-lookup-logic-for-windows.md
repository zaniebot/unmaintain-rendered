```yaml
number: 2121
title: Adjust base Python lookup logic for Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/home
created_at: 2024-03-01T20:47:18Z
updated_at: 2024-03-03T17:47:24Z
url: https://github.com/astral-sh/uv/pull/2121
synced_at: 2026-01-10T14:54:43Z
```

# Adjust base Python lookup logic for Windows

---

_Pull request opened by @charliermarsh on 2024-03-01 20:47_

## Summary

When I install via the Windows Store, `interpreter.base_prefix` contains a bunch of resolved information that leads to a broken environment.

Instead, we now use `sys._base_executable` on Windows by default, falling back to `sys.base_prefix` if it doesn't exist. (There are some issues with `sys.base_executable` that lead to complexity in `virtualenv`, but they only affect POSIX.) Admittedly, I don't know when `sys._base_executable` wouldn't exist. It exists in all the environments I've tested.

Additionally, we use the system interpreter directly if we're outside of a virtualenv.

---

_Label `windows` added by @charliermarsh on 2024-03-01 20:47_

---

_Review requested from @konstin by @charliermarsh on 2024-03-01 20:47_

---

_@charliermarsh reviewed on 2024-03-01 21:02_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:209 on 2024-03-01 21:02_

@konstin - What was the original motivation for this? How would I get it to break?

---

_@charliermarsh reviewed on 2024-03-02 17:23_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:209 on 2024-03-02 17:23_

Ok I realized why this is here: when creating a virtualenv from within a virtualenv? What I have above wouldn’t work on Windows, I think. I’ll change to use _base_executable and then fall back to base_prefix.

---

_@zanieb reviewed on 2024-03-02 19:37_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/bare.rs`:209 on 2024-03-02 19:37_

Creating from within a venv seems feasible to add unit test coverage for right?

---

_Converted to draft by @charliermarsh on 2024-03-02 20:16_

---

_@charliermarsh reviewed on 2024-03-02 20:16_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/bare.rs`:209 on 2024-03-02 20:16_

You ask too much of me...

---

_Comment by @charliermarsh on 2024-03-02 20:16_

Moving to draft to (1) add tests and (2) change Windows logic a bit.

---

_Marked ready for review by @charliermarsh on 2024-03-02 22:00_

---

_Renamed from "Always use interpreter parent for home" to "Adjust base Python lookup logic for Windows" by @charliermarsh on 2024-03-02 22:01_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-02 22:02_

---

_Label `bug` added by @charliermarsh on 2024-03-02 22:02_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/bare.rs`:64 on 2024-03-03 17:38_

Could you add a note that this more complex case is for windows store python?

---

_@konstin approved on 2024-03-03 17:38_

---

_@konstin reviewed on 2024-03-03 17:38_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/bare.rs`:64 on 2024-03-03 17:38_

I've only now seen #2122 does this

---

_Merged by @charliermarsh on 2024-03-03 17:44_

---

_Closed by @charliermarsh on 2024-03-03 17:44_

---

_Branch deleted on 2024-03-03 17:44_

---

_Comment by @konstin on 2024-03-03 17:47_

Linking https://github.com/python/cpython/issues/114476 for future reference

---
