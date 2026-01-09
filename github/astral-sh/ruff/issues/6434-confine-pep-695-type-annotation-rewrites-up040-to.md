---
number: 6434
title: "Confine PEP 695 type annotation rewrites (`UP040`) to non-runtime contexts"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
  - python312
assignees: []
created_at: 2023-08-09T02:10:28Z
updated_at: 2023-10-06T19:56:41Z
url: https://github.com/astral-sh/ruff/issues/6434
synced_at: 2026-01-07T13:12:15-06:00
---

# Confine PEP 695 type annotation rewrites (`UP040`) to non-runtime contexts

---

_Issue opened by @charliermarsh on 2023-08-09 02:10_

See the comment here: https://github.com/astral-sh/ruff/pull/6289#issuecomment-1670239247. PEP 695 type aliases can't be used for runtime typing, so we need to limit the cases in which we flag and rewrite them. For starters, we could consider limiting to `.pyi` files, where it should ~always be safe. We could then extend to "private" type aliases that aren't used in runtime contexts within a file.

Alternatively, we could just turn off this rule when [`keep-runtime-typing`](https://beta.ruff.rs/docs/settings/#pyupgrade-keep-runtime-typing) is set, and leave it as-is.


---

_Comment by @charliermarsh on 2023-08-09 02:10_

\cc @zanieb 

---

_Label `rule` added by @charliermarsh on 2023-08-09 02:10_

---

_Referenced in [astral-sh/ruff#6289](../../astral-sh/ruff/pulls/6289.md) on 2023-08-09 02:10_

---

_Comment by @zanieb on 2023-08-09 03:05_

Thank for raising Henry!

Here are some notes on validity after some experimentation:

```python
import typing

# valid
x: typing.TypeAlias = int
print(isinstance(1, x))

# valid
x: typing.TypeAlias = int | str
print(isinstance(1, x))

# valid
x: typing.TypeAlias = typing.Union[int, str]
print(isinstance(1, x))

# not valid
x: typing.TypeAlias = list[int]
print(isinstance([1,2], x))

# not valid
x: typing.TypeAlias = typing.list[int]
print(isinstance([1,2], x))

# valid
from pydantic import BaseModel
type x = int
class Foo(BaseModel):
    y: x
f = Foo(y=1)
print(f.y)

# not valid
type x = int | str
print(isinstance(1, x))

# not valid
type x = typing.Union[int, str]
print(isinstance(1, x))
```

It may be enough to categorize this as a "Suggested" fix.

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---

_Referenced in [astral-sh/ruff#7769](../../astral-sh/ruff/pulls/7769.md) on 2023-10-03 17:37_

---

_Referenced in [astral-sh/ruff#7836](../../astral-sh/ruff/pulls/7836.md) on 2023-10-06 15:54_

---

_Closed by @zanieb on 2023-10-06 19:56_

---
