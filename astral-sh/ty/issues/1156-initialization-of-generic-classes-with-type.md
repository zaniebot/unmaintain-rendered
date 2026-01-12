```yaml
number: 1156
title: Initialization of generic classes with type parameter default values is broken
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-09T14:10:32Z
updated_at: 2025-09-10T15:00:30Z
url: https://github.com/astral-sh/ty/issues/1156
synced_at: 2026-01-12T15:54:24Z
```

# Initialization of generic classes with type parameter default values is broken

---

_@sharkdp_

### Summary

I identified another problem that "blocks" the merge of https://github.com/astral-sh/ruff/pull/18007 (in the sense that there are hundreds of false positives caused by this). It can be reproduced on `main` by explicitly annotating `self: Self`, and occurs when trying to initialize a generic class with a type parameter default value:

```py
from typing import Self

class MyClass[T = int]:
    def __init__(self: Self, x: T) -> None:
        self.x = x

# error: Argument to bound method `__init__` is incorrect: Argument type `MyClass[T@MyClass]` does not satisfy upper bound of type variable `Self`
MyClass(1)
```
https://play.ty.dev/adc72fc1-667d-4956-aa06-ed72e1403e1f

### Version

Current main (25853e237)

---

_Label `bug` added by @sharkdp on 2025-09-09 14:10_

---

_Label `generics` added by @sharkdp on 2025-09-09 14:10_

---

_Comment by @dcreager on 2025-09-09 14:23_

It looks like we're using the typevar default instead of the type inferred by the arguments:

```py
from typing import Self

class MyClass[T = str]:
    def __init__(self: Self, x: T) -> None:
        self.x = x

reveal_type(MyClass(1))  # revealed: MyClass[str]
```

https://play.ty.dev/1f512f4b-b072-4844-b2dc-314b9b1b2598

---

_Assigned to @sharkdp by @sharkdp on 2025-09-09 14:54_

---

_Added to milestone `Beta` by @carljm on 2025-09-09 16:44_

---

_Closed by @sharkdp on 2025-09-10 15:00_

---
