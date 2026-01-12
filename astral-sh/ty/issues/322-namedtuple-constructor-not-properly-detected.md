```yaml
number: 322
title: NamedTuple constructor not properly detected
type: issue
state: closed
author: eruvanos
labels:
  - bug
  - attribute access
assignees: []
created_at: 2025-05-11T21:35:12Z
updated_at: 2025-05-14T13:33:43Z
url: https://github.com/astral-sh/ty/issues/322
synced_at: 2026-01-12T15:54:22Z
```

# NamedTuple constructor not properly detected

---

_@eruvanos_

### Summary

Adding a `__get_attr__` function to a `NamedTuple` confuses the detection of allowed parameter in the constructor.

https://play.ty.dev/6acf1eb3-71e8-4af3-8bbc-8240febbde1b

```python
from __future__ import annotations
import typing as _typing


class Vec2(_typing.NamedTuple):

    x: float = 0.0
    y: float = 0.0

    def __getattr__(self, attrs: str):
        ...

Vec2(0.0, 0.0)
```

Error:
```
error[too-many-positional-arguments]: Too many positional arguments to bound method `__init__`: expected 0, got 2
  --> file.py:13:6
   |
11 |         ...
12 |
13 | Vec2(0.0, 0.0)
   |      ^^^
   |
info: `too-many-positional-arguments` is enabled by default
```



### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Label `bug` added by @sharkdp on 2025-05-12 09:24_

---

_Comment by @sharkdp on 2025-05-12 09:34_

Thank you for reporting this!

The problem here is that during construction, we try to see if the class has a `__init__` method (that's not derived from `object`). Apparently we think it has, as soon as `__getattr__` is defined. That's a bug; `__getattr__` can only be used for accessing attributes on instances, but dunder methods shouldn't be looked up on instances.

---

_Label `attribute access` added by @sharkdp on 2025-05-12 09:40_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-14 13:22_

---

_Closed by @sharkdp on 2025-05-14 13:33_

---
