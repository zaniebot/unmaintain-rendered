```yaml
number: 864
title: provide more diagnostic context for invalid-context-manager
type: issue
state: open
author: jelly
labels:
  - diagnostics
assignees: []
created_at: 2025-07-21T15:34:31Z
updated_at: 2026-01-09T01:50:05Z
url: https://github.com/astral-sh/ty/issues/864
synced_at: 2026-01-12T15:54:24Z
```

# provide more diagnostic context for invalid-context-manager

---

_@jelly_

(The error initially reported below is a true positive, but our diagnostic message for bad context manager method implementations should describe why we consider `__enter__` or `__exit__` wrongly implemented. That is, we should propagate the actual call error encountered.)

### Summary

<details>

Testing ty on our Cockpit Python code found a false positive in our custom contextmanager which takes an argument:

```python
import os
from typing import Any

class Handle(int):
    """An integer subclass that makes it easier to work with file descriptors"""

    def __new__(cls, fd: int = -1) -> 'Handle':
        return super(Handle, cls).__new__(cls, fd)

    # separate __init__() to set _needs_close mostly to keep pylint quiet
    def __init__(self, fd: int = -1):
        super().__init__()
        self._needs_close = fd != -1

    def __bool__(self) -> bool:
        return self != -1

    def close(self) -> None:
        if self._needs_close:
            self._needs_close = False
            os.close(self)

    def __eq__(self, value: object) -> bool:
        if int.__eq__(self, value):  # also handles both == -1
            return True

        if not isinstance(value, int):  # other object is not an int
            return False

        if not self or not value:  # when only one == -1
            return False

        return os.path.sameopenfile(self, value)

    def __del__(self) -> None:
        if self._needs_close:
            self.close()

    def __enter__(self) -> 'Handle':
        return self

    def __exit__(self, _type: type, _value: object, _traceback: object) -> None:
        self.close()

    @classmethod
    def open(cls, *args: Any, **kwargs: Any) -> 'Handle':
        return cls(os.open(*args, **kwargs))

    def steal(self) -> 'Handle':
        self._needs_close = False
        return self.__class__(int(self))

dir_fd = os.open('/', os.O_RDONLY)
with Handle.open('tmp', os.O_RDONLY, dir_fd=dir_fd) as fd:
    print(fd)
```

Error:
```
error[invalid-context-manager]: Object of type `Handle` cannot be used with `with` because it does not correctly implement `__exit__`
  --> contenxt-manager-ty-issue.py:80:6
   |
79 | dir_fd = os.open('/', os.O_RDONLY)
80 | with Handle.open('tmp', os.O_RDONLY, dir_fd=dir_fd) as fd:
   |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
81 |     print(fd)
   |
info: rule `invalid-context-manager` is enabled by default
```

</details>

I managed to reduce this to down to:
```python
class TestContextManager:
    def __init__(self, name: str) -> None:
        self.name = name

    def __enter__(self) -> 'TestContextManager':
        return self

    def __exit__(self, _type: type, _value: object, _traceback: object) -> None:
        ...

with TestContextManager('something') as foo:
    ...
```

Which gives:

```
error[invalid-context-manager]: Object of type `TestContextManager` cannot be used with `with` because it does not correctly implement `__exit__`
  --> foo.py:11:6
   |
 9 |         ...
10 |
11 | with TestContextManager('something') as foo:
   |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
12 |     ...
   |
info: rule `invalid-context-manager` is enabled by default
```

### Version

ty ruff/0.12.4+31 (c2380fa0e 2025-07-21)

---

_Comment by @carljm on 2025-07-21 15:46_

Thanks for the report! I think this error is correct, though our diagnostic could definitely be more informative here.

The three arguments to `__exit__` are all set to `None` in case there is no exception, so the annotation `_type: type` is not correct. It should be `type | None` (or more precisely it could be `type[BaseException] | None`). But the key part is that `| None` must be included. If I include that, the error goes away: https://play.ty.dev/a1ff10ae-ffc2-42f7-b32a-212213ed34db

(Note the same issue in principle applies to `_value` and `_traceback` arguments also, but it doesn't show up here because you annotate those as `object`, which already includes `None`.)

I will leave this issue open but re-purpose it to describe an improvement to the diagnostic message here.

---

_Comment by @AlexWaygood on 2025-07-21 15:48_

I was typing out exactly the same message as @carljm. Here's a demonstration in the REPL of why it's important that the `type_` parameter in your `__exit__` method needs to have an annotation that indicates it can accept `None` being passed in -- if no exception is raised in the `with` block, `None` will be passed for all three non-`self` parameters to `__exit__`:

```pycon
>>> class Foo:
...     def __enter__(self): return self
...     def __exit__(self, type_, value, traceback):
...         print(f"{type_=}, {value=}, {traceback=}")
...         
>>> with Foo():
...     pass
...     
type_=None, value=None, traceback=None
```

So ty is essentially warning you that the function says it can only accept instances of `type`, but it might be passed `None` as a result of the `with` block

---

_Renamed from "invalid-context-manager wrongly raised for a contextmanager which takes an argument" to "provide more diagnostic context for invalid-context-manager" by @carljm on 2025-07-21 15:48_

---

_Label `diagnostics` added by @AlexWaygood on 2025-07-21 15:48_

---

_Comment by @jelly on 2025-07-21 16:08_

Thanks for the quick replies, I didn't make the connection of "does not correctly implement `__exit__`" being related to the typing of `__exit__`. Maybe the diagnostic can also highly the `__exit__` implementation?

(Also really impressive that `ty` caught this as mypy/pyrefly didn't warn about it)

---

_Added to milestone `Stable` by @carljm on 2026-01-09 01:50_

---
