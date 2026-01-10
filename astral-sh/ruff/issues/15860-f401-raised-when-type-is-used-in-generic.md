```yaml
number: 15860
title: F401 raised when type is used in Generic
type: issue
state: closed
author: vovchic17
labels:
  - bug
assignees: []
created_at: 2025-01-31T20:58:40Z
updated_at: 2025-02-01T17:05:23Z
url: https://github.com/astral-sh/ruff/issues/15860
synced_at: 2026-01-10T11:09:57Z
```

# F401 raised when type is used in Generic

---

_Issue opened by @vovchic17 on 2025-01-31 20:58_

### Description

## Ruff version
```
ruff 0.9.4 (854ab0307 2025-01-30)
```
## Code snippet
```python
from typing import TYPE_CHECKING, Generic, TypeVar
if TYPE_CHECKING:
    from asyncio.queues import Queue  # <- F401 raised here

T = TypeVar("T")

class Example(Generic[T]): ...

class Derived(Example["Queue"]): ...
```
## `ruff.toml` config
```toml
lint.select = ["F401", "TC004"]
```
## Command I invoked
```bash
$ ruff check
```
## Command output
```
main.py:3:32: F401 [*] `asyncio.queues.Queue` imported but unused
  |
1 | from typing import TYPE_CHECKING, Generic, TypeVar
2 | if TYPE_CHECKING:
3 |     from asyncio.queues import Queue  # <- F401 raised here
  |                                ^^^^^ F401
4 |
5 | T = TypeVar("T")
  |
  = help: Remove unused import: `asyncio.queues.Queue`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Label `bug` added by @ntBre on 2025-01-31 22:13_

---

_Comment by @ntBre on 2025-01-31 22:16_

Thanks for the report! This looks closely related to #15812, so possibly it can be fixed in a similar way by visiting subscripts in base classes as type expressions.

---

_Comment by @Daverball on 2025-01-31 23:00_

Without type inference it's going to be difficult to significantly improve the accuracy, since there's no way to tell if a subscript is a generic subscript without knowing the type that's being subscripted (and whether or not that type derives from `Generic` or `Protocol`). Ruff already employs some heuristics by at least recognizing all the standard library generic types, so the most common cases should be covered.

Special casing base class expressions seems relatively safe though, as far as additional heuristics beyond deeper type inference/analysis go. But I'm also not fully convinced that this comes up often enough to actually warrant a special case.

---

_Comment by @AlexWaygood on 2025-02-01 17:05_

Thanks for the report! Closing as a duplicate of https://github.com/astral-sh/ruff/issues/9298, however. See https://github.com/astral-sh/ruff/issues/9298#issuecomment-1871157183 for a suggested fix :-)

@ntBre is correct that visiting subscripts in base classes as type expressions rather than value expressions would "fix" this, but unfortunately it wouldn't be correct to do so -- some examples are given in https://github.com/astral-sh/ruff/issues/9298#issuecomment-1871157183 for why.

See also https://github.com/astral-sh/ruff/issues/13181, https://github.com/astral-sh/ruff/issues/15560, https://github.com/astral-sh/ruff/issues/12780 and https://github.com/astral-sh/ruff/issues/4654

---

_Closed by @AlexWaygood on 2025-02-01 17:05_

---
