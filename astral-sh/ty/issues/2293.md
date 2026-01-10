---
number: 2293
title: Inconsistent callable inference between direct method assignment and wrapper-returned method
type: issue
state: closed
author: Cjkjvfnby
labels:
  - callables
assignees: []
created_at: 2025-12-31T16:09:14Z
updated_at: 2026-01-05T10:31:12Z
url: https://github.com/astral-sh/ty/issues/2293
synced_at: 2026-01-10T01:51:14Z
---

# Inconsistent callable inference between direct method assignment and wrapper-returned method

---

_Issue opened by @Cjkjvfnby on 2025-12-31 16:09_

### Summary

This code sample works with Mypy and fails on ty.  

No additional configurations, only run: `ty check`

```python
from typing import Callable


def _method(self: "Foo") -> "Foo":
    return Foo()


def _method_wrapper(
) -> Callable[["Foo"], "Foo"]:
    return _method


class Foo:    
    direct_assignment =  _method
    assign_via_wrapper = _method_wrapper()


def boo(c: Foo) -> None:
    c.direct_assignment()
    c.assign_via_wrapper()
# | ^^^^^^^^^^^^^^^^^^^^^^
# |
# info: Union variant `(Foo, /) -> Foo` is incompatible with this call site
# info: Attempted to call union type `Unknown | ((Foo, /) -> Foo)`
# info: rule `missing-argument` is enabled by default
```


The original problem was discovered when using PySpark 3.4 https://github.com/apache/spark/blob/branch-3.4/python/pyspark/sql/column.py#L1065
```python
from pyspark.sql.column import Column
def foo(c: Column) -> Column:
    return c.desc()
```

### Version

0.0.8

---

_Label `callables` added by @mtshiba on 2026-01-01 06:11_

---

_Comment by @mtshiba on 2026-01-01 07:01_

Upon closer inspection, ty's behavior appears to be more sound.

```python
from typing import Callable

class Callee:
    def __call__(self, x: "Foo") -> "Foo":
        return Foo()

def _method_wrapper() -> Callable[["Foo"], "Foo"]:
    return Callee() # This is definitely OK

class Foo:
    assign_via_wrapper = _method_wrapper()

def boo(c: Foo) -> None:
    c.assign_via_wrapper()

boo(Foo())
```

This results in a runtime error.

```
  File "main.py", line 14, in boo
    c.assign_via_wrapper()
    ~~~~~~~~~~~~~~~~~~~~^^
TypeError: Callee.__call__() missing 1 required positional argument: 'x'
```

mypy and pyright miss this error, while ty and pyrefly seem to catch it as an error.

So, perhaps the thing to do here is to improve the error message (`Callable` does not necessarily bind `self`)?

---

_Comment by @Cjkjvfnby on 2026-01-01 15:15_

> This results in a runtime error.

You use a different usecase in your example.


When you define a function inside the class, Python converts it to a bound method ("removes" the first argument from the signature). 

However, when you assign the bound method to the class, it does not become a bound method of this class; it's just an attribute that references this bound method.

```python
>>> print(f.assign_via_wrapper.__call__)
<bound method Callee.__call__ of <__main__.Callee object at 0x000001CAC2CD9940>>
```

<details><summary>Example</summary>
<p>

```python
class One:
    def one(self, x):
        print(f"{self.__class__.__name__} and {x}")

class Two:
    def two(self, x):
        print(f"{self.__class__.__name__} and {x}")
    one = One().one



if __name__ == '__main__':
    t = Two()
    t.one(1)  # <bound method One.one of <__main__.One object a
    t.two(2)  # <bound method Two.two of <__main__.Two object at
```

```
One and 1
Two and 2
```

</p>
</details> 

---

_Comment by @dhruvmanila on 2026-01-05 10:31_

I think the underlying issue is the same as https://github.com/astral-sh/ty/issues/491, a simpler way to reproduce this:

```py
from collections.abc import Callable

class Foo:
    other: Callable[["Foo"], "Foo"]

def boo(c: Foo) -> None:
	# No argument provided for required parameter 1 (missing-argument)
    c.other()
```

The reason it works for the direct assignment is because the type of `Foo.direct_assignment` is same as `_method` which is a function literal.

---

_Closed by @dhruvmanila on 2026-01-05 10:31_

---
