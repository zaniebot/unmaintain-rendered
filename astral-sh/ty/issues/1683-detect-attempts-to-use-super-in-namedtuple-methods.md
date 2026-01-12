```yaml
number: 1683
title: "Detect attempts to use `super()` in `NamedTuple` methods"
type: issue
state: closed
author: AlexWaygood
labels:
  - runtime semantics
  - namedtuples
assignees: []
created_at: 2025-11-29T18:17:03Z
updated_at: 2025-11-30T15:49:07Z
url: https://github.com/astral-sh/ty/issues/1683
synced_at: 2026-01-12T15:54:25Z
```

# Detect attempts to use `super()` in `NamedTuple` methods

---

_@AlexWaygood_

### Summary

This raises a `TypeError` on Python 3.14+ at runtime:

```pycon
% uvx python3.14 
Python 3.14.0 (main, Oct 10 2025, 12:54:13) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import NamedTuple
>>> class F(NamedTuple):
...     x: int
...     def method(self):
...         super()
...         
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    class F(NamedTuple):
    ...<2 lines>...
            super()
  File "/Users/alexw/Library/Application Support/uv/python/cpython-3.14.0-macos-aarch64-none/lib/python3.14/typing.py", line 2954, in __new__
    raise TypeError(
        "uses of super() and __class__ are unsupported in methods of NamedTuple subclasses")
TypeError: uses of super() and __class__ are unsupported in methods of NamedTuple subclasses
```

and raises `RuntimeError` (with a confusing error message) on older versions of Python:

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from typing import NamedTuple
>>> class F(NamedTuple):
...     x: int
...     def method(self):
...         super()
...         
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    class F(NamedTuple):
    ...<2 lines>...
            super()
RuntimeError: __class__ not set defining 'F' as <class '__main__.F'>. Was __classcell__ propagated to type.__new__?
```

### Version

_No response_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-11-29 18:17_

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-29 19:19_

---

_Closed by @charliermarsh on 2025-11-30 15:49_

---
