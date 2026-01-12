```yaml
number: 3215
title: "Ruff rewrites `Union[tuple_of_types]` in potentially breaking ways"
type: issue
state: closed
author: rouge8
labels:
  - bug
assignees: []
created_at: 2023-02-24T21:21:26Z
updated_at: 2023-02-24T22:40:01Z
url: https://github.com/astral-sh/ruff/issues/3215
synced_at: 2026-01-12T15:54:43Z
```

# Ruff rewrites `Union[tuple_of_types]` in potentially breaking ways

---

_@rouge8_

After Ruff rewrites this file, Python starts throwing an error.

## `t.py`

```python
from typing import Union, cast


def _make_optional(cls: type) -> type:
    """Convert a type to an Optional type."""
    return cast(type, cls | None)


new_types = (int, float)

_make_optional(cast(type, Union[new_types]))
```

## output

```
❯ ruff --isolated --fix --target-version py311 --select UP t.py
Found 1 error (1 fixed, 0 remaining).
```

## rewritten

```python
from typing import Union, cast


def _make_optional(cls: type) -> type:
    """Convert a type to an Optional type."""
    return cast(type, cls | None)


new_types = (int, float)

_make_optional(cast(type, new_types))
```

Unfortunately, `t.py` now errors out:

```
Traceback (most recent call last):
  File "/Users/andyfreeland/t.py", line 11, in <module>
    _make_optional(cast(type, new_types))
  File "/Users/andyfreeland/t.py", line 6, in _make_optional
    return cast(type, cls | None)
                      ~~~~^~~~~~
TypeError: unsupported operand type(s) for |: 'tuple' and 'NoneType'
```

## version

```
ruff 0.0.252
```

---

_Comment by @rouge8 on 2023-02-24 21:25_

FWIW, `pyupgrade --py311-plus t.py` does not make any changes to the original `t.py`.

---

_Comment by @charliermarsh on 2023-02-24 21:26_

Yeah `pyupgrade` never applies PEP 604 transformations to runtime types. Maybe we shouldn't either. It's come up before.

---

_Label `bug` added by @charliermarsh on 2023-02-24 21:26_

---

_Comment by @charliermarsh on 2023-02-24 21:26_

This was another recent case: #2981.

---

_Comment by @charliermarsh on 2023-02-24 21:28_

(So let's just change it.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-24 22:10_

---

_Closed by @charliermarsh on 2023-02-24 22:40_

---

_Closed by @charliermarsh on 2023-02-24 22:40_

---
