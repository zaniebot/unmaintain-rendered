```yaml
number: 2449
title: False positives for cls/self in dict creation
type: issue
state: closed
author: adamjstewart
labels: []
assignees: []
created_at: 2026-01-11T17:06:44Z
updated_at: 2026-01-11T17:50:33Z
url: https://github.com/astral-sh/ty/issues/2449
synced_at: 2026-01-12T15:54:26Z
```

# False positives for cls/self in dict creation

---

_@adamjstewart_

### Summary

Ty appears to be hard-coded to assume that `cls` and `self` are reserved keywords for classes. However, this is not always true, for example in the following perfectly valid (albeit confusing) Python code:
```python
dict(cls=1)
dict(self=2)
```
`ty check` with no custom `pyproject.toml` configuration results in:
```
error[parameter-already-assigned]: Multiple values provided for parameter `cls` of function `__new__`
 --> test.py:1:6
  |
1 | dict(cls=1)
  |      ^^^^^
2 | dict(self=2)
  |
info: rule `parameter-already-assigned` is enabled by default

error[no-matching-overload]: No overload of bound method `__init__` matches arguments
 --> test.py:2:1
  |
1 | dict(cls=1)
2 | dict(self=2)
  | ^^^^^^^^^^^^
  |
info: First overload defined here
    --> stdlib/builtins.pyi:2962:9
     |
2960 |     # Also multiprocessing.managers.SyncManager.dict()
2961 |     @overload
2962 |     def __init__(self) -> None: ...
     |         ^^^^^^^^^^^^^^^^^^^^^^
2963 |     @overload
2964 |     def __init__(self: dict[str, _VT], **kwargs: _VT) -> None: ...  # pyright: ignore[reportInvalidTypeVarUse]  #11780
     |
info: Possible overloads for bound method `__init__`:
info:   (self) -> None
info:   (self: dict[str, _VT@dict], **kwargs: _VT@dict) -> None
info:   (self, map: SupportsKeysAndGetItem[_KT@dict, _VT@dict], /) -> None
info:   (self: dict[str, _VT@dict], map: SupportsKeysAndGetItem[str, _VT@dict], /, **kwargs: _VT@dict) -> None
info:   (self, iterable: Iterable[tuple[_KT@dict, _VT@dict]], /) -> None
info:   (self: dict[str, _VT@dict], iterable: Iterable[tuple[str, _VT@dict]], /, **kwargs: _VT@dict) -> None
info:   (self: dict[str, str], iterable: Iterable[list[str]], /) -> None
info:   (self: dict[bytes, bytes], iterable: Iterable[list[bytes]], /) -> None
info: rule `no-matching-overload` is enabled by default

Found 2 diagnostics
```
For comparison, `mypy --strict` does not flag these lines of code.

### Version

ty 0.0.11

---

_Comment by @AlexWaygood on 2026-01-11 17:28_

Thanks for the report!

> Ty appears to be hard-coded to assume that `cls` and `self` are reserved keywords for classes.

There's no hard-coding of this, I can assure you :-)

This looks like a typeshed bug to me, not a ty bug. Passing `self` as a keyword argument into `dict.__init__` only works at runtime because `dict.__init__`'s first argument is positional-only at runtime. (It _cannot_ be passed via keyword argument at runtime, therefore there's no ambiguity about which parameter `self=42` is "filling" -- it _must_ be the `**kwargs` keyword-variadic parameter in the signature for `dict.__init__`.)

We can see here that `__init__` methods are not treated specially by Python at runtime; it really is illegal to pass the `self` parameter twice if the `self` parameter is not positional-only:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class PositionalOnly:
...     def __init__(self, /, **kwargs): ...
...     
>>> class PosOrKeyword:
...     def __init__(self, **kwargs): ...
...     
>>> PositionalOnly(self=42)
<__main__.PositionalOnly object at 0x101718c20>
>>> PosOrKeyword(self=42)
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    PosOrKeyword(self=42)
    ~~~~~~~~~~~~^^^^^^^^^
TypeError: PosOrKeyword.__init__() got multiple values for argument 'self'
>>> PositionalOnly.__init__(PositionalOnly(), self=42)
>>> PosOrKeyword.__init__(PosOrKeyword(), self=42)
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    PosOrKeyword.__init__(PosOrKeyword(), self=42)
    ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: PosOrKeyword.__init__() got multiple values for argument 'self'
```

So the fix for this issue will be to make the `self` parameter positional-only in [these methods](https://github.com/python/typeshed/blob/5a45a9a5bcec0c64f7fcaf3db92b9159cee05d9c/stdlib/builtins.pyi#L1178-L1205) upstream. If a PR doing that is accepted, we'll automatically reflect the changes in our vendored copy of typeshed's stubs after a few days. (We sync our vendored version of typeshed every two weeks.)

---

_Closed by @AlexWaygood on 2026-01-11 17:28_

---

_Comment by @AlexWaygood on 2026-01-11 17:30_

> For comparison, `mypy --strict` does not flag these lines of code.

Indeed. And it also does not emit a diagnostic on this code, even though the code fails at runtime!

```py
class Foo:
    def __init__(self, **kwargs: object) -> None: ...

Foo(self=42)
```

---

_Comment by @adamjstewart on 2026-01-11 17:37_

> Passing `self` as a keyword argument into `dict.__init__` only works at runtime because `dict.__init__`'s first argument is positional-only. (It cannot be passed via keyword, therefore there's no ambiguity about which parameter `self=42` is "filling" -- it _must_ be the `**kwargs` keyword-variadic parameter in the signature for `dict.__init__`.)

I don't fully understand this part. From what I can tell, this is valid Python code:
```pycon
>>> dict(cls=1, self=2)
{'cls': 1, 'self': 2}
>>> dict(self=1, cls=2)
{'self': 1, 'cls': 2}
```

---

_Comment by @adamjstewart on 2026-01-11 17:40_

Oh, I think I understand. The code is valid, but ty marks it as invalid because typeshed is incorrect. Let me see if I can work up a PR. Thanks!

---

_Comment by @AlexWaygood on 2026-01-11 17:41_

Yes, sorry! The first parameter in typeshed's `dict.__init__` signature is currently marked as positional-or-keyword, but it should be marked as positional-only, and that's where the bug is.

---

_Comment by @adamjstewart on 2026-01-11 17:44_

Based on comments, it seems this was somewhat intentional while mypy caught up to pyright? https://github.com/python/typeshed/pull/11780

---

_Comment by @AlexWaygood on 2026-01-11 17:47_

> Based on comments, it seems this was somewhat intentional while mypy caught up to pyright? [python/typeshed#11780](https://github.com/python/typeshed/pull/11780)

I'm confused -- I don't see any comments in that linked thread that are relevant to what we're discussing here.

---

_Comment by @adamjstewart on 2026-01-11 17:48_

Ah, I'm talking about comments like:
```python
    @overload
    def __init__(self: dict[str, _VT], **kwargs: _VT) -> None: ...  # pyright: ignore[reportInvalidTypeVarUse]  #11780
```
I assume the fix you're envisioning is changing this to:
```python
    @overload
    def __init__(self: dict[str, _VT], /, **kwargs: _VT) -> None: ...
```
? And I'm also assuming that this is the reason for the pyright ignore?

---

_Comment by @AlexWaygood on 2026-01-11 17:49_

That is indeed the fix I'm envisioning, but that's not the reason for the pyright ignore

---

_Comment by @AlexWaygood on 2026-01-11 17:50_

I made a PR: https://github.com/python/typeshed/pull/15262

---
