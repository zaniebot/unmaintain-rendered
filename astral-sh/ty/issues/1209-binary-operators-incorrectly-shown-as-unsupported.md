```yaml
number: 1209
title: "Binary operators incorrectly shown as 'unsupported-operator' if defined via a factory with `Callable` return type"
type: issue
state: closed
author: wurli
labels: []
assignees: []
created_at: 2025-09-19T15:30:16Z
updated_at: 2025-10-14T12:27:55Z
url: https://github.com/astral-sh/ty/issues/1209
synced_at: 2026-01-10T02:06:25Z
```

# Binary operators incorrectly shown as 'unsupported-operator' if defined via a factory with `Callable` return type

---

_Issue opened by @wurli on 2025-09-19 15:30_

### Summary

I ran into this issue when working with [pyspark's `Col` type](https://github.com/apache/spark/blob/bb7846dd487f259994fdc69e18e03382e3f64f42/python/pyspark/sql/column.py#L277C1-L280C27) (`from pyspark.sql.functions import col`).

Here's a minimal reprex:

``` python
from typing import Callable

# Note: removing the return annotation 'fixes' the issue :)
def make_less_than() -> Callable[["MyClass", "MyClass"], bool]:
    def _(x: "MyClass", y: "MyClass") -> bool:
        return x.val < y.val
    return _

class MyClass:
    def __init__(self, val: int):
        self.val = val

    __lt__ = make_less_than()

MyClass(1) < MyClass(2) # Operator `<` is not supported for types `MyClass` and `MyClass` [unsupported-operator]
```

Playground link: https://play.ty.dev/d17232fa-2da8-4c04-baa7-45036bf59418

Many thanks! I've been playing around with ty on a pretty bit project and this is really the only issue I've found. Overall a fantastic experience for a project which is still in alpha ⭐ 

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Renamed from "Binary operators incorrectly shown as 'not supported' if defined via a factory with `Callable` return type" to "Binary operators incorrectly shown as 'unsupported-operator' if defined via a factory with `Callable` return type" by @wurli on 2025-09-19 15:32_

---

_Comment by @carljm on 2025-09-19 15:47_

Thanks for the report! I think this is https://github.com/astral-sh/ty/issues/491

---

_Closed by @carljm on 2025-09-19 15:47_

---

_Comment by @wurli on 2025-09-19 15:50_

Aha! Thank you, yes I think so :)

---

_Closed by @sharkdp on 2025-10-14 12:27_

---
