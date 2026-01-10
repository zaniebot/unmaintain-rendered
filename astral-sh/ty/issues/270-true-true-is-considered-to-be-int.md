```yaml
number: 270
title: "`True & True` is considered to be `int`"
type: issue
state: closed
author: lancelote
labels:
  - bug
assignees: []
created_at: 2025-05-08T12:42:55Z
updated_at: 2025-05-08T13:16:07Z
url: https://github.com/astral-sh/ty/issues/270
synced_at: 2026-01-10T02:34:09Z
```

# `True & True` is considered to be `int`

---

_Issue opened by @lancelote on 2025-05-08 12:42_

### Summary

given ...

```python
from typing import reveal_type

reveal_type(True & True)
```

`ty` reports ...

```
% ty check sample.py 
info: revealed-type: Revealed type
 --> sample.py:3:1
  |
1 | from typing import reveal_type
2 |
3 | reveal_type(True & True)
  | ^^^^^^^^^^^^^^^^^^^^^^^^ `int`
  |

Found 1 diagnostic
```

### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Renamed from "`True & True` considered to be `int`" to "`True & True` is considered to be `int`" by @lancelote on 2025-05-08 12:43_

---

_Label `bug` added by @AlexWaygood on 2025-05-08 12:45_

---

_Comment by @AlexWaygood on 2025-05-08 12:49_

Amusingly, we give a worse result here for the literal `True` constants than we do if all the information you tell us is that there are two instances of `bool`:

```py
from typing import reveal_type

reveal_type(True & True)  # revealed: bool

def f(x: bool, y: bool):
    reveal_type(x & y)  # revealed: int
```

https://play.ty.dev/1cfad6de-0386-4733-84f9-df9c1c7e7327

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-08 12:49_

---

_Comment by @lancelote on 2025-05-08 12:56_

My "real world" example was something like that ...

```python
from typing import Any, reveal_type


class SomeClass:
    def some_func(self, param: Any) -> None:
        flag = True

        ...

        flag &= param is self
        reveal_type(flag)  # `ty` says `int`
```

---

_Closed by @AlexWaygood on 2025-05-08 13:10_

---

_Comment by @lancelote on 2025-05-08 13:16_

@AlexWaygood Thank you for an instant fix 👍

---
