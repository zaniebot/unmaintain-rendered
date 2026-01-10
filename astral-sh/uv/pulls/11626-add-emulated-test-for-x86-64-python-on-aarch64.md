```yaml
number: 11626
title: Add emulated test for x86-64 Python on aarch64 Windows
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/aarch64-em
created_at: 2025-02-19T16:55:04Z
updated_at: 2025-02-19T18:02:16Z
url: https://github.com/astral-sh/uv/pull/11626
synced_at: 2026-01-10T11:10:38Z
```

# Add emulated test for x86-64 Python on aarch64 Windows

---

_Pull request opened by @zanieb on 2025-02-19 16:55_

Coverage of https://github.com/astral-sh/uv/pull/11625 for unmanaged Python.

---

_Label `testing` added by @zanieb on 2025-02-19 16:55_

---

_@Gankra approved on 2025-02-19 17:27_

---

_Comment by @zanieb on 2025-02-19 17:49_

This shouldn't pass today?

---

_Comment by @zanieb on 2025-02-19 17:50_

I guess we already allow x64 Python with the ARM uv?  Surprising

```
DEBUG Searching for default Python interpreter in registry or search path
DEBUG Found `cpython-3.13.2-windows-x86_64-none` at `C:\a\_tool\Python\3.13.2\x64\python.exe` (first executable in the search path)
```

---

_Merged by @zanieb on 2025-02-19 18:02_

---

_Closed by @zanieb on 2025-02-19 18:02_

---

_Branch deleted on 2025-02-19 18:02_

---
