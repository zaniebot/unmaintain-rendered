```yaml
number: 6818
title: "`uv add` should roll back changes to `pyproject.toml` if the automatic sync is cancelled"
type: issue
state: closed
author: akx
labels:
  - bug
  - projects
assignees: []
created_at: 2024-08-29T15:54:18Z
updated_at: 2024-09-04T15:04:01Z
url: https://github.com/astral-sh/uv/issues/6818
synced_at: 2026-01-10T04:45:09Z
```

# `uv add` should roll back changes to `pyproject.toml` if the automatic sync is cancelled

---

_Issue opened by @akx on 2024-08-29 15:54_

uv 0.4.0, macOS.

A minimal code snippet that reproduces the issue:

```
$ uv init foo
$ cd foo
$ uv add pyobjc
💭 oh crud, I don't need every single pyobjc framework! that's a lot of framework!
^C
$ uv add pyobjc-core
💭 oh crud, it's still downloading all frameworks! what's happening?!
^C
$ cat pyproject.toml
[...]
dependencies = [
    "pyobjc",
    "pyobjc-core",
]
$
```

IOW, I'd expect that if I cancel the `uv add` operation (even if it's made up of `add` and implicit `sync` afterwards), `pyproject.toml` was back to the state it was before `uv add`.

---

_Label `bug` added by @zanieb on 2024-08-29 16:40_

---

_Label `projects` added by @zanieb on 2024-08-29 16:40_

---

_Comment by @charliermarsh on 2024-08-29 20:02_

(We correctly do this if `lock` or `sync` fail, but not if the program is interrupted.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-04 14:42_

---

_Closed by @charliermarsh on 2024-09-04 15:04_

---
