---
number: 139
title: "`F401 imported but unused` for imports used in forward reference type hints"
type: issue
state: closed
author: nikolaik
labels:
  - bug
assignees: []
created_at: 2022-09-10T12:13:32Z
updated_at: 2022-09-10T18:51:44Z
url: https://github.com/astral-sh/ruff/issues/139
synced_at: 2026-01-07T13:12:14-06:00
---

# `F401 imported but unused` for imports used in forward reference type hints

---

_Issue opened by @nikolaik on 2022-09-10 12:13_

Similar to #123 but not quite.

Using the technique described in https://docs.python.org/3/library/typing.html#typing.TYPE_CHECKING

```python
if TYPE_CHECKING:
    from .models import Fruit. # F401 `models.Fruit` imported but unused 

def is_editable(fruit: 'Fruit'):
    fruit.is_editable()

---

_Label `bug` added by @charliermarsh on 2022-09-10 14:12_

---

_Comment by @charliermarsh on 2022-09-10 16:45_

This seems to work for me, do you know what version you're on? Is this the exact code snippet that trips an error?

---

_Comment by @nikolaik on 2022-09-10 17:34_

I tried reducing the example to a minimum, but I removed too much. Sorry about that. The actual issue seems to appear when [type aliases](https://docs.python.org/3/library/typing.html#type-aliases) references forward references.

```python
# ruffies.py:
from typing import Union, TYPE_CHECKING

if TYPE_CHECKING:
    from models import Fruit, Nut

Edibles = Union['Fruit', 'Nut']

def is_editable(fruit: Edibles):
    fruit.is_edible()
```
gives
```console
$ ruff ruffies.py
ruffies.py:4:5: F401 `models.Nut` imported but unused
ruffies.py:4:5: F401 `models.Fruit` imported but unused

Found 2 error(s).
```

---

_Comment by @charliermarsh on 2022-09-10 17:39_

Ah thanks, this makes sense!

---

_Comment by @charliermarsh on 2022-09-10 17:39_

The fix here is that we need to take into account `Union` and other typing structures.


---

_Referenced in [astral-sh/ruff#141](../../astral-sh/ruff/pulls/141.md) on 2022-09-10 18:32_

---

_Closed by @charliermarsh on 2022-09-10 18:51_

---
