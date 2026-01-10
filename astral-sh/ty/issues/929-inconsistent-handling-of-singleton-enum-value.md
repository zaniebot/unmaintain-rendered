```yaml
number: 929
title: "Inconsistent handling of singleton `Enum` value assigned to variable"
type: issue
state: closed
author: mthuurne
labels:
  - question
assignees: []
created_at: 2025-08-03T00:17:24Z
updated_at: 2025-10-06T16:34:18Z
url: https://github.com/astral-sh/ty/issues/929
synced_at: 2026-01-10T02:06:24Z
```

# Inconsistent handling of singleton `Enum` value assigned to variable

---

_Issue opened by @mthuurne on 2025-08-03 00:17_

### Summary

[testcase in playground](https://play.ty.dev/63e090fe-8583-4523-8b42-25e413ea1e92)

When I check the following code:
```py
from enum import Enum
from typing import Final, Literal


class Singleton(Enum):
    instance = "there can be only one"


instance1 = Singleton.instance
instance2: Singleton = Singleton.instance
instance3: Final[Singleton] = Singleton.instance
instance4: Literal[Singleton.instance] = Singleton.instance


def f(x: Singleton | int) -> None:
    instance5 = Singleton.instance

    if x is not Singleton.instance:
        print(x + 0)
    if x is not instance1:
        print(x + 1)
    if x is not instance2:
        print(x + 2)
    if x is not instance3:
        print(x + 3)
    if x is not instance4:
        print(x + 4)
    if x is not instance5:
        print(x + 5)
```
ty reports:
```
error[unsupported-operator]: Operator `+` is unsupported between objects of type `Singleton | int` and `Literal[1]`
  --> enum_singleton.py:21:15
   |
19 |         print(x + 0)
20 |     if x is not instance1:
21 |         print(x + 1)
   |               ^^^^^
22 |     if x is not instance2:
23 |         print(x + 2)
   |
info: rule `unsupported-operator` is enabled by default
```
I'd expect no issue to be reported, as enum `Singleton` only has one member and enum members have exactly one instance.

For some reason, only in the case of `instance1` (assignment without annotation at module level) does ty not detect that `instance` is the only member of the enum.

### Version

ty 0.0.1-alpha.16

---

_Comment by @sharkdp on 2025-08-04 07:24_

Thank you for reporting this.

I can see how this looks confusing, but everything is working as intended here. `instance1` is not annotated. If you check its type inside of the function `f`, you'll notice that we infer `Unknown | Singleton`. That's because other code might modify this module-global variable. Since it does not have a declared type, it might be changed to anything — without a type checker complaining about the assignment. For example, there could be a function like the following inside this module (or even another module):
```py
def modify():
    global instance1
    instance1 = "whatever"
```

Consequently, it is not safe to assume that the `if x is not instance1` check will exclude `Singleton` as a possibility.


You can read more about our reasoning [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md).

---

_Label `question` added by @sharkdp on 2025-08-04 07:24_

---

_Closed by @AlexWaygood on 2025-10-06 16:34_

---
