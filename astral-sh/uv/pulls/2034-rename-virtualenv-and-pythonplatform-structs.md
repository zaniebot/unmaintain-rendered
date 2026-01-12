```yaml
number: 2034
title: "Rename `Virtualenv` and `PythonPlatform` structs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/python-platform
created_at: 2024-02-28T04:18:29Z
updated_at: 2024-02-28T15:04:56Z
url: https://github.com/astral-sh/uv/pull/2034
synced_at: 2026-01-12T16:04:50Z
```

# Rename `Virtualenv` and `PythonPlatform` structs

---

_@charliermarsh_

## Summary

`PythonPlatform` only exists to format paths to directories within virtual environments based on a root and an OS, so it's now `VirtualenvLayout`.

`Virtualenv` is now used for non-virtual environment Pythons, so it's now `PythonEnvironment`.

---

_Review requested from @konstin by @charliermarsh on 2024-02-28 04:18_

---

_Label `internal` added by @charliermarsh on 2024-02-28 04:18_

---

_@konstin approved on 2024-02-28 09:25_

---

_Merged by @charliermarsh on 2024-02-28 15:04_

---

_Closed by @charliermarsh on 2024-02-28 15:04_

---

_Branch deleted on 2024-02-28 15:04_

---
