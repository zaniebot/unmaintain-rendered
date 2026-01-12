```yaml
number: 1254
title: Multiple entries in PUFFIN_PYTHON_PATH for windows tests
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/puffin-python-path-windows
created_at: 2024-02-05T22:06:47Z
updated_at: 2024-02-06T19:28:31Z
url: https://github.com/astral-sh/uv/pull/1254
synced_at: 2026-01-12T16:04:32Z
```

# Multiple entries in PUFFIN_PYTHON_PATH for windows tests

---

_@konstin_

There are no binary installers for the latests patch versions of cpython for windows, and building them is hard. As an alternative, we download python-build-standanlone cpythons and put them into `<project root>/bin`. On unix, we can symlink `pythonx.y.z` into this directory and point `PUFFIN_PYTHON_PATH` to it. On windows, all pythons are called `python.exe` and they don't like being linked. Instead, we add the path to each directory containing a `python.exe` to `PUFFIN_PYTHON_PATH`, similar to the regular `PATH`. The python discovery on windows was extended to respect `PUFFIN_PYTHON_PATH` where needed.

These changes mean that we don't need to (sym)link pythons anymore and could drop that part to the script.

435 tests run: 389 passed (21 slow), 46 failed, 1 skipped

---

_Label `windows` added by @konstin on 2024-02-05 22:06_

---

_Marked ready for review by @konstin on 2024-02-05 22:34_

---

_Review requested from @zanieb by @konstin on 2024-02-05 22:34_

---

_@zanieb approved on 2024-02-06 19:17_

---

_Merged by @konstin on 2024-02-06 19:28_

---

_Closed by @konstin on 2024-02-06 19:28_

---

_Branch deleted on 2024-02-06 19:28_

---
