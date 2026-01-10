---
number: 2242
title: ty gets confused when reassigning to bound method
type: issue
state: closed
author: Paillat-dev
labels: []
assignees: []
created_at: 2025-12-27T22:50:28Z
updated_at: 2025-12-29T11:21:45Z
url: https://github.com/astral-sh/ty/issues/2242
synced_at: 2026-01-10T01:51:14Z
---

# ty gets confused when reassigning to bound method

---

_Issue opened by @Paillat-dev on 2025-12-27 22:50_

### Summary

When reassigning a bound method to another bound method, type inference glitches and thinks it is now unbound.

The following code:
```py
class State:
    def __init__(self, rebind: bool = True):
        if rebind:
            self.foo = self.bar  # ty: ignore[invalid-assignment]

    async def foo(self, x: int) -> None:
        pass

    async def bar(self, x: int) -> None:
        pass


class User:
    _state: State

    def test(self) -> None:
        self._state.foo(123)
```
Causes the confusing:

```
uvx ty check --output-format=concise .\test_something.py
test_undetected_self.py:17:9: error[missing-argument] No argument provided for required parameter `x` of function `foo`
test_undetected_self.py:17:25: error[invalid-argument-type] Argument to function `foo` is incorrect: Expected `State`, found `Literal[123]`
Found 2 diagnostics
`````
https://play.ty.dev/540400b7-d0dd-4c3d-acf3-2507ab807e93


The assignment `self.foo = self.bar` is obviously unsafe, and being rightfully flagged by ty, and for that reason a `# ty: ignore[invalid-assignment]` comment is added to the line.

However, at least to my understanding, ty should not try to infer types from that assignment, and just assume `foo` never changed. Maybe I am wrong on that ?

### Version

ty 0.0.7 (cf82a04b5 2025-12-24)

---

_Renamed from "ty gets confused when assigning to bound method" to "ty gets confused when reassigning to bound method" by @Paillat-dev on 2025-12-27 22:50_

---

_Comment by @MichaReiser on 2025-12-29 11:21_

Pyright, pyrefly, and mypy are all okay with this and I think this is the same as https://github.com/astral-sh/ty/issues/350

---

_Closed by @MichaReiser on 2025-12-29 11:21_

---
