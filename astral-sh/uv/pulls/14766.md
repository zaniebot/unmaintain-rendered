```yaml
number: 14766
title: "Use a match for Windows executables in `venv`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/m
created_at: 2025-07-20T22:10:27Z
updated_at: 2025-07-21T14:48:54Z
url: https://github.com/astral-sh/uv/pull/14766
synced_at: 2026-01-10T06:53:02Z
```

# Use a match for Windows executables in `venv`

---

_Pull request opened by @charliermarsh on 2025-07-20 22:10_

## Summary

I found it confusing that the `else` case for `== "graalpy"` is still necessary for the `== "pypy"` branch (i.e., that `pythonw.exe` is copied for PyPy despite not being in the `== "pypy"` branch).

Instead, we now use a match for PyP, GraalPy, and then everything else.


---

_Renamed from "Use a match for Windows executables in venv" to "Use a match for Windows executables in `venv`" by @charliermarsh on 2025-07-20 22:10_

---

_Label `internal` added by @charliermarsh on 2025-07-20 22:10_

---

_Marked ready for review by @charliermarsh on 2025-07-20 22:10_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-20 22:10_

---

_Review requested from @jtfmumm by @charliermarsh on 2025-07-20 22:10_

---

_@charliermarsh reviewed on 2025-07-20 22:11_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:320 on 2025-07-20 22:11_

Seems wrong that we omit `PythonMajorw` and `PythonMajorMinorw`, but that's the existing behavior.

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:280 on 2025-07-21 12:38_

Nit: This is available without `markers()`.

I'd also consider using `LenientImplementationName`

---

_@zanieb reviewed on 2025-07-21 12:38_

---

_@jtfmumm approved on 2025-07-21 14:36_

---

_@charliermarsh reviewed on 2025-07-21 14:40_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:280 on 2025-07-21 14:40_

Fixed. I left it as-is for now w/r/t `LenientImplementationName`, I'd just rather not change it unless we have a reason to.

---

_Merged by @charliermarsh on 2025-07-21 14:48_

---

_Closed by @charliermarsh on 2025-07-21 14:48_

---

_Branch deleted on 2025-07-21 14:48_

---
