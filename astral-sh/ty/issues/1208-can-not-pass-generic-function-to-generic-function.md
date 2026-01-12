```yaml
number: 1208
title: Can not pass generic function to generic function
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-09-19T10:52:18Z
updated_at: 2025-09-23T12:02:27Z
url: https://github.com/astral-sh/ty/issues/1208
synced_at: 2026-01-12T15:54:24Z
```

# Can not pass generic function to generic function

---

_@sharkdp_

The following example …
```py
from typing import Callable

def invoke[A, B](fn: Callable[[A], B], value: A) -> B:
    return fn(value)

def identity[T](x: T) -> T:
    return x

invoke(identity, 1)
```
(https://play.ty.dev/6dfb9951-b78f-4791-af82-151a1c694a7c)

… currently fails with this error:
```
error[invalid-argument-type]: Argument to function `invoke` is incorrect
  --> /home/shark/playground/test.py:12:8
   |
12 | invoke(identity, 1)
   |        ^^^^^^^^ Expected `(Literal[1], /) -> Unknown`, found `def identity[T](x: T@identity) -> T@identity`
   |
info: Function defined here
 --> /home/shark/playground/test.py:4:5
  |
4 | def invoke[A, B](fn: Callable[[A], B], value: A) -> B:
  |     ^^^^^^       -------------------- Parameter declared here
5 |     return fn(value)
  |
info: rule `invalid-argument-type` is enabled by default
```

I am reporting this in addition to #623, because it seems like this is not really related to the shortcomings of the current solver, but rather to the way we handle assignability of the generic `identity` function argument to the call(?).

---

_Label `bug` added by @sharkdp on 2025-09-19 10:52_

---

_Label `generics` added by @sharkdp on 2025-09-19 10:52_

---

_Assigned to @sharkdp by @sharkdp on 2025-09-19 11:41_

---

_Comment by @carljm on 2025-09-19 15:54_

Is this the same as https://github.com/astral-sh/ty/issues/95 ?

---

_Comment by @sharkdp on 2025-09-22 08:16_

Yes, very much looks like it. I'll copy over my example to make sure it gets resolved when that issue is fixed.

---

_Closed by @sharkdp on 2025-09-22 08:16_

---

_Reopened by @sharkdp on 2025-09-22 14:57_

---

_Comment by @sharkdp on 2025-09-22 14:57_

This is fixable independently from #95, so I'm reopening it for now.

---

_Closed by @sharkdp on 2025-09-23 12:02_

---
