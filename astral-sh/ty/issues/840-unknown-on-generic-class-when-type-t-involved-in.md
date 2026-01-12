```yaml
number: 840
title: "Unknown on Generic class when type[T] involved in signature"
type: issue
state: closed
author: y2kbugger
labels:
  - generics
assignees: []
created_at: 2025-07-17T13:02:01Z
updated_at: 2025-07-18T19:17:45Z
url: https://github.com/astral-sh/ty/issues/840
synced_at: 2026-01-12T15:54:24Z
```

# Unknown on Generic class when type[T] involved in signature

---

_@y2kbugger_

### Summary

https://play.ty.dev/e8003948-f463-42e4-88cc-e18436126f23

Pyright is able to get this correctly.
see also possible related a bug I had in pyright in the past: https://github.com/microsoft/pyright/issues/10207

```python
from typing import assert_type

class Row:
    pass

class Proxy[R: Row]:
    def __init__(self, Model: type[R]): # works when taking concrete object instead of type[R]
        self.Model = Model

    def fetch(self) -> R:
        return self.Model()

myproxy = Proxy(Row) # Proxy[Unknown] instead of Proxy[R]
myrow = myproxy.fetch() # Unknown instead of Row

assert_type(myrows[0], Row)
```

### Version

_No response_

---

_Label `generics` added by @sharkdp on 2025-07-18 07:15_

---

_Comment by @carljm on 2025-07-18 19:17_

Thanks! This is the same as #501 

---

_Closed by @carljm on 2025-07-18 19:17_

---
