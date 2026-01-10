---
number: 12736
title: "N805 False positive for `class Meta(type(base))`"
type: issue
state: closed
author: randolf-scholz
labels:
  - bug
assignees: []
created_at: 2024-08-07T17:28:29Z
updated_at: 2024-08-09T20:10:14Z
url: https://github.com/astral-sh/ruff/issues/12736
synced_at: 2026-01-10T01:22:52Z
---

# N805 False positive for `class Meta(type(base))`

---

_Issue opened by @randolf-scholz on 2024-08-07 17:28_

```python
from typing import Protocol

class MyMeta(type):
    def __subclasscheck__(cls, other): ...  # ✅

class MyProtocolMeta(type(Protocol)):
    def __subclasscheck__(cls, other): ...  # ❌
```

https://play.ruff.rs/5573b0e2-fba5-47c1-bc69-a7e780324a86

---

_Comment by @randolf-scholz on 2024-08-08 08:13_

Problem is with this function, which does not correctly recognizes metaclass in this case:

https://github.com/astral-sh/ruff/blob/df7345e118f456e43b04aefbbaaa253c16b62329/crates/ruff_python_semantic/src/analyze/class.rs#L115-L122

---

_Label `bug` added by @charliermarsh on 2024-08-09 01:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 01:18_

---

_Referenced in [astral-sh/ruff#12770](../../astral-sh/ruff/pulls/12770.md) on 2024-08-09 02:04_

---

_Closed by @charliermarsh on 2024-08-09 20:10_

---

_Closed by @charliermarsh on 2024-08-09 20:10_

---

_Referenced in [astral-sh/ruff#14675](../../astral-sh/ruff/issues/14675.md) on 2024-11-29 11:44_

---
