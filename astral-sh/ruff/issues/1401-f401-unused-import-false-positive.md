---
number: 1401
title: F401 (unused import) false positive
type: issue
state: closed
author: jad-haddad
labels:
  - bug
assignees: []
created_at: 2022-12-27T11:45:58Z
updated_at: 2023-05-24T15:12:11Z
url: https://github.com/astral-sh/ruff/issues/1401
synced_at: 2026-01-10T01:22:39Z
---

# F401 (unused import) false positive

---

_Issue opened by @jad-haddad on 2022-12-27 11:45_

Hello, I have an import that is used in typing and is getting removed by ruff (flake8 detects it as unused as well) here is the example:

```python
from typing import Optional
import datetime # <--- gets removed by ruff
import pydantic

class SomeClass(pydantic.BaseModel):
    datetime: Optional[datetime.datetime]
```
for now, i'm adding the comment `# noqa: F401` to keep the import.

ruff version 0.0.195

Thank you for this project 🤩 

---

_Comment by @charliermarsh on 2022-12-27 12:46_

Thank you! Will check this out. 

---

_Label `bug` added by @charliermarsh on 2022-12-27 12:46_

---

_Comment by @charliermarsh on 2022-12-27 16:12_

Is this the exact snippet you're using? When I run Ruff on:

```py
from typing import Optional
import datetime
import pydantic

class SomeClass(pydantic.BaseModel):
    datetime: Optional[datetime.datetime]
```

It doesn't report any errors 🤔 Neither does `flake8`.

---

_Label `bug` removed by @charliermarsh on 2022-12-27 16:12_

---

_Label `question` added by @charliermarsh on 2022-12-27 16:12_

---

_Comment by @charliermarsh on 2022-12-27 16:12_

(I'm guessing that whatever the problem is, it may have to do with the use of `datetime` as the class property, which collides with the import name `datetime`.)

---

_Comment by @jad-haddad on 2022-12-27 17:03_

well, there are other classes definitions in the module, but none of them have the property `datetime` nor the type `datetime` the full list of imports is
```python
from __future__ import annotations
import datetime  # noqa: F401 (false positive)
from enum import Enum
from typing import List, Optional

import pydantic
```
could it be that the `__future__` annotations module is interfering with ruff/flake8 ?
I'm using python 3.8 and I have set in ruff.toml `target-version = "py38"`

---

_Comment by @charliermarsh on 2022-12-27 17:04_

Oh, yeah, it could be `__future__`.

---

_Comment by @charliermarsh on 2022-12-27 17:07_

Here's an minimal reproducible example:

```py
from __future__ import annotations

import datetime
from typing import Optional

class SomeClass:
    datetime: Optional[datetime.datetime]
```

---

_Comment by @charliermarsh on 2022-12-27 17:16_

A little challenging to fix, won't happen immediately... My suggestion would be to use a different name for the class attribute, since these kinds of collisions can cause a lot of confusing behavior. For example, Mypy and Pyright handle this case differently, and I don't know which is correct:

```py
from __future__ import annotations

import datetime
from typing import Optional

class SomeClass:
    datetime: Optional[datetime.datetime]

    def __init__(self, datetime: datetime.datetime):
        self.datetime = datetime

SomeClass(datetime.datetime(...))
```

Mypy flags an error on line 9 (`error: Name "datetime.datetime" is not defined`), because `datetime` there is bound to the class property, not the import. But Pyright does not.


---

_Label `question` removed by @charliermarsh on 2022-12-27 17:16_

---

_Label `bug` added by @charliermarsh on 2022-12-27 17:16_

---

_Comment by @rassie on 2023-01-01 13:54_

Is this more or less an extended version of the "A003 problem" where A003 is triggered in the following code because `id` apparently shadows a Python built-in? 

```python
class SomeClass:
    id: int
```

Basically, `datetime` (or in my case `uuid`) is shadowing the import, so that the import becomes unusued. I wouldn't normally expect the second `datetime` in the field declaration `datetime: Optional[datetime.datetime]` to refer to the first one, would be a major gotcha for me, does this shadowing really happen? 

---

_Comment by @andersk on 2023-01-03 04:45_

An annotation like `id: int` or `datetime: Optional[datetime.datetime]` doesn’t bind anything at runtime, so it shouldn’t be considered as shadowing the outer binding.

An annotated _assignment_ like `id: int = 0` or `datetime: Optional[datetime.datetime] = datetime.datetime.now()`, however, does shadow from the perspective of some references:

```python
class SomeClass:
    #         ↓ shadowed except under __future__.annotations
    datetime: datetime.datetime = (
        # ↓ not shadowed
        datetime.datetime.now()
    )
    #      ↓ shadowed except under __future__.annotations
    other: datetime.datetime = (
        # ↓ always shadowed
        datetime
    )
```

(The behavior for annotations can be observed at runtime with [`typing.get_type_hints(SomeClass)`](https://docs.python.org/3/library/typing.html#typing.get_type_hints).)

Mypy’s understanding of annotation shadowing seems to be quite busted. It accepts nonsense like

```python
x = "oops"

class SomeClass:
    x: int
    print(x + 1)
```

which does `str + int` at runtime.

---

_Referenced in [astral-sh/ruff#2248](../../astral-sh/ruff/issues/2248.md) on 2023-01-27 13:24_

---

_Referenced in [astral-sh/ruff#2359](../../astral-sh/ruff/issues/2359.md) on 2023-01-30 20:03_

---

_Referenced in [astral-sh/ruff#3014](../../astral-sh/ruff/issues/3014.md) on 2023-02-18 18:06_

---

_Referenced in [astral-sh/ruff#4044](../../astral-sh/ruff/issues/4044.md) on 2023-04-21 01:37_

---

_Comment by @jack-mcivor on 2023-05-04 13:05_

Came across this today - if it helps here is where the `from __future__ import annotations` issue was raised in `pyflakes`

- https://github.com/PyCQA/pyflakes/issues/648
- https://github.com/PyCQA/pyflakes/issues/713
- https://github.com/PyCQA/pyflakes/issues/773

---

_Comment by @charliermarsh on 2023-05-18 19:35_

I've spent a while looking into this. What we might need is this: https://github.com/microsoft/pyright/commit/27bf8c1f2.

---

_Referenced in [astral-sh/ruff#4509](../../astral-sh/ruff/pulls/4509.md) on 2023-05-18 20:41_

---

_Closed by @charliermarsh on 2023-05-20 02:54_

---

_Comment by @jad-haddad on 2023-05-24 15:08_

I still have the issue @charliermarsh:
```python
from __future__ import annotations

import datetime  # <--- gets removed by ruff

import pydantic


class SomeClass(pydantic.BaseModel):
    datetime: datetime.datetime

```

command: `ruff --config ruff.toml --show-fixes --fix ./test.py`
```
Fixed 2 errors:
- test.py:
    1 × F401 (unused-import)
    1 × I001 (unsorted-imports)
```

Ruff 0.0.269
Python 3.11.2

---

_Comment by @charliermarsh on 2023-05-24 15:09_

It hasn't been released. It'll be part of 0.0.270.

---

_Comment by @jad-haddad on 2023-05-24 15:11_

oh okay my bad, i'll wait for the next release then, thanks :) 

---

_Comment by @charliermarsh on 2023-05-24 15:12_

Should be out soon, maybe today? We'll see! :)

---
