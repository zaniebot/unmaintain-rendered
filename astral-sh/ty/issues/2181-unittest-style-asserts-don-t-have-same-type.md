---
number: 2181
title: "unittest style asserts don't have same type narrowing behaviour as normal asserts"
type: issue
state: closed
author: joe-pierce
labels:
  - narrowing
assignees: []
created_at: 2025-12-23T09:19:31Z
updated_at: 2025-12-23T13:40:09Z
url: https://github.com/astral-sh/ty/issues/2181
synced_at: 2026-01-10T01:52:52Z
---

# unittest style asserts don't have same type narrowing behaviour as normal asserts

---

_Issue opened by @joe-pierce on 2025-12-23 09:19_

### Summary

unittest-style `self.assert...` is not narrowing types as I'd expect. 

I have just seen this diagnostic:

```
error[non-subscriptable]: Cannot subscript object of type `None` with no `__getitem__` method
   --> tests\test_builder.py:304:27
    |
302 |         # Assert
303 |         self.assertIsNotNone(publisher.message)
304 |         self.assertIsNone(publisher.message["ElapsedTimeMs"])
    |                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
info: rule `non-subscriptable` is enabled by default
```
I would expect the `self.assertIsNotNone` to make line 304 valid. If I change the first like to `assert publisher.message is not None` then no error is flagged.

target python version: 3.12
ty version: ty 0.0.5 (d37b7dbd9 2025-12-20)    

Thanks!

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Label `narrowing` added by @AlexWaygood on 2025-12-23 09:42_

---

_Comment by @AlexWaygood on 2025-12-23 11:19_

Thanks for the report!

You haven't provided us with a minimal repro here, but I'm guessing your code looks something like this?

```py
import unittest

def string_or_int() -> int | None:
    return 42

class Foo(unittest.TestCase):
    def test_foo(self) -> None:
        x = string_or_int()
        self.assertIsNotNone(x)
        reveal_type(x)  # revealed: int | None
```

If so, this is not a form of narrowing supported by any other type checker right now. I think the real solution to this is https://github.com/python/typing/issues/930, which would allow typeshed to annotate the return type of methods like `assertIsNotNone` in such a way that all type checkers would understand their narrowing behaviour.

---

_Comment by @joe-pierce on 2025-12-23 13:32_

Yes that code snippet reflects my raised issue - sounds like a resolution to this sits outside this project so feel free to close, unless you think there may be some improvements to the returned diagnostic in the terminal.

---

_Closed by @AlexWaygood on 2025-12-23 13:40_

---
