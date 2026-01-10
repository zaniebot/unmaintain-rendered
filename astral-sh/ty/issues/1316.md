```yaml
number: 1316
title: "Overloaded non-stub function `f` must have an implementation - issue just on the last overload"
type: issue
state: closed
author: darioinova
labels:
  - question
  - diagnostics
  - overloads
assignees: []
created_at: 2025-10-07T09:11:57Z
updated_at: 2025-10-07T14:41:12Z
url: https://github.com/astral-sh/ty/issues/1316
synced_at: 2026-01-10T02:06:25Z
```

# Overloaded non-stub function `f` must have an implementation - issue just on the last overload

---

_Issue opened by @darioinova on 2025-10-07 09:11_

### Summary

When defining an overloaded function, the **LAST** implementation is always labeled as 
**"Overloaded non-stub function `f` must have an implementation"**

```py
from typing import overload

class A:
    @overload
    def f(v: float) -> float:
        return v * 2

    @overload
    def f(v: int) -> int: # issue here
        return v * 2
```

```py
from typing import overload

class A:
    @overload
    def f(v: float) -> float:
        return v * 2

    @overload
    def f(v: int) -> int:  # no issue here !!!!
        return v * 2

    @overload
    def f(v: str) -> str:
        return v * 2

    @overload
    def f(v: list) -> list:     # issue here
        return v + v
```

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @AlexWaygood on 2025-10-07 09:14_

Thanks for the report!

Could you say a little bit more about what you think the behaviour should be instead? I'm guessing you wouldn't want a diagnostic on every single overload -- would you want the diagnostic to be emitted on the first overload rather than the last overload? Or are you saying that you don't think we should emit a diagnostic at all here?

---

_Label `question` added by @AlexWaygood on 2025-10-07 09:14_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-07 09:14_

---

_Label `overloads` added by @AlexWaygood on 2025-10-07 09:14_

---

_Comment by @darioinova on 2025-10-07 10:10_

All the overloaded functions do have an implementation.
The should be no diagnostic at all.

---

_Comment by @AlexWaygood on 2025-10-07 10:14_

I see! I can't see any implementation in the example snippets you posted in this issue, however. An "overload implementation" is a function not decorated with `@overload` that immediately follows all the `@overload`-decorated functions. See the typing-module docs [here](https://github.com/astral-sh/ty/issues/147) -- there is a non-`@overload`-decorated `process` function immediately after all the overloads that are decorated with `@overload`, and it is the final non-`@overload`-decorated `process()` function that provides the function implementation that is actually used at runtime

---

_Comment by @AlexWaygood on 2025-10-07 10:17_

To be clear, if I try to run your first example at runtime, it is rejected -- and it is this `TypeError` at runtime that ty is trying to warn you about:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import overload
... 
... class A:
...     @overload
...     def f(v: float) -> float:
...         return v * 2
... 
...     @overload
...     def f(v: int) -> int: # issue here
...         return v * 2
...         
>>> A.f(42)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    A.f(42)
    ~~~^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 2681, in _overload_dummy
    raise NotImplementedError(
    ...<3 lines>...
        "by an implementation that is not @overload-ed.")
NotImplementedError: You should not call an overloaded function. A series of @overload-decorated functions outside a stub module should always be followed by an implementation that is not @overload-ed.
```

---

_Comment by @darioinova on 2025-10-07 12:38_

Completely my fault ... actually now I understand the @overload decorator!

Thank you @AlexWaygood !



---

_Closed by @darioinova on 2025-10-07 12:38_

---

_Comment by @sharkdp on 2025-10-07 13:55_

There's probably a chance to improve our diagnostic message here, maybe by linking to the [`typing` module documentation](https://docs.python.org/3/library/typing.html#typing.overload) (the link above seems incorrect)?

---

_Comment by @AlexWaygood on 2025-10-07 14:01_

> There's probably a chance to improve our diagnostic message here

I already have a patch locally ;)

---

_Comment by @AlexWaygood on 2025-10-07 14:41_

See:
- https://github.com/astral-sh/ruff/pull/20745

---
