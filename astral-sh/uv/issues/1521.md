```yaml
number: 1521
title: PEP 514 support
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - windows
assignees: []
created_at: 2024-02-16T18:13:18Z
updated_at: 2024-08-29T20:48:24Z
url: https://github.com/astral-sh/uv/issues/1521
synced_at: 2026-01-10T04:45:09Z
```

# PEP 514 support

---

_Issue opened by @zanieb on 2024-02-16 18:13_

https://peps.python.org/pep-0514/

We use `py` to find Python installations on Windows today but we want to switch to registry look-ups per PEP 514

---

_Label `windows` added by @zanieb on 2024-02-16 18:13_

---

_Comment by @gaborbernat on 2024-02-16 19:29_

Note virtualenv already does this via https://github.com/pypa/virtualenv/blob/main/src/virtualenv/discovery/windows/pep514.py 👍 

---

_Comment by @gaborbernat on 2024-02-16 20:37_

`tox-uv` seems to be failing on Github Actions without this https://github.com/tox-dev/tox-uv/actions/runs/7935511069?pr=2

---

_Label `help wanted` added by @zanieb on 2024-08-22 15:23_

---

_Closed by @konstin on 2024-08-29 20:48_

---

_Closed by @konstin on 2024-08-29 20:48_

---
