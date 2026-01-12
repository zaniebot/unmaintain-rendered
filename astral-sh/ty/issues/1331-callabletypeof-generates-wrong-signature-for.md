```yaml
number: 1331
title: "`CallableTypeOf` generates wrong signature for custom callables"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - calls
assignees: []
created_at: 2025-10-10T08:21:33Z
updated_at: 2025-10-10T10:04:38Z
url: https://github.com/astral-sh/ty/issues/1331
synced_at: 2026-01-12T15:54:25Z
```

# `CallableTypeOf` generates wrong signature for custom callables

---

_@sharkdp_

Our `CallableTypeOf` type form creates `Callable` types with the wrong signature when used on custom `__call__`-able classes:

```py
from typing import reveal_type
from ty_extensions import CallableTypeOf

class MyCallable:
    def __call__(self, x: int) -> str:
        return "a"

my_callable = MyCallable()

def _(signature: CallableTypeOf[my_callable]):
    # This is wrong (should not contain `self`):
    reveal_type(signature)  # (self, x: int) -> str

    # This works fine, though:
    my_callable(1)
```
https://play.ty.dev/6b8d509b-4727-4695-b51b-41fd46813133


This sounds somewhat similar to https://github.com/astral-sh/ty/issues/1172, but is a distinct problem, I believe.

---

_Label `bug` added by @sharkdp on 2025-10-10 08:21_

---

_Label `calls` added by @sharkdp on 2025-10-10 08:21_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-10 08:25_

---

_Closed by @sharkdp on 2025-10-10 10:04_

---
