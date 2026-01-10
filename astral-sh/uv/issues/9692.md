```yaml
number: 9692
title: "`--project` and `lock` produces incorrect relative path in lock file"
type: issue
state: closed
author: JamesMTSloan
labels:
  - bug
assignees: []
created_at: 2024-12-06T18:09:28Z
updated_at: 2024-12-07T20:16:15Z
url: https://github.com/astral-sh/uv/issues/9692
synced_at: 2026-01-10T04:36:21Z
```

# `--project` and `lock` produces incorrect relative path in lock file

---

_Issue opened by @JamesMTSloan on 2024-12-06 18:09_

## Minimal reproduction
Running in `/home/user/projects/uv_test` with `uv` 0.5.6.
```bash
mkdir other
mkdir project
cd project
uv init
cd ../other
uv --project ../project/ lock
uv --project ../project/ run python3 hello.py
```

Gives the following output:
```bash
Initialized project `project`
Using CPython 3.10.12 interpreter at: /usr/bin/python3.10
Resolved 1 package in 2ms

Using CPython 3.10.12 interpreter at: /usr/bin/python3.10
Creating virtual environment at: ../project/.venv
error: Failed to generate package metadata for `project==0.1.0 @ virtual+../../../project`
  Caused by: Distribution not found at: file:///home/user/project
```

Contents of project/uv.lock:
```
version = 1
requires-python = ">=3.10"

[[package]]
name = "project"
version = "0.1.0"
source = { virtual = "../../../project" }
```
---

The relative path that ends up in the lock file is incorrect. Using `--directory` instead of `--project` produces the expected `"."` path.
Nor is the path produced, `"../../../project"`, the one given as `--project` - it ends up pointing to a somewhat arbitrary directory and so with some bad luck could produce some very unexpected results if it happens to hit a different valid project. In my case the concrete project is at `/home/user/projects/uv_test/project` with `uv` looking in `/home/user/project`.

To be clear, in my usecase switching to `--directory` worked. Maybe this is expected behaviour but I am reporting it as it surprised me.

---

_Comment by @charliermarsh on 2024-12-06 18:23_

Might be a bug? I'll take a look.

---

_Label `bug` added by @charliermarsh on 2024-12-06 21:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-07 18:29_

---

_Closed by @charliermarsh on 2024-12-07 20:16_

---

_Closed by @charliermarsh on 2024-12-07 20:16_

---
