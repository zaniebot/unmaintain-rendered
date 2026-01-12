```yaml
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
synced_at: 2026-01-12T15:58:24Z
```

# Missing version parsing fixup: ` >="3.6"`

---

_@konstin_

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

_Closed by @charliermarsh on 2023-12-15 18:11_

---
