```yaml
number: 11115
title: Avoid fallback to PyPI in mixed CPU/CUDA example
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pt-darwin
created_at: 2025-01-30T18:59:52Z
updated_at: 2025-03-03T03:35:34Z
url: https://github.com/astral-sh/uv/pull/11115
synced_at: 2026-01-12T16:09:41Z
```

# Avoid fallback to PyPI in mixed CPU/CUDA example

---

_@charliermarsh_

## Summary

This is roughly equivalent, but gets the non-`+cpu` macOS build from the PyTorch index rather than PyPI. It seems a bit simpler? Though up for debate.


---

_Label `documentation` added by @charliermarsh on 2025-01-30 19:00_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 19:00_

---

_Comment by @charliermarsh on 2025-01-30 19:00_

Sort of torn on this. It does the same thing, but I don't know what's more intuitive.

---

_@zanieb approved on 2025-01-30 19:04_

Not needing to explain a fallback for macOS makes sense to me. Should you use `== darwin and == win32` instead of a negative marker again...?

---

_Comment by @charliermarsh on 2025-01-30 19:05_

Yeah... But that _would_ probably end up with something in the lockfile like: `sys_platform != 'win32' and sys_platform != 'darwin' and sys_platform != 'linux'`, for PyPI.

---

_Comment by @zanieb on 2025-01-30 19:31_

> But that would probably end up with something in the lockfile like: sys_platform != 'win32' and sys_platform != 'darwin' and sys_platform != 'linux', for PyPI.

That seems okay to me? Or at least seems explicable.

---

_Merged by @charliermarsh on 2025-03-03 03:35_

---

_Closed by @charliermarsh on 2025-03-03 03:35_

---

_Branch deleted on 2025-03-03 03:35_

---
