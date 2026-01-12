```yaml
number: 1800
title: "false positive `unsupported-base` for self-referential generic base"
type: issue
state: closed
author: jorenham
labels:
  - bug
assignees: []
created_at: 2025-12-07T20:39:55Z
updated_at: 2025-12-11T17:53:45Z
url: https://github.com/astral-sh/ty/issues/1800
synced_at: 2026-01-12T15:54:25Z
```

# false positive `unsupported-base` for self-referential generic base

---

_@jorenham_

### Summary

There are several examples of this in [optype](https://github.com/jorenham/optype), and this is a (stripped down version of) one of them ([src](https://github.com/jorenham/optype/blob/2d03b31e0ce61c6fa827a9753ea131e2aada527a/optype/copy.py#L31-L43)):

```py
from typing import Protocol, override, Self

class CanCopy[T](Protocol):
    def __copy__(self, /) -> T: ...

class CanCopySelf(CanCopy["CanCopySelf"], Protocol):
    @override
    def __copy__(self, /) -> Self: ...
```

```
Unsupported class base with type `<class 'CanCopy[CanCopySelf]'> | <class 'CanCopy[CanCopySelf]'>` (unsupported-base) [Ln 6, Col 19]
```

which runs just fine at runtime:

```pycon
>>> from typing import Protocol, override, Self
... 
... class CanCopy[T](Protocol):
...     def __copy__(self, /) -> T: ...
... 
... class CanCopySelf(CanCopy["CanCopySelf"], Protocol):
...     @override
...     def __copy__(self, /) -> Self: ...
...     
>>> # ¯\_(ツ)_/¯
```

### Version

ty 0.0.1-alpha.32

---

_Comment by @AlexWaygood on 2025-12-07 22:01_

This is probably https://github.com/astral-sh/ty/issues/1732. I'll check tomorrow if it repros prior to https://github.com/astral-sh/ruff/commit/2c0c5ff4e7a0f347e81bdcdb6e38180dcee0688b landing (if not, it's almost certainly #1732)

---

_Label `bug` added by @AlexWaygood on 2025-12-07 22:12_

---

_Comment by @AlexWaygood on 2025-12-07 22:18_

This is a bug in ty; we should support this (we shouldn't "need" to infer a union type there at all). But with regards to

> which runs just fine at runtime

note that the error code was `unsupported-base` rather than `invalid-base`. `invalid-base` is about things that will cause runtime errors; `unsupported-base` is specifically about patterns that will not cause errors at runtime but which ty cannot support:

- https://docs.astral.sh/ty/reference/rules/#invalid-base
- https://docs.astral.sh/ty/reference/rules/#unsupported-base

---

_Added to milestone `Beta` by @carljm on 2025-12-08 17:41_

---

_Comment by @carljm on 2025-12-11 00:13_

Will be fixed by https://github.com/astral-sh/ruff/pull/21909

---

_Assigned to @carljm by @carljm on 2025-12-11 00:13_

---

_Closed by @carljm on 2025-12-11 17:53_

---
