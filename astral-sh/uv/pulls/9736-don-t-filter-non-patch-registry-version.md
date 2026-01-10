```yaml
number: 9736
title: "Don't filter non-patch registry version"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/windows-registry-no-patch
created_at: 2024-12-09T10:18:18Z
updated_at: 2024-12-09T19:58:59Z
url: https://github.com/astral-sh/uv/pull/9736
synced_at: 2026-01-10T12:00:01Z
```

# Don't filter non-patch registry version

---

_Pull request opened by @konstin on 2024-12-09 10:18_

The `SysVersion` registry entry may or may not include the patch version, so if we encounter a registry entry without a patch version, we must not assume that the patch version is 0.

```
Name                           Property
----                           --------
3.9                            DisplayName     : Python 3.9 (64-bit)
                               SupportUrl      : https://www.python.org/
                               Version         : 3.9.13
                               SysVersion      : 3.9
                               SysArchitecture : 64bit

    Hive: HKEY_CURRENT_USER\Software\Python\PythonCore\3.9
```

Confirmed the fix manually.

Fixes #9668

---

_Label `bug` added by @konstin on 2024-12-09 10:18_

---

_Review requested from @zanieb by @konstin on 2024-12-09 10:18_

---

_@zanieb approved on 2024-12-09 18:43_

---

_Merged by @konstin on 2024-12-09 19:58_

---

_Closed by @konstin on 2024-12-09 19:58_

---

_Branch deleted on 2024-12-09 19:58_

---
