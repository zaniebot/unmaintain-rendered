```yaml
number: 83
title: "F401 `__future__.annotations` imported but unused"
type: issue
state: closed
author: orsinium
labels:
  - bug
assignees: []
created_at: 2022-09-02T09:26:19Z
updated_at: 2022-09-02T12:46:40Z
url: https://github.com/astral-sh/ruff/issues/83
synced_at: 2026-01-12T15:54:40Z
```

# F401 `__future__.annotations` imported but unused

---

_@orsinium_

ruff marks `from __future__ import annotations` imports as unused. They are not supposed to be used, and so shouldn't be reported.

```
models.py:1:1: F401 `__future__.annotations` imported but unused
```

---

_Label `bug` added by @charliermarsh on 2022-09-02 12:43_

---

_Comment by @charliermarsh on 2022-09-02 12:46_

Thanks! Fixed in #85.

---

_Closed by @charliermarsh on 2022-09-02 12:46_

---
