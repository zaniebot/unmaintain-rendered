```yaml
number: 592
title: "`invalid-parameter-default` error from generic with default of `None`"
type: issue
state: open
author: samuelcolvin
labels:
  - generics
assignees: []
created_at: 2025-06-06T12:35:46Z
updated_at: 2025-11-13T06:33:07Z
url: https://github.com/astral-sh/ty/issues/592
synced_at: 2026-01-10T02:06:24Z
```

# `invalid-parameter-default` error from generic with default of `None`

---

_Issue opened by @samuelcolvin on 2025-06-06 12:35_

### Summary

Checking the following code

```py
from typing import TypeVar

D = TypeVar('D')


def foo(x: D = None) -> D:
    return x
```

Gives

```
error[invalid-parameter-default]: Default value of type `None` is not assignable to annotated parameter type `D`
 --> ex.py:6:9
  |
6 | def foo(x: D = None) -> D:
  |         ^^^^^^^^^^^
7 |     return x
  |
info: rule `invalid-parameter-default` is enabled by default
```

This passes with pyright and IMHO should not cause an error.

Apologies if this is reported elsewhere, nothing came up with the search "invalid-parameter-default generic".

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @AlexWaygood on 2025-06-06 13:02_

Thanks for the report!

It's interesting that pyright accepts this, [even in strict mode](https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgGoCGIAUFQCJQC8hJFIAFAOS0cCUNVAE2LAowMGDYAPAFxR6TAHJgUxHlAC0APjnSqUfVBDEYAVxAookqkA). For comparison, mypy [rejects this](https://mypy-play.net/?mypy=latest&python=3.12&gist=60a0120a02a47a7b15e162a9c143e639), as does [pyrefly](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgGoCGIAUFQCJQC8hJFIAFAOS0cCUNVAE2LAowMGDYAPAFxR6TAHJgUxHlAC0APjnSqUfVBDEYAVxAookqkA).

Getting ty to both accept this _and_ correctly solve the TypeVar in cases where no value is passed for the parameter could be tricky. But I do think we should at least give a better diagnostic here.

---

_Label `generics` added by @AlexWaygood on 2025-06-06 13:02_

---

_Comment by @sharkdp on 2025-06-06 13:29_

Unless this is explicitly specified somewhere, I don't really see a good reason why mypy and pyrefly reject this?

> Getting ty to both accept this _and_ correctly solve the TypeVar […] could be tricky

Why do you think so? Constraint-solving-wise, couldn't `f()` be handled in the exact same way as `f(None)`?

---

_Comment by @AlexWaygood on 2025-06-06 13:34_

> Constraint-solving-wise, couldn't `f()` be handled in the exact same way as `f(None)`?

Sure. But I think you might have to handle bare TypeVars in this position differently to TypeVars nested inside other contexts. For comparison, here's a snippet that neither mypy nor pyright rejects, though I think they probably should? Pyright reveals `Unknown` for the last line; mypy reveals `Never`:

```py
from typing import TypeVar

D = TypeVar('D')

def foo(x: D | None = None) -> D:
    assert x
    return x

reveal_type(foo())  # ?
```

---

_Comment by @sharkdp on 2025-06-06 13:40_

Okay, but I don't see how that's related to the question whether or not we should be able to understand calls to generic functions with default values. You could ask the same questions for `def foo(x: D | None) -> D` with an explicit `foo(None)` call.

---

_Comment by @AlexWaygood on 2025-06-06 13:46_

That's true.

I'm pretty sure that there are cases where using TypeVars for parameters with defaults can cause issues, but possibly these only apply to constrained or bound TypeVars. I'd have to reread https://github.com/microsoft/pyright/issues/3494 and https://github.com/python/typeshed/issues/7928 to refresh my memory on exactly what the issues are...

---

_Comment by @erictraut on 2025-06-06 16:45_

Handling defaults for parameters annotated with TypeVars is tricky. As noted above, mypy rejects it outright. It is even disallowed in TypeScript, which generally has a more advanced type system and supports more features than Python.

I decided to support it in pyright because it was a highly-requested feature. It's also one of the most-highly-requested features in the [mypy issue tracker](https://github.com/python/mypy/issues/3737).

It's pretty straightforward to handle in cases where the TypeVar occurs only once in a function signature, but it's more complicated when the TypeVar appears multiple times. There are also some complexities when it's used in a constructor with a class-scoped TypeVar. I think I was able to plug all of the type safety issues that arose from this feature, but it wasn't easy. It's also possible that there are some subtle holes that I missed. Here are some code samples that show where it can be problematic. All of these required additional logic in the type checker to handle correctly.

```python
class A[T]:
    def __init__(self, value: T = 1):
        self.value = value


a1 = A()  # OK
a2 = A[str]()  # Error
```

```python
class B[T]:
    def __init__(self, value: T) -> None:
        self._value: T = value

    def update(self, value: T = 0, /) -> "B[T]":
        return B(value)

b = B("")
b.update("")  # OK
b.update()  # Error
```

```python
from typing import Mapping, OrderedDict


class C[T: Mapping[str, str]]:
    def __init__(self, dict_type: type[T] = OrderedDict[str, str]) -> None: ...

    def defaults(self) -> T: ...

class UserMapping(Mapping[str, str]): ...

c1 = C[UserMapping](UserMapping)  # OK
c2 = C[UserMapping]()  # Error
```

```python
def func1[T](x: list[T], y: T = 1) -> T: ...

func1([""], "")  # OK
func1([""])  # Error

x1: Callable[[list[int]], int] = func1  # OK
x2: Callable[[list[str]], str] = func1  # Error
```

```python
def test1[T](func: Callable[[T], T], value: T) -> T:
    return func(value)

def func2[T](x: T, y: Iterable[T] = [""], z: T = "", /) -> T: ...

reveal_type(test1(func2, 1))  # str | int
```

I'll also note that a default value of `None` introduces another challenge if the type checker optionally supports the original PEP 484 (now obsolete) semantics that `= None` in a parameter annotation implicitly makes the parameter type `Optional`. Pyright and mypy both support (non-default) modes that enable this obsolete behavior. When this mode is enabled, `x: T = None` effectively means `x: T | None = None`, which can leave the TypeVar unsolved. I don't know if ty plans to support such a mode for backward compatibility or if @samuelcolvin assumed these semantics in his initial post.

---

_Comment by @erictraut on 2025-06-06 17:20_

Here's another tricky case that I forgot in my list above. Note the revealed types for `transformed3` and `transformed4` below. For `func3`, the higher-order type variables remain unspecialized and can be solved later. For `func4`, the presence of a default value for parameter `y` requires `T@func4` to be specialized in the call `transform(func4)`.

```python
from typing import Callable

def transform[T, R](cb: Callable[[list[T]], R]) -> Callable[[list[T]], R]: ...

def func3[T](x: list[T], y: int = 1) -> T: ...
def func4[T](x: list[T], y: T = 1) -> T: ...

func3([""])  # OK
func4([""])  # Error

transformed3 = transform(func3)
transformed4 = transform(func4)

reveal_type(transformed3)  # (list[T]) -> T
reveal_type(transformed4)  # (list[int]) -> int

transformed3([""])  # OK
transformed4([""])  # Error
```

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:33_

---
