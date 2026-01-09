---
number: 5044
title: Simplify union of marker expression and negation
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2024-07-14T14:11:14Z
updated_at: 2024-07-14T23:53:59Z
url: https://github.com/astral-sh/uv/issues/5044
synced_at: 2026-01-07T13:12:17-06:00
---

# Simplify union of marker expression and negation

---

_Issue opened by @konstin on 2024-07-14 14:11_

We currently don't simplify the union of marker expression and its negation, e.g. `sys_platform == 'darwin' or sys_platform != 'darwin'`.

As an example, for `echo "colabfold==1.5.5" | cargo run pip compile -p 3.12 --universal -`, we get:

```
[...]
tensorflow-macos==2.13.1 ; sys_platform == 'darwin'
    # via colabfold
[...]
werkzeug==3.0.3 ; sys_platform == 'darwin' or sys_platform != 'darwin'
    # via tensorboard
wheel==0.43.0 ; sys_platform == 'darwin' or sys_platform != 'darwin'
    # via
    #   astunparse
    #   tensorboard
wrapt==1.16.0 ; sys_platform == 'darwin' or sys_platform != 'darwin'
    # via
    #   tensorflow-cpu
    #   tensorflow-macos
zipp==3.19.2
    # via importlib-metadata
```

The above `sys_platform == 'darwin' or sys_platform != 'darwin'` markers should be elided, these package aren't fork dependent.

---

_Label `enhancement` added by @konstin on 2024-07-14 14:11_

---

_Comment by @charliermarsh on 2024-07-14 14:56_

Good call.

---

_Comment by @charliermarsh on 2024-07-14 16:32_

\cc @ibraheemdev 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-14 23:40_

---

_Referenced in [astral-sh/uv#5050](../../astral-sh/uv/pulls/5050.md) on 2024-07-14 23:43_

---

_Closed by @charliermarsh on 2024-07-14 23:53_

---

_Referenced in [astral-sh/uv#5078](../../astral-sh/uv/issues/5078.md) on 2024-07-15 17:56_

---
