```yaml
number: 960
title: "Incorrect type narrow/infer when creating dict with `dict(key=value)` (not for `{\"key\":value}`)"
type: issue
state: closed
author: DanielYang59
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-08-09T20:06:34Z
updated_at: 2025-11-12T19:19:14Z
url: https://github.com/astral-sh/ty/issues/960
synced_at: 2026-01-10T02:06:24Z
```

# Incorrect type narrow/infer when creating dict with `dict(key=value)` (not for `{"key":value}`)

---

_Issue opened by @DanielYang59 on 2025-08-09 20:06_

### Summary

Seems like a bug (if I understand everything correctly) regarding type narrowing of dict with `dict()` interface.


```python
from typing import Any


name: str = "hello"
d: dict[str, Any] = dict(name=name)  # {"name": name} wouldn't
d["age"] = 0
```

Triggers:
```
error[invalid-assignment]: Method `__setitem__` of type `bound method dict[str, str].__setitem__(key: str, value: str, /) -> None` cannot be called with a key of type `Literal["age"]` and a value of type `Literal[0]` on object of type `dict[str, str]`
 --> debug.py:6:1
  |
4 | name: str = "hello"
5 | d: dict[str, Any] = dict(name=name)  # {"name": name} wouldn't
6 | d["age"] = 0
  | ^
  |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```


### Version

ty 0.0.1-alpha.17 (1712284c8 2025-08-06)

---

_Comment by @DanielYang59 on 2025-08-09 20:22_

Noticed another issue related to using `dict()` instead of `{}`:
```python
inner_list = "hello world".split()  # noqa:SIM905

out_dict = dict(hello=inner_list)  # {"hello": inner_list} wouldn't
```

Gives (again using `{}` wouldn't trigger any issue):

```
error[invalid-argument-type]: Argument to bound method `__init__` is incorrect
 --> debug.py:3:17
  |
1 | inner_list = "hello world".split()  # noqa:SIM905
2 |
3 | out_dict = dict(hello=inner_list)  # {"hello": inner_list} wouldn't
  |                 ^^^^^^^^^^^^^^^^ Expected `list[str]`, found `list[LiteralString]`
  |
info: Matching overload defined here
    --> stdlib/builtins.pyi:2907:9
     |
2905 |     def __init__(self) -> None: ...
2906 |     @overload
2907 |     def __init__(self: dict[str, _VT], **kwargs: _VT) -> None: ...  # pyright: ignore[reportInvalidTypeVarUse]  #11780
     |         ^^^^^^^^                       ------------- Parameter declared here
2908 |     @overload
2909 |     def __init__(self, map: SupportsKeysAndGetItem[_KT, _VT], /) -> None: ...
     |
info: Non-matching overloads for bound method `__init__`:
info:   (self) -> None
info:   (self, map: SupportsKeysAndGetItem[_KT@dict, _VT@dict], /) -> None
info:   (self: dict[str, _VT@dict], map: SupportsKeysAndGetItem[str, _VT@dict], /, **kwargs: _VT@dict) -> None
info:   (self, iterable: Iterable[tuple[_KT@dict, _VT@dict]], /) -> None
info:   (self: dict[str, _VT@dict], iterable: Iterable[tuple[str, _VT@dict]], /, **kwargs: _VT@dict) -> None
info:   (self: dict[str, str], iterable: Iterable[list[str]], /) -> None
info:   (self: dict[bytes, bytes], iterable: Iterable[list[bytes]], /) -> None
info: rule `invalid-argument-type` is enabled by default
```



---

_Renamed from "[invalid-assignment] Incorrect type narrowing when creating dict with `dict(key=value)`" to "[invalid-assignment] Incorrect type narrowing when creating dict with `dict(key=value)` instead of `{"key":value}`" by @DanielYang59 on 2025-08-09 20:23_

---

_Renamed from "[invalid-assignment] Incorrect type narrowing when creating dict with `dict(key=value)` instead of `{"key":value}`" to "[invalid-assignment] Incorrect type narrow/infer when creating dict with `dict(key=value)` instead of `{"key":value}`" by @DanielYang59 on 2025-08-09 20:28_

---

_Comment by @DanielYang59 on 2025-08-09 20:29_

One more related bug:
```python
d: dict[str, str | float] = dict(width=1.5)
```

```
error[invalid-assignment]: Object of type `dict[str, float]` is not assignable to `dict[str, str | int | float]`
 --> debug.py:1:1
  |
1 | d: dict[str, str | float] = dict(width=1.5)
  | ^
  |
info: rule `invalid-assignment` is enabled by default
```

---

_Renamed from "[invalid-assignment] Incorrect type narrow/infer when creating dict with `dict(key=value)` instead of `{"key":value}`" to "[invalid-assignment] Incorrect type narrow/infer when creating dict with `dict(key=value)` (not for `{"key":value}`)" by @DanielYang59 on 2025-08-09 20:36_

---

_Renamed from "[invalid-assignment] Incorrect type narrow/infer when creating dict with `dict(key=value)` (not for `{"key":value}`)" to "Incorrect type narrow/infer when creating dict with `dict(key=value)` (not for `{"key":value}`)" by @DanielYang59 on 2025-08-09 20:36_

---

_Comment by @MatthewMckee4 on 2025-08-10 20:56_

This is due to the fact that we infer `dict(name=name)` as `dict[str, str]`, and the `dict[str, Any]` does not take priority because `dict[str, str]` is not a [gradual type](https://typing.python.org/en/latest/spec/concepts.html#gradual-types).

If you need this to work with the `dict` builtin, then you can do `d = dict[str, Any](name=name)` provided you are on python 3.9 or greater.



---

_Comment by @AlexWaygood on 2025-08-10 21:03_

Ideally we would use the user-supplied declaration as "type context" to take into account when solving the TypeVars in the `dict` constructor calls; this would avoid the false positives at the places where the keys are set later on. This is also known as "bidirectional type inference", and it's something both mypy and pyright are capable of, so it's definitely something we should aspire to do as well, but it requires some design from us. See https://github.com/astral-sh/ty/issues/168.

---

_Label `generics` added by @AlexWaygood on 2025-08-11 13:30_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-08-11 13:30_

---

_Comment by @ibraheemdev on 2025-11-04 22:13_

These two snippets are examples of https://github.com/astral-sh/ty/issues/1228, which will be fixed by https://github.com/astral-sh/ruff/pull/20933:
```py
from typing import Any

def _(key: str):
    a: dict[str, Any] = dict(key=key)
    a[key] = 0
```
```py
b: dict[str, str | float] = dict(width=1.5)
```

The last example I think is actually the correct diagnostic, and is unrelated to `dict`:
```py
# error: expected `list[str]`, found `list[LiteralString]`
x: list[str] = "hello world".split()
```

Pyright also errors in this case. I guess we *could* promote the literal string based on the type annotation, but I'm not at all sure how that would work.

---

_Comment by @ibraheemdev on 2025-11-12 19:19_

Confirmed that the first two examples are fixed on main.

---

_Closed by @ibraheemdev on 2025-11-12 19:19_

---
