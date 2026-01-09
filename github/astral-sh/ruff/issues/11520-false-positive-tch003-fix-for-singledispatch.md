---
number: 11520
title: False positive TCH003 fix for singledispatch functions with type annotations.
type: issue
state: closed
author: wence-
labels:
  - bug
assignees: []
created_at: 2024-05-23T19:13:52Z
updated_at: 2024-05-24T08:27:41Z
url: https://github.com/astral-sh/ruff/issues/11520
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive TCH003 fix for singledispatch functions with type annotations.

---

_Issue opened by @wence- on 2024-05-23 19:13_

Consider:

```python
from __future__ import annotations

from functools import singledispatch
from typing import Any

from collections.abc import MutableMapping


@singledispatch
def foo(x: Any, y: MutableMapping) -> int:
    raise NotImplementedError


@foo.register
def _(x: int, y: MutableMapping) -> int:
    return x
```

With the following ruff configuration
```toml
[tool.ruff]
target-version = "py39"

[tool.ruff.lint]
select = ["TCH"]

[tool.ruff.lint.flake8-type-checking]
strict = true
```

ruff suggests that (TCH003) `MutableMapping` be moved into a `TYPE_CHECKING` block, however, this is an invalid fix since the singledispatch registration needs access to the types at runtime.

```
$ python --version
Python 3.10.14
$ ruff --version
0.4.5
$ python bug.py # no issues
$ ruff check bug.py
bug.py:6:29: TCH003 Move standard library import `collections.abc.MutableMapping` into a type-checking block
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$ ruff check --unsafe-fixes bug.py
Found 1 error (1 fixed, 0 remaining).
$ python bug.py
Traceback (most recent call last):
  File "/home/coder/doodles/python/ruff-check/bug.py", line 17, in <module>
    def _(x: int, y: MutableMapping) -> int:
  File "/home/coder/.conda/envs/rapids/lib/python3.10/functools.py", line 871, in register
    argname, cls = next(iter(get_type_hints(func).items()))
  File "/home/coder/.conda/envs/rapids/lib/python3.10/typing.py", line 1871, in get_type_hints
    value = _eval_type(value, globalns, localns)
  File "/home/coder/.conda/envs/rapids/lib/python3.10/typing.py", line 327, in _eval_type
    return t._evaluate(globalns, localns, recursive_guard)
  File "/home/coder/.conda/envs/rapids/lib/python3.10/typing.py", line 694, in _evaluate
    eval(self.__forward_code__, globalns, localns),
  File "<string>", line 1, in <module>
NameError: name 'MutableMapping' is not defined
```

---

_Comment by @charliermarsh on 2024-05-23 19:16_

Can't argue with that trace, but I thought `singledispatch` only applied to the type of the first argument...? https://docs.python.org/3/library/functools.html#functools.singledispatch

Is it an implementation detail, that it needs to evaluate the entire signature to get any of the types?

---

_Comment by @wence- on 2024-05-23 21:18_

> Can't argue with that trace, but I thought `singledispatch` only applied to the type of the first argument...? https://docs.python.org/3/library/functools.html#functools.singledispatch

Yes, this is right.
 
> Is it an implementation detail, that it needs to evaluate the entire signature to get any of the types?

It appears to be, here's a [feature request](https://github.com/python/cpython/issues/110373) to only look at the annotation of the first argument in cpython.

It turns out this implementation has a more [nefarious bug](https://github.com/python/cpython/issues/84644) (which ruff _might_ be able to flag?), which is that the code:

```
@foo.register
def _(a, b: str): # Note first argument is not annotated
    ...
```

registers a dispatch for `str`, even though this clearly wasn't the intention.


---

_Renamed from "False positive TCH002 fix for singledispatch functions with type annotations." to "False positive TCH003 fix for singledispatch functions with type annotations." by @wence- on 2024-05-23 21:21_

---

_Label `bug` added by @charliermarsh on 2024-05-23 21:21_

---

_Comment by @charliermarsh on 2024-05-23 21:21_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-24 00:01_

---

_Referenced in [astral-sh/ruff#11523](../../astral-sh/ruff/pulls/11523.md) on 2024-05-24 00:22_

---

_Closed by @charliermarsh on 2024-05-24 00:36_

---

_Comment by @wence- on 2024-05-24 08:27_

Thanks!

---

_Referenced in [astral-sh/ruff#13924](../../astral-sh/ruff/issues/13924.md) on 2024-10-25 11:41_

---

_Referenced in [astral-sh/ruff#13955](../../astral-sh/ruff/issues/13955.md) on 2024-10-28 02:58_

---
