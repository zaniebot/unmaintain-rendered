---
number: 849
title: Isort sorting order
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2022-11-21T10:47:14Z
updated_at: 2022-11-21T18:30:25Z
url: https://github.com/astral-sh/ruff/issues/849
synced_at: 2026-01-07T13:12:14-06:00
---

# Isort sorting order

---

_Issue opened by @JonathanPlasse on 2022-11-21 10:47_

`isort` sorting order is

```python
from ..a import a
from ..b import a
from .a import a
from .b import a
```

`ruff` sorting order is

```python
from .a import a
from ..a import a
from .b import a
from ..b import a
```

It would be nice that the behavior of `ruff` matched `isort`.

---

_Label `bug` added by @charliermarsh on 2022-11-21 14:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-21 17:53_

---

_Referenced in [astral-sh/ruff#856](../../astral-sh/ruff/pulls/856.md) on 2022-11-21 18:11_

---

_Closed by @charliermarsh on 2022-11-21 18:30_

---

_Referenced in [Mic92/nix-update#110](../../Mic92/nix-update/pulls/110.md) on 2022-11-21 20:58_

---
