```yaml
number: 1423
title: Performance regression in ty==0.0.1a23
type: issue
state: closed
author: danielhollas
labels:
  - performance
assignees: []
created_at: 2025-10-23T16:43:15Z
updated_at: 2025-12-10T00:51:34Z
url: https://github.com/astral-sh/ty/issues/1423
synced_at: 2026-01-10T01:56:40Z
```

# Performance regression in ty==0.0.1a23

---

_Issue opened by @danielhollas on 2025-10-23 16:43_

### Summary

I noticed that since version `0.0.1-alpha.23`  `ty` was noticeably slower on our codebase,
and was ultimately able to narrow it down to the following MRE, which takes over 3s to type check on my machine. (it is instant with 0.0.1-alpha.22)

```python
from typing import Self

class A:
    def __init__(self: Self):
        self.a = 0
        self.b = 0

    def foo(self: Self):
        if self.a:
            self.b = self.b + 1

    def bar(self: Self):
        if str(self.b):
            self.a = self.b
```

Couple interesting observations:

1. the call to `str` in `bar` is needed to reproduce the problem
2. if I change `self.b = self.b + 1` to `self.b += 1` I don't see a problem

### Version

0.0.1-alpha.23

(I originally noticed this in 0.0.1-alpha.24, which introduced implicit type of self)

---

_Label `performance` added by @AlexWaygood on 2025-10-23 16:45_

---

_Comment by @danielhollas on 2025-10-23 16:45_

Actually, the performance regression is there on `0.0.1-alpha.23` is I explicitly type self with Self.

EDIT: I don't see the regression with `0.0.1-alpha.22`

---

_Renamed from "Performace regression in ty==0.0.1a24" to "Performace regression in ty==0.0.1a23" by @danielhollas on 2025-10-23 16:47_

---

_Renamed from "Performace regression in ty==0.0.1a23" to "Performance regression in ty==0.0.1a23" by @AlexWaygood on 2025-10-23 19:54_

---

_Comment by @MichaReiser on 2025-10-24 06:40_

I haven't investigated this closely but we iterate a cycle 190 times before falling back. This is suspiciously close to the 190 iteration limit for unions ;)

https://github.com/astral-sh/ty/issues/957

---

_Comment by @danielhollas on 2025-10-24 08:07_

Yeah, annotating `self.a:int = 0` solves it so Literal math is a suspect.

> The operation must be acting on a local variable, as opposed to an attribute or subscript expression or a variable defined in some outer scope

Looks like this pyright heuristic (from the linked issue) would solve this.

---

_Closed by @carljm on 2025-10-24 15:24_

---

_Comment by @danielhollas on 2025-12-10 00:51_

The example I gave now type checks instantly 🎉  (ty 0.0.1-alpha.33)

---
