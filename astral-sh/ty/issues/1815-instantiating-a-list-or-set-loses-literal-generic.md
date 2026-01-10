---
number: 1815
title: "Instantiating a `list` or `set` loses literal generic type information"
type: issue
state: open
author: Avasam
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-12-09T00:22:17Z
updated_at: 2025-12-20T01:57:02Z
url: https://github.com/astral-sh/ty/issues/1815
synced_at: 2026-01-10T01:52:52Z
---

# Instantiating a `list` or `set` loses literal generic type information

---

_Issue opened by @Avasam on 2025-12-09 00:22_

### Summary

Feel free to rename or mark as duplicate if there's a pre-existing issue that better describes the root cause. The closest related I found was maybe https://github.com/astral-sh/ty/issues/1648 / https://github.com/astral-sh/ty/issues/1576

Passing a list of literals to `list` or `set` loses type information with ty. Whereas both mypy and pyright keep the literal generic types:
- https://mypy-play.net/?gist=698f5d996f794a1a02536cebb8fe47a1
- https://play.ty.dev/7f70524b-6288-4669-8627-62926ed7bad5
- [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2ANFCCQG4lUD68CJAsAFAAmJwKCwAUADwBcUSkgDOMANpFSFSvIBE5NbTUAjLVDUBjNQF0TASnE8oNuo2aU2iEmPPXb9Jq3Yvpc127ctnZejj7CMiQwATxAA)
<img width="501" height="272" alt="Image" src="https://github.com/user-attachments/assets/65b7c5ed-9c08-44ce-bf0c-99e19b3113c0" />

### Version

ty 0.0.1-alpha.32

---

_Renamed from "Instantiating a `list` or `set` looses literal generic type information" to "Instantiating a `list` or `set` loses literal generic type information" by @Avasam on 2025-12-09 00:24_

---

_Comment by @carljm on 2025-12-09 00:29_

@ibraheemdev I think we had previously discussed in https://discord.com/channels/1039017663004942429/1279201882337705994/1437881888789364838 that we should not promote in this case, because the argument `x` in `list(x)` is already an invariant type (`list`), so we should assume that if it is specialized to a literal type, that must originate from an explicit annotation (as it does in this case), or else we'd have promoted on the construction of that type.

Did we just not fully implement that plan yet? Do we have another issue tracking that?

---

_Label `bidirectional inference` added by @carljm on 2025-12-09 00:29_

---

_Label `generics` added by @carljm on 2025-12-09 00:29_

---

_Assigned to @ibraheemdev by @carljm on 2025-12-09 00:29_

---

_Added to milestone `Stable` by @carljm on 2025-12-09 00:30_

---

_Comment by @ibraheemdev on 2025-12-10 20:17_

`list` takes an `Iterable[T]` argument, so I believe this is just https://github.com/astral-sh/ty/issues/1576, and should be fixed by https://github.com/astral-sh/ruff/pull/21747.

---

_Closed by @ibraheemdev on 2025-12-10 20:40_

---

_Comment by @carljm on 2025-12-10 20:54_

It doesn't look like https://github.com/astral-sh/ruff/pull/21747 changes the behavior of the OP example here, so there must be something additional going on. We can revisit after landing that one, though.

---

_Reopened by @carljm on 2025-12-10 20:54_

---

_Comment by @ibraheemdev on 2025-12-10 21:19_

Ah right, the issue here is unrelated. We consider the variance of the type parameter in the *argument* type, not parameter type, for literal promotion. There seems to be something else going on here:
```py
from typing import Iterable, Literal
from collections.abc import MutableSequence

class X[T](MutableSequence[T]):
    def __init__(self, iterable: Iterable[T], /) -> None: ...

class Y[T]:
    def __init__(self, iterable: Iterable[T], /) -> None: ...

def _(x: list[Literal[1]]):
    reveal_type(X(x))  # revealed: X[int]
    reveal_type(Y(x))  # revealed: Y[Literal[1]]
```

---

_Comment by @ibraheemdev on 2025-12-20 01:57_

It looks like we actually do consider the variance in the parameter type instead of the argument type, and this was just hidden by the tests using a bivariant type.

---
