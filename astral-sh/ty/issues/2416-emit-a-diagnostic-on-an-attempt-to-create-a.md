---
number: 2416
title: "Emit a diagnostic on an attempt to create a \"generic enum\""
type: issue
state: open
author: AlexWaygood
labels:
  - runtime semantics
  - typing semantics
assignees: []
created_at: 2026-01-09T14:06:52Z
updated_at: 2026-01-09T19:18:37Z
url: https://github.com/astral-sh/ty/issues/2416
synced_at: 2026-01-10T01:48:23Z
---

# Emit a diagnostic on an attempt to create a "generic enum"

---

_Issue opened by @AlexWaygood on 2026-01-09 14:06_

This class statement fails at runtime, but ty does not detect it:

```py
from enum import Enum

class Foo[T](Enum):
    X = list[T]()
```

Runtime:

```pytb
>>> from enum import Enum
... 
... class Foo[T](Enum):
...     X = list[T]()
...     
Traceback (most recent call last):
  File "<python-input-0>", line 3, in <module>
    class Foo[T](Enum):
        X = list[T]()
  File "<python-input-0>", line 3, in <generic parameters of Foo>
    class Foo[T](Enum):
        X = list[T]()
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 491, in __prepare__
    member_type, first_enum = metacls._get_mixins_(cls, bases)
                              ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 956, in _get_mixins_
    raise TypeError("new enumerations should be created as "
            "`EnumName([mixin_type, ...] [data_type,] enum_type)`")
TypeError: new enumerations should be created as `EnumName([mixin_type, ...] [data_type,] enum_type)`
```

This also fails in the same way, and again, we emit no error:

```py
from enum import Enum
from typing import Generic, TypeVar

T = TypeVar("T")

class Foo(Enum, Generic[T]):
    X = list[T]()
```

The only way you can get a "generic enum" to not immediately fail is by putting `Generic[T]` _first_ in the bases list (contrary to the usual recommendation that `Generic[T]` should always be the last base class). But even if you do this, things do not work at runtime in the way you'd expect -- it's illegal to specialize such a class at runtime, because `Generic.__class_getitem__` is totally clobbered by `EnumMeta.__getitem__`:

```pytb
>> from enum import Enum
>>> from typing import Generic, TypeVar
>>> T = TypeVar("T")
>>> class Foo(Generic[T], Enum):
...     X = list[T]()
...     
>>> Foo[int]
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    Foo[int]
    ~~~^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/enum.py", line 789, in __getitem__
    return cls._member_map_[name]
           ~~~~~~~~~~~~~~~~^^^^^^
KeyError: <class 'int'>
```

Aside from whether or not it works at runtime, the concept of a generic enum just isn't very coherent on a conceptual level anyway. We currently don't permit them to exist in our internal model, and mypy/pyrefly both emit diagnostics if a user tries to create one:
- https://mypy-play.net/?mypy=latest&python=3.12&gist=35ef91038c460dd48cd557bb8704076d
- https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BxGOhiUIAYwA0dACrMYANVSUAOujDV6wgK70uPPnQCi6XWrUy6AXlnyllABQqQM5wEpz6cVFRw4dADFcXAchETFxAG0ZAF1pE103RDU6VLp8RFk1EEkQbQZoOBJyRBAAYjoAVQKoCCY6MG0vAtx0OE9MGDAG3hpUBgB9UxpsUQcMznQGNzoAWgA%2BOjgGSmT0NLpKGAZtSnWwZwA5XVHVumB8AF9nbNyyLbAoUkIGWigKCoAFUgenpYwcAQ6OJWpA2Lt%2BhBWoQ1BUAMowGB0AAWDAYxDgiAA9Fj7l0noReGwscIsZhcOI4FiQeoIODKJDWliepQ6KgAG6oaCobCwYGgukQlrrXDEYVFNRkBjI1qzdmiOBQ9Y2ZwAZkIAEYAEw3dAgS65VDiArygLQGAUNBYPBEMj6oA

We should do the same as mypy/pyrefly here.

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-09 14:06_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-09 14:14_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-09 14:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 19:18_

---
