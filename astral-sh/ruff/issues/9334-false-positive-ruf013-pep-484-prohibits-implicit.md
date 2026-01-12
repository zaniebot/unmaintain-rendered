```yaml
number: 9334
title: "False positive RUF013 PEP 484 prohibits implicit `Optional`"
type: issue
state: closed
author: randolf-scholz
labels:
  - bug
assignees: []
created_at: 2023-12-31T16:37:46Z
updated_at: 2023-12-31T18:23:34Z
url: https://github.com/astral-sh/ruff/issues/9334
synced_at: 2026-01-12T15:54:49Z
```

# False positive RUF013 PEP 484 prohibits implicit `Optional`

---

_@randolf-scholz_

The bug occurs if `Optional` is imported from `typing_extensions` instead of `typing`.

```python
from typing_extensions import Optional

def foo(x: Optional[int] = None):
     pass
```

https://play.ruff.rs/dda2f645-171a-4666-93c6-d686c3f7edfa

---

_Label `bug` added by @charliermarsh on 2023-12-31 16:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-31 16:44_

---

_Comment by @charliermarsh on 2023-12-31 16:47_

Fixed in https://github.com/astral-sh/ruff/pull/9335, thanks.

---

_Closed by @randolf-scholz on 2023-12-31 17:37_

---

_Reopened by @randolf-scholz on 2023-12-31 17:37_

---

_Closed by @charliermarsh on 2023-12-31 18:23_

---
