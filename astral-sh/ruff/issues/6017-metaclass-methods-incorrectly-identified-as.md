---
number: 6017
title: Metaclass methods incorrectly identified as classmethods
type: issue
state: open
author: jamesdow21
labels:
  - needs-decision
assignees: []
created_at: 2023-07-23T17:24:53Z
updated_at: 2023-07-24T02:23:24Z
url: https://github.com/astral-sh/ruff/issues/6017
synced_at: 2026-01-10T01:22:45Z
---

# Metaclass methods incorrectly identified as classmethods

---

_Issue opened by @jamesdow21 on 2023-07-23 17:24_

ruff incorrectly believes that the `__call__` method in a metaclass should be a classmethod and gives error N804 that the first argument should be named "cls" instead of "self", but only for immediate subclasses of `type` or `ABCMeta` (and possibly others, those were the only two I've tried)

saving the following in a file named "meta_call.py" and checking with ruff gives an error only on the `__call__` definitions of `BaseMeta` and `ABCBaseMeta`
```
from abc import ABCMeta
from typing import Any


class BaseMeta(type):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)


class SubMeta(BaseMeta):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)


class ABCBaseMeta(ABCMeta):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)


class SubABCMeta(ABCBaseMeta):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)


class AMeta(type):
    pass


class BMeta(AMeta):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)


class CMeta(BMeta):
    def __call__(self, *args: Any, **kwds: Any) -> Any:
        return super().__call__(*args, **kwds)

```

command: `ruff check --select N meta_call.py`
version: 0.0.280
default settings (i.e. just running from ruff freshly installed into pipx standalone environment)


ruff's output
```
meta_call_err.py:6:18: N804 First argument of a class method should be named `cls`
meta_call_err.py:16:18: N804 First argument of a class method should be named `cls`
Found 2 errors.
```

[`ABCMeta` itself](https://github.com/python/cpython/blob/956b3de816f4b18f6b947a11c8eec6c39f60ea88/Lib/abc.py#L92) inherits directly from type and doesn't do anything to modify the definition of `__call__`  

[In typeshed at least](https://github.com/python/typeshed/blob/7d33060e6ae3ebe54462a891f0c566c97371915b/stdlib/builtins.pyi#L195), the parameter name for `type.__call__` is self


---

_Renamed from "M" to "Metaclass `__call__` method incorrectly identified as a classmethod" by @jamesdow21 on 2023-07-23 17:25_

---

_Comment by @jamesdow21 on 2023-07-23 17:31_

I know the choice of `cls` or `self` in a metaclass can be a touch confusing, because an instance (expected to be named "self" in an instance method) of a metaclass is a class (which is normally expected to be assigned to "cls")

I wasn't sure if this was some special casing for metaclasses in the naming rules, but if so, it doesn't seem to be accounting for the possibility of sub-metaclasses

---

_Renamed from "Metaclass `__call__` method incorrectly identified as a classmethod" to "Metaclass methods incorrectly identified as classmethods" by @jamesdow21 on 2023-07-23 17:42_

---

_Comment by @jamesdow21 on 2023-07-23 17:44_

After a bit more testing, this is actually much more general for any method on a metaclass derived directly from either `type` or `ABCMeta` (which as far as I can tell are the only two metaclasses available from the standard library)

```
from abc import ABCMeta

class A(type):
    def foo(self) -> None:
        print("I am an instance method of metaclass A")


class B(A):
    def bar(self) -> None:
        print("I am an instance method of metaclass B")


class C(ABCMeta):
    def baz(self) -> None:
        print("I am an instance method of metaclass C")

```

ruff reports N804 errors on `A.foo` and `C.baz` methods, but not `B.bar`

---

_Comment by @charliermarsh on 2023-07-23 17:46_

I think it is special-cased somewhere, and we likely did that to follow some other tool's conventions, probably `pep8-naming` which has the same behavior as Ruff on the snippet you just shared:

```console
❯ flake8 foo.py
foo.py:3:1: E302 expected 2 blank lines, found 1
foo.py:4:14: N804 first argument of a classmethod should be named 'cls'
foo.py:14:14: N804 first argument of a classmethod should be named 'cls'
```

---

_Comment by @jamesdow21 on 2023-07-23 17:54_

I don't know rust in particular, but if I understand correctly I believe this snippet is where that special casing is done

https://github.com/astral-sh/ruff/blob/5dbb4dd8235e389ae299d57fb23e36bcc9995c66/crates/ruff_python_semantic/src/analyze/function_type.rs#L46-L52

Looks like it is indeed special casing exactly "type" or "abc.ABCMeta" as the direct parent class

---

_Label `needs-decision` added by @charliermarsh on 2023-07-24 02:23_

---

_Referenced in [astral-sh/ruff#15143](../../astral-sh/ruff/issues/15143.md) on 2024-12-26 09:19_

---

_Referenced in [astral-sh/ruff#13698](../../astral-sh/ruff/issues/13698.md) on 2024-12-26 09:20_

---
