```yaml
number: 1566
title: Consider unsafely narrowing non-literal type to literal based on equality check
type: issue
state: open
author: alexmacnorthstar
labels:
  - needs-decision
  - narrowing
assignees: []
created_at: 2025-11-14T21:24:41Z
updated_at: 2025-12-23T02:59:34Z
url: https://github.com/astral-sh/ty/issues/1566
synced_at: 2026-01-10T01:56:40Z
```

# Consider unsafely narrowing non-literal type to literal based on equality check

---

_Issue opened by @alexmacnorthstar on 2025-11-14 21:24_

### Summary

Using a guard to check if a str is a value from a list of literals like this:

```py
from typing import Literal

type MyType = Literal['test1', 'test2']

def test(foo:str) -> MyType:
  valid_vals: list[MyType] = ['test1', 'test2']
  if foo in valid_vals:
    return foo
  return 'test1'
```

Gives:

```
error[invalid-return-type]: Return type does not match returned value
 --> ty_test.py:5:22
  |
3 | type MyType = Literal['test1', 'test2']
4 |
5 | def test(foo:str) -> MyType:
  |                      ------ Expected `MyType` because of return type
6 |   valid_vals: list[MyType] = ['test1', 'test2']
7 |   if foo in valid_vals:
8 |     return foo
  |            ^^^ expected `MyType`, found `str`
9 |   return 'test1'
  |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

But foo is constrained to only valid values of the MyType literal at this point

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @carljm on 2025-11-14 21:45_

It's intentional that we haven't so far implemented support for this kind of narrowing, because it is unsound. The type `str` includes subclasses of `str`, which are permitted to implement `__eq__` to compare equal to whatever they like. And a subclass of `str` that compares equal to `"test1"` does not inhabit the type `Literal["test1"]` -- that type is limited to the instance of the actual `str` class with value `"test1"`.

```py
class MyStr(str):
    def __eq__(self, other):
        return True  # YOLO

test(MyStr("something else"))  # returns MyStr("something else") at runtime, violating the return type of `test`
```

If the type of `foo` were a larger union of literal types, we would eliminate types from that union which we know _cannot_ compare equal to `test1`. We currently do this for `in` checks only if the RHS is a tuple, not if its a list.

Pyright narrows in this case; mypy does not.

I will leave this issue open for now, to consider whether a change in behavior makes sense (and provide a central place for user feedback on the question.)

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 21:45_

---

_Renamed from "Support narrowing/inferring to literal after check" to "Support narrowing/inferring non-literal type to literal" by @carljm on 2025-11-14 21:46_

---

_Renamed from "Support narrowing/inferring non-literal type to literal" to "Consider unsafely narrowing non-literal type to literal based on equality check" by @carljm on 2025-11-14 22:01_

---

_Label `needs-decision` added by @carljm on 2025-11-14 22:01_

---

_Label `narrowing` added by @carljm on 2025-11-14 22:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @carljm on 2025-12-23 02:59_

We could add a diagnostic that prohibits overriding `__eq__` on subclasses of all types for which we have literal types, and then go ahead and implement this kind of narrowing. See #2171 where we plan to do this for tuples. I don't know if overriding `__eq__` on `int` or `str` subclasses is more common than on `tuple` subclasses.

The advantage of this approach is that people who aren't worried about the unsoundness and want to match pyright behavior can always just turn off the "unsafe subclass" warning and still get the narrowing.

---
