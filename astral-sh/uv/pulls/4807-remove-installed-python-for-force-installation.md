```yaml
number: 4807
title: Remove installed python for force installation
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: force-reinstall
created_at: 2024-07-04T14:41:03Z
updated_at: 2024-07-04T15:59:44Z
url: https://github.com/astral-sh/uv/pull/4807
synced_at: 2026-01-10T13:48:28Z
```

# Remove installed python for force installation

---

_Pull request opened by @j178 on 2024-07-04 14:41_

## Summary

Currently `uv python install` does not respect `--force` reinstallation, the downloading process is just skipped if the installation existed.

```
$ uv python install 3.12

Looking for installation Python 3.12 (any-3.12-any-any-any)
Downloading cpython-3.12.3-windows-x86_64-none
Installed Python 3.12.3 to C:\Users\jo\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none
Installed 1 installation in 6s

$ uv python install --force 3.12

Looking for installation Python 3.12 (any-3.12-any-any-any)
Found installed installation `cpython-3.12.3-windows-x86_64-none` that satisfies Python 3.12
Downloading cpython-3.12.3-windows-x86_64-none
Installed 1 installation in 0s
```

## Test Plan


```
$ uv python install 3.12

Looking for installation Python 3.12 (any-3.12-any-any-any)
Downloading cpython-3.12.3-windows-x86_64-none
Installed Python 3.12.3 to C:\Users\jo\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none
Installed 1 installation in 6s

$ uv python install --force 3.12

Looking for installation Python 3.12 (any-3.12-any-any-any)
Found installed installation `cpython-3.12.3-windows-x86_64-none` that satisfies Python 3.12
Removing installed installation `cpython-3.12.3-windows-x86_64-none`
Downloading cpython-3.12.3-windows-x86_64-none
Installed Python 3.12.3 to C:\Users\jo\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none
Installed 1 installation in 7s
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-04 15:04_

---

_Label `bug` added by @charliermarsh on 2024-07-04 15:04_

---

_@charliermarsh approved on 2024-07-04 15:13_

Thanks!

---

_Merged by @charliermarsh on 2024-07-04 15:13_

---

_Closed by @charliermarsh on 2024-07-04 15:13_

---

_@zanieb reviewed on 2024-07-04 15:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:76 on 2024-07-04 15:13_

```suggestion
                    "Removing existing installation `{}`",
```

---

_Comment by @zanieb on 2024-07-04 15:13_

lol @charliermarsh 

---

_Comment by @zanieb on 2024-07-04 15:14_

Updating the output in https://github.com/astral-sh/uv/pull/4808

---

_Branch deleted on 2024-07-04 15:59_

---
