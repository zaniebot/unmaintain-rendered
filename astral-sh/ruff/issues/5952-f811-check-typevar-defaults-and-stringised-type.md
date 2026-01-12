```yaml
number: 5952
title: "F811: Check TypeVar defaults and stringised type parameters in bases"
type: issue
state: closed
author: Gobot1234
labels:
  - bug
assignees: []
created_at: 2023-07-21T16:40:52Z
updated_at: 2025-04-23T15:47:38Z
url: https://github.com/astral-sh/ruff/issues/5952
synced_at: 2026-01-12T15:54:45Z
```

# F811: Check TypeVar defaults and stringised type parameters in bases

---

_@Gobot1234_

I have code that looks like
```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
	import some_circular_mod

def bar():
	import some_circular_mod  # F811 Redefinition of unused `some_circular_mod` from line 4
```
I don't think an error should be reported in this case and ruff should check if the module is imported in TYPE_CHECKING before emitting the error.

---

_Comment by @charliermarsh on 2023-07-21 16:44_

Is `some_circular_mod` being used in type annotations elsewhere?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-21 16:44_

---

_Comment by @Gobot1234 on 2023-07-21 16:45_

Yeah, sorry missed that from the example

---

_Comment by @charliermarsh on 2023-07-21 16:57_

Do you mind updating the example to include a usage? For example, this _doesn't_ raise F811, which seems correct:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import some_circular_mod

def bar():
    import some_circular_mod

def foo():
    x: some_circular_mod.A = 1
```

---

_Comment by @Gobot1234 on 2023-07-21 17:08_

Sorry again, didn't take the time to check, 
```py
from typing import TYPE_CHECKING
from typing_extensions import TypeVar

if TYPE_CHECKING:
    import some_circular_mod

T = TypeVar("T", default="some_circular_mod.X")  # problematically not checked


def bar():
    import some_circular_mod

class Foo(Generic[T]): pass
class Bar(Foo["some_circular_mod.Y"]): pass  # problematically not checked
```

---

_Comment by @charliermarsh on 2023-07-21 17:10_

Thanks!

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-07-21 17:10_

---

_Label `bug` added by @charliermarsh on 2023-07-21 17:10_

---

_Renamed from "[Feature] F811: Check if import is inside of TYPE_CHECKING block" to "[Feature] F811: Check TypeVar defaults and stringised type parameters in bases" by @Gobot1234 on 2023-07-21 17:24_

---

_Renamed from "[Feature] F811: Check TypeVar defaults and stringised type parameters in bases" to "F811: Check TypeVar defaults and stringised type parameters in bases" by @Gobot1234 on 2023-07-21 17:25_

---

_Comment by @ntBre on 2025-04-23 15:47_

I believe this was fixed in 0.1.12.

---

_Closed by @ntBre on 2025-04-23 15:47_

---
