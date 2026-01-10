```yaml
number: 13953
title: "Rename `DirectorySymlink` to the more descriptive `PythonMinorVersionLink`"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/rename-directory-symlink
created_at: 2025-06-10T18:33:33Z
updated_at: 2025-06-10T18:52:09Z
url: https://github.com/astral-sh/uv/pull/13953
synced_at: 2026-01-10T11:10:42Z
```

# Rename `DirectorySymlink` to the more descriptive `PythonMinorVersionLink`

---

_Pull request opened by @jtfmumm on 2025-06-10 18:33_

The `DirectorySymlink` struct assumes that it will be a Python minor version link, so this renaming makes the intention clearer.


---

_Label `internal` added by @jtfmumm on 2025-06-10 18:33_

---

_Review requested from @zanieb by @jtfmumm on 2025-06-10 18:35_

---

_@zanieb approved on 2025-06-10 18:37_

I'd probably include `Python` in the name, but don't feel strongly.

---

_Renamed from "Rename `DirectorySymlink` to the more descriptive `MinorVersionLink`" to "Rename `DirectorySymlink` to the more descriptive `PythonMinorVersionLink`" by @jtfmumm on 2025-06-10 18:40_

---

_Merged by @jtfmumm on 2025-06-10 18:52_

---

_Closed by @jtfmumm on 2025-06-10 18:52_

---

_Branch deleted on 2025-06-10 18:52_

---
