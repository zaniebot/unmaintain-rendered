---
number: 658
title: "Missing version parsing fixup: ` >=\"3.6\"`"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-12-15T11:06:25Z
updated_at: 2023-12-15T18:11:23Z
url: https://github.com/astral-sh/uv/issues/658
synced_at: 2026-01-10T01:23:04Z
---

# Missing version parsing fixup: ` >="3.6"`

---

_Issue opened by @konstin on 2023-12-15 11:06_

https://pypi.org/simple/tensorflowonspark/?format=application/vnd.pypi.simple.v1+json

```
No solution found when resolving: tensorflowonspark

Caused by:
    0: Received some unexpected JSON from https://pypi.org/simple/tensorflowonspark/?format=application/vnd.pypi.simple.v1+json
    1: Failed to parse version:
       >="3.6"
       ^^^^^^^
        at line 1 column 18915
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 17:01_

---

_Label `bug` added by @charliermarsh on 2023-12-15 17:01_

---

_Referenced in [astral-sh/uv#663](../../astral-sh/uv/pulls/663.md) on 2023-12-15 18:07_

---

_Closed by @charliermarsh on 2023-12-15 18:11_

---
