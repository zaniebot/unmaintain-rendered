---
number: 4099
title: Enforce constraints eagerly when forking on markers
type: issue
state: closed
author: blueraft
labels:
  - bug
assignees: []
created_at: 2024-06-06T14:21:11Z
updated_at: 2024-06-07T19:57:03Z
url: https://github.com/astral-sh/uv/issues/4099
synced_at: 2026-01-07T13:12:17-06:00
---

# Enforce constraints eagerly when forking on markers

---

_Issue opened by @blueraft on 2024-06-06 14:21_

Despite specifying urllib3>=1.22.0 in my dependencies, uv lock traverses down to an incompatible version urllib3==1.2.1, which fails to build on my machine.

```toml
[project]
name = 'foobar'
dynamic = ["version"]
requires-python = ">=3.9, <4"

dependencies = [
  'urllib3>=1.22.0',
  'elasticsearch==7.17.1',
  'dockerspawner==13.0.0',
]
```

It works fine with `uv pip compile` though.

---

_Comment by @charliermarsh on 2024-06-06 14:33_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-06-06 14:34_

---

_Comment by @charliermarsh on 2024-06-06 14:36_

So at least one problem here is that we're not pruning branches based on the Python version. We end up searching branches to satisfy `python_version == '3.3'`.

---

_Comment by @charliermarsh on 2024-06-06 14:38_

The other issue seems to be that we're losing the original constraints when we fork with a marker.

---

_Comment by @charliermarsh on 2024-06-06 14:41_

I think that latter needs to be solved by something like a `Marker` package, similar to what we did for extras.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 14:43_

---

_Comment by @charliermarsh on 2024-06-06 14:43_

I'll use this issue to track the second problem.

---

_Comment by @charliermarsh on 2024-06-06 14:44_

Added Python version thing here: https://github.com/astral-sh/uv/issues/4102

---

_Renamed from "Unexpected dependency resolution with uv lock" to "Enforce constraints eagerly when forking on markers" by @charliermarsh on 2024-06-06 14:45_

---

_Referenced in [astral-sh/uv#4123](../../astral-sh/uv/pulls/4123.md) on 2024-06-07 02:13_

---

_Closed by @charliermarsh on 2024-06-07 19:57_

---
