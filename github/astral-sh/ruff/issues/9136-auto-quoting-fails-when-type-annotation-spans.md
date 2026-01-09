---
number: 9136
title: Auto-quoting fails when type annotation spans multiple lines
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-14T18:13:35Z
updated_at: 2023-12-15T01:03:11Z
url: https://github.com/astral-sh/ruff/issues/9136
synced_at: 2026-01-07T13:12:15-06:00
---

# Auto-quoting fails when type annotation spans multiple lines

---

_Issue opened by @charliermarsh on 2023-12-14 18:13_

Given:

```python
from django.contrib.auth.models import AbstractBaseUser

def foo(
    self, user: AbstractBaseUser[
        int
    ]
):
    pass
```

Running `cargo run -p ruff_cli -- check foo.py --select TCH --fix -n --unsafe-fixes` fails with:

```python
error: Fix introduced a syntax error in `foo.py` with rule codes TCH002: EOL while scanning string literal at byte offset 156
---
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from django.contrib.auth.models import AbstractBaseUser

def foo(
    self, user: 'AbstractBaseUser[
        int
    ]', view: 'type[CondorBaseViewSet]'
):
    pass

---
```


---

_Label `bug` added by @charliermarsh on 2023-12-14 18:15_

---

_Referenced in [astral-sh/ruff#9131](../../astral-sh/ruff/issues/9131.md) on 2023-12-14 18:15_

---

_Referenced in [astral-sh/ruff#9139](../../astral-sh/ruff/issues/9139.md) on 2023-12-14 19:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-14 19:41_

---

_Referenced in [astral-sh/ruff#9142](../../astral-sh/ruff/pulls/9142.md) on 2023-12-15 00:57_

---

_Closed by @charliermarsh on 2023-12-15 01:03_

---
