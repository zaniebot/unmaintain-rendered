```yaml
number: 1332
title: Issues with kwarg splats inside dictionary literals
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-10-10T10:51:31Z
updated_at: 2026-01-08T18:39:51Z
url: https://github.com/astral-sh/ty/issues/1332
synced_at: 2026-01-12T15:54:25Z
```

# Issues with kwarg splats inside dictionary literals

---

_@AlexWaygood_

### Summary

At runtime, any object can be `**`-unpacked if that object has a `.keys()` method and a `__getitem__` method:

```pycon
>>> from typing import KeysView
>>> class HasKeysAndGetItem:
...     def keys(self) -> KeysView[str]:
...         return {"foo": 42}.keys()
...     
...     def __getitem__(self, arg: str) -> int:
...         return 42
...         
>>> {**HasKeysAndGetItem()}
{'foo': 42}
```

Functions that have `**kwargs` parameters have the additional requirement that the keys of the mapping must be strings:

```pycon
>>> def f(**kwargs): ...
... 
>>> f(**HasKeysAndGetItem())
>>> f(**{42: 42})
Traceback (most recent call last):
  File "<python-input-7>", line 1, in <module>
    f(**{42: 42})
    ~^^^^^^^^^^^^
TypeError: keywords must be strings
```

Ty should attempt to emulate this exactly:
- All mappings used with `**` splats should be treated the same way, assuming they have a `keys` method and a `__getitem__` method
- Invalid `**` splats should be detected and should cause us to emit diagnostics

We currently get this right for the `**kwargs` function-call case, but not for `**` splats inside dictionary literals, where it appears we treat `dict`s different to other mappings and we do not emit diagnostics for invalid `**` splats:

```py
from typing import Mapping, KeysView

class HasKeysAndGetItem:
    def keys(self) -> KeysView[str]:
        return {}.keys()
    
    def __getitem__(self, arg: str) -> int:
        return 42

def h(**kwargs): ...

def f(
    a: dict[str, int],
    b: Mapping[str, int],
    c: HasKeysAndGetItem,
    d: object
):
    reveal_type({**a})  # dict[Unknown | str, Unknown | int] (good!)
    reveal_type({**b})  # dict[Unknown, Unknown]             (bad!)
    reveal_type({**c})  # dict[Unknown, Unknown]             (bad!)
    reveal_type({**d})  # dict[Unknown, Unknown]             (good, but no diagnostic emitted!)
    
    h(**a)
    h(**b)
    h(**c)
    h(**d)              # error: [invalid-argument-type] "Argument expression after ** must be a mapping type: Found `object`"
```

The logic for `**` splats passed to function calls looks correct to me -- I think we probably want to do exactly the same thing inside dictionary literals, _except_ that we don't need to check whether the type of the keys is assignable to `str`. So we may want to extract that logic out into a helper function somewhere: https://github.com/astral-sh/ruff/blob/69f918203359f834e0ca76764a502f3421b2c2c7/crates/ty_python_semantic/src/types/call/bind.rs#L2691-L2742

Cc. @ibraheemdev -- this looks related to https://github.com/astral-sh/ruff/pull/20523

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-10 10:51_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-10-10 10:54_

---

_Added to milestone `GA` by @carljm on 2025-10-10 16:25_

---

_Comment by @ibraheemdev on 2025-10-10 21:45_

Dictionary unpacking expressions [are special-cased here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/infer/builder.rs#L5941), it shouldn't be too hard to generalize if we already have the infrastructure for function calls.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-11-14 08:06_

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 18:39_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 18:39_

---
