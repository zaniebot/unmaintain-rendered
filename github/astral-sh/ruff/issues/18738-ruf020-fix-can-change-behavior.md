---
number: 18738
title: RUF020 fix can change behavior
type: issue
state: open
author: MeGaGiGaGon
labels:
  - fixes
  - type-inference
assignees: []
created_at: 2025-06-18T00:30:58Z
updated_at: 2025-06-18T19:04:11Z
url: https://github.com/astral-sh/ruff/issues/18738
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF020 fix can change behavior

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 00:30_

### Summary

These are both very contrived, but theoretically fixing RUF020 can change program behavior [playground](https://play.ruff.rs/4421f539-dabb-44ba-a67b-da81b932b096)
```py
from typing import Never, NoReturn
class A:
    def __or__(self, other): print(other)
x = A() | Never
y: A() | NoReturn
```
Gets fixed to
```py
from typing import Never, NoReturn
class A:
    def __or__(self, other): print(other)
x = A()
y: A()
```

```pycon
>>> from typing import Never, NoReturn
... class A:
...     def __or__(self, other): print(other)
... x = A() | Never
... y: A() | NoReturn
...
typing.Never
typing.NoReturn
>>> from typing import Never, NoReturn
... class A:
...     def __or__(self, other): print(other)
... x = A()
... y: A()
...
>>>
```
The first one is very constructed since I don't know how you would end up with a literal `{never_like}` outside a type annotation along with a `__or__`. The second one not as much, since at least without `__future__` `__annotations__` you could have a `__or__` that does something with a `{never_like}` type.

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Comment by @ntBre on 2025-06-18 03:12_

Thanks for these reports!

I'm not as sure about this one as the others. I don't think we have a general way of checking for this kind of custom `__or__` behavior, and I'm not sure it's worth marking the fix as always unsafe. These are probably pretty rare situations, as you said.

---

_Label `fixes` added by @ntBre on 2025-06-18 03:12_

---

_Label `type-inference` added by @ntBre on 2025-06-18 03:12_

---

_Comment by @MeGaGiGaGon on 2025-06-18 05:30_

That's reasonable, I mostly opened this so there would be a record of the decision. It might also be worth adding something to the docs about how this could happen, unsure.

---
