```yaml
number: 19566
title: Trigger UP043 on <3.13 with __future__.annotations
type: issue
state: open
author: Gallaecio
labels:
  - rule
assignees: []
created_at: 2025-07-25T21:58:09Z
updated_at: 2025-07-26T13:48:01Z
url: https://github.com/astral-sh/ruff/issues/19566
synced_at: 2026-01-12T15:54:56Z
```

# Trigger UP043 on <3.13 with __future__.annotations

---

_@Gallaecio_

### Summary

I wonder if this could be considered part of https://github.com/astral-sh/ruff/issues/18502. It may also be related to https://github.com/astral-sh/ruff/issues/19402 and https://github.com/astral-sh/ruff/issues/19359. But I am not sure, and none of those mention this rule specifically.

[UP043](https://docs.astral.sh/ruff/rules/unnecessary-default-type-args/) seems to only trigger in Python 3.13+. However, it seems that (as long as you are using `from __future__ import annotations`?) the new syntax works in older versions. I tried 3.9, and things work both at run time (at least as long as you do not try to resolve the type?) and when running mypy.

### Version

0.12.5

---

_Comment by @ntBre on 2025-07-25 22:48_

Oh cool, thanks for pointing that out! It sounds like this should definitely be added to #19359/#18502, but allowing the rule to apply if `from __future__ import annotations` is already present could also be a separate (preview) change to the rule.

I feel like I'm missing something obvious, but I'm actually having trouble causing errors by using this syntax on earlier versions. The example from the docs, plus an added module-level annotation, seems fine in mypy, pyright, ty, and even Python with 3.9:

```python
# try.py
from collections.abc import Generator, AsyncGenerator


x: Generator[int] = (1 for _ in range(3))


def sync_gen() -> Generator[int]:
    yield 42


async def async_gen() -> AsyncGenerator[int]:
    yield 42


for y in sync_gen():
    print(y)
```

```shell
> mypy try.py --python-version 3.9
Success: no issues found in 1 source file
> pyright --pythonversion 3.9 try.py
0 errors, 0 warnings, 0 informations
> ty check --python-version 3.9 try.py
All checks passed!
> uvx python@3.9 try.py
42
```

Maybe the version check isn't needed in general?


---

_Label `rule` added by @ntBre on 2025-07-25 22:48_

---

_Comment by @Gallaecio on 2025-07-25 23:54_

> I'm actually having trouble causing errors by using this syntax on earlier versions

+1. I assumed it should, [it sounds like it was introduced in 3.13](https://peps.python.org/pep-0696/), but it does not seem to 🤷 .

Also tried aliasing in 3.9, `MyType = Generator[str]`, works as well.

---

_Comment by @AlexWaygood on 2025-07-26 11:52_

> I feel like I'm missing something obvious, but I'm actually having trouble causing errors by using this syntax on earlier versions. The example from the docs, plus an added module-level annotation, seems fine in mypy, pyright, ty, and even Python with 3.9:

At runtime, providing <3 parameters works with `collections.abc.Generator` on all Python versions, but if you use `typing.Generator` you can only provide <3 parameters if you're on Python 3.13+ _or_ it's a type annotation and you have `from __future__ import annotations` enabled:

```pycon
% uvx python3.9
Python 3.9.6 (default, Apr 30 2025, 02:07:17) 
[Clang 17.0.0 (clang-1700.0.13.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import typing, collections.abc
>>>
>>> x = collections.abc.Generator[int]  # succeeds on all versions of Python
>>>
>>> x: collections.abc.Generator[int]  # succeeds on all versions of Python
>>>
>>>
>>> y = typing.Generator[int]  # Fails on Python <3.13 with or without `__future__.annotations`
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 275, in inner
    return func(*args, **kwds)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 828, in __getitem__
    _check_generic(self, params, self._nparams)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 212, in _check_generic
    raise TypeError(f"Too {'many' if alen > elen else 'few'} parameters for {cls};"
TypeError: Too few parameters for typing.Generator; actual 1, expected 3
>>>
>>> y: typing.Generator[int]  # Fails on Python <3.13 without `__future__.annotations`
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 275, in inner
    return func(*args, **kwds)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 828, in __getitem__
    _check_generic(self, params, self._nparams)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 212, in _check_generic
    raise TypeError(f"Too {'many' if alen > elen else 'few'} parameters for {cls};"
TypeError: Too few parameters for typing.Generator; actual 1, expected 3
>>>
>>>
>>> from __future__ import annotations
>>>
>>> z = typing.Generator[int]  # still fails despite the `__future__` import
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 275, in inner
    return func(*args, **kwds)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 828, in __getitem__
    _check_generic(self, params, self._nparams)
  File "/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/lib/python3.9/typing.py", line 212, in _check_generic
    raise TypeError(f"Too {'many' if alen > elen else 'few'} parameters for {cls};"
TypeError: Too few parameters for typing.Generator; actual 1, expected 3
>>>
>>> z: typing.Generator[int]  # now passes due to the `__future__` import
>>>
```

`typing_extensions.Generator` should also allow <3 parameters on all Python versions, regardless of whether `__future__.annotations` has been imported or not. Type checkers aren't aware of the subtle differences between `collections.abc.Generator` and `typing.Generator` here; it would be hard to make them aware of these differences. They assume `collections.abc.Generator` semantics for `typing.Generator` on all Python versions.

---

_Comment by @ntBre on 2025-07-26 13:48_

Thanks Alex! It sounds like we should definitely keep the version check, and the change suggested in the issue would still be helpful, even if it only strictly applies to `typing.Generator`.

---
