```yaml
number: 587
title: Cannot infer type of a generic Callable return
type: issue
state: closed
author: lczyk
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-05T17:21:21Z
updated_at: 2025-06-05T23:20:17Z
url: https://github.com/astral-sh/ty/issues/587
synced_at: 2026-01-12T15:54:23Z
```

# Cannot infer type of a generic Callable return

---

_@lczyk_

### Summary

```python
from typing_extensions import reveal_type
from typing import Callable, TypeVar

_T = TypeVar("_T")

def f(fun: Callable[[], _T]) -> _T:
    v = fun()
    return v

def g() -> int:
    return 42

reveal_type(g)
reveal_type(f(g))
```

```sh
$ mypy mwe.py
mwe.py:13: note: Revealed type is "def () -> builtins.int"
mwe.py:14: note: Revealed type is "builtins.int"
```

```sh
$ ty check mwe.py
info[revealed-type] mwe.py:13:13: Revealed type: `def g() -> int`
info[revealed-type] mwe.py:14:13: Revealed type: `Unknown`
```

aka `ty` seems to not be able to infer the return type of `f` correctly

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `generics` added by @AlexWaygood on 2025-06-05 17:22_

---

_Label `bug` added by @AlexWaygood on 2025-06-05 17:22_

---

_Comment by @lczyk on 2025-06-05 17:23_

The specific codebase i'm working on: https://github.com/MarcinKonowalczyk/type_conversion_dict. There are a couple more snags i found along the way. Will post later 👍

---

_Comment by @MatthewMckee4 on 2025-06-05 19:57_

This looks like a duplicate of #500

---

_Closed by @carljm on 2025-06-05 20:25_

---

_Comment by @lczyk on 2025-06-05 23:07_

> There are a couple more snags i found along the way. Will post later

1) a duplicate of #220, temporarily patched with `# type: ignore[unresolved-reference, unused-ignore]` (unused-ignore to stop mypy from complaining. i've +1'ed the issue

2) i think its a more subtle version of the same case as i've posted here, but i'm not sure. here is a mwe:

```python
from typing import TypeVar
from typing_extensions import reveal_type

_K = TypeVar("_K")
_V = TypeVar("_V")

class MyDict(dict[_K, _V]):
    pass

a = MyDict(foo=1)
reveal_type(a)
```

which gives

```
error[no-matching-overload] mwe.py:13:5: No overload of bound method `__init__` matches arguments
info[revealed-type] mwe.py:14:13: Revealed type: `MyDict[Unknown, Unknown]`
```

and works with mypy.

essentially MyDict does not see the inherited __init__. I am not sure this is necessarily anything to do with generics, but I did not get it to fire in a simpler case. i'd appreciate if someone with deeper knowledge could triage this as either _'indeed again duplicate of #500'_ or _'actually, might be smth else`_.


---

_Comment by @carljm on 2025-06-05 23:20_

@MarcinKonowalczyk that one looks new to me! I created https://github.com/astral-sh/ty/issues/588 based on your comment, thank you!

---
