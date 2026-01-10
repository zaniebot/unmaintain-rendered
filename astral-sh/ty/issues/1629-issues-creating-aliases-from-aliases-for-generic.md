```yaml
number: 1629
title: Issues Creating Aliases from Aliases for Generic Classes
type: issue
state: closed
author: RainSunGone
labels: []
assignees: []
created_at: 2025-11-25T10:41:01Z
updated_at: 2025-11-28T19:38:25Z
url: https://github.com/astral-sh/ty/issues/1629
synced_at: 2026-01-10T01:58:59Z
```

# Issues Creating Aliases from Aliases for Generic Classes

---

_Issue opened by @RainSunGone on 2025-11-25 10:41_

### Summary

Having issues when I have a class with multiple type arguments.
I can create some alias if I specify all the type arguments, however, if I leave one as a TypeVar then attempt to create a new alias from this alias, ty doesn't like it.
Mypy and my IDE seem happy with this approach.

```
from typing import Generic, TypeVar

T = TypeVar("T")
U = TypeVar("U")


class MyClass(Generic[T, U]):
    pass


IntStrClass = MyClass[int, str]  # This is fine

V = TypeVar("V")
IntAnyClass = MyClass[int, V]
IntStrClass2 = IntAnyClass[str]  # This gets non-subscriptable error

W = TypeVar("W")
AnyStrClass = MyClass[W, str]
IntStrClass3 = AnyStrClass[int]  # This gets non-subscriptable error

```

[Playground Link](https://play.ty.dev/d3896822-6e8e-4904-a3df-65285f1b3ef4)

All pyproject.toml settings are default and we're just running a simple `ty check`.

The obvious workaround is to just have every alias defined directly on MyClass, but it would be nice to allow this sugar as it helps minimize code. And we can use the "partial aliases" throughout the code where we do not know one of the Generic type arguments upfront.

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @AlexWaygood on 2025-11-25 10:47_

Thank you. Please see https://github.com/astral-sh/ty/issues/1596. This should be fixed by https://github.com/astral-sh/ruff/pull/21553

---

_Closed by @AlexWaygood on 2025-11-25 10:47_

---

_Comment by @sharkdp on 2025-11-25 11:06_

> This should be fixed by [astral-sh/ruff#21553](https://github.com/astral-sh/ruff/pull/21553)

Yes, I just confirmed that it will be.

---

_Closed by @sharkdp on 2025-11-28 19:38_

---
