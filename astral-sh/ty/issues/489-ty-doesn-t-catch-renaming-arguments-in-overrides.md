```yaml
number: 489
title: "`ty` doesn't catch renaming arguments in overrides"
type: issue
state: closed
author: mathialo
labels: []
assignees: []
created_at: 2025-05-22T17:40:13Z
updated_at: 2025-05-22T17:47:03Z
url: https://github.com/astral-sh/ty/issues/489
synced_at: 2026-01-12T15:54:23Z
```

# `ty` doesn't catch renaming arguments in overrides

---

_@mathialo_

### Summary

First of all, after having migrated every project I'm involved with to `ruff` and `uv`, I'm really excited to see where `ty` is going! 

**Summary**

When overriding methods in subclasses, it's important to keep the same argument names since any argument in Python can be given as a keyword argument. If an override in a subclass changes the name of a parameter, it breaks the polymorphism for any user of the class using keyword arguments

**To Reproduce**

```python
from abc import ABC, abstractmethod


class Adder(ABC):
    @abstractmethod
    def add(self, a: int, b: int) -> int:
        pass


class MyAdder(Adder):
    def add(self, a: int, b: int) -> int:
        return a + b


class OtherAdder(Adder):
    def add(self, a: int, new_b: int) -> int:
        return a + new_b


def use_adder(adder: Adder) -> int:
    return adder.add(a=1, b=2)


print(f"1 + 2 = {use_adder(MyAdder())}")  # works
print(f"1 + 2 = {use_adder(OtherAdder())}")  # broken, but type checks in ty
```

The code example above should not type check, as `OtherAdder` is not a valid subclass of `Adder`. If I type check this code in `pyright`, it correctly identifies this issue:

``` commandline
$ uvx pyright classes.py
/path/to/classes.py
  /path/to/classes.py:16:9 - error: Method "add" overrides class "Adder" in an incompatible manner
    Parameter 3 name mismatch: base parameter is named "b", override parameter is named "new_b" (reportIncompatibleMethodOverride)
1 error, 0 warnings, 0 informations
```

`ty` lets this error through:

``` commandline
$ uvx ty check classes.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```


### Version

ty 0.0.1-alpha.6

---

_Closed by @AlexWaygood on 2025-05-22 17:46_

---

_Comment by @AlexWaygood on 2025-05-22 17:47_

Thanks! Yes, we don't enforce the Liskov Substitution Principle at all yet. This is a priority for us to complete before the Beta release.

---
