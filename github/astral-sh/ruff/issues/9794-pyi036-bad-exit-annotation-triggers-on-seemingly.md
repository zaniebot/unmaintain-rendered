---
number: 9794
title: PYI036 (bad-exit-annotation) triggers on seemingly appropriate annotations
type: issue
state: open
author: Sachaa-Thanasius
labels:
  - rule
assignees: []
created_at: 2024-02-02T19:15:46Z
updated_at: 2025-07-09T14:37:27Z
url: https://github.com/astral-sh/ruff/issues/9794
synced_at: 2026-01-07T13:12:15-06:00
---

# PYI036 (bad-exit-annotation) triggers on seemingly appropriate annotations

---

_Issue opened by @Sachaa-Thanasius on 2024-02-02 19:15_

**Code snippet**
For additional context: Importing from `types` outside of the `TYPE_CHECKING` block doesn't trigger this. However, I'd prefer not actually importing that at runtime ~~since it can be expensive~~. Curious if this kind of case is in scope for this rule.

```py
from typing import TYPE_CHECKING

from typing_extensions import Self


if TYPE_CHECKING:
    from types import TracebackType
else:

    class TracebackType:
        pass


class Example:
    def __enter__(self) -> Self:
        return self

    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_val: BaseException | None,
        traceback: TracebackType | None,
) -> None:
        return None
```

**Invoked Command**
```bash
ruff core\utils\_test.py --isolated --select PYI036
```

Output:
```bash
core\utils\_test.py:22:20: PYI036 The third argument in `__exit__` should be annotated with `object` or `types.TracebackType | None`
Found 1 error.
```

**Relevant pyproject.toml Sections**
Use of the `--isolated` and `--select` flags above makes this redundant, I think? If not, I will update to include them, though the only seemingly relevant thing is `PYI`'s presence in the `select` list.

**Version**
ruff 0.2.0

---

_Label `rule` added by @AlexWaygood on 2024-02-02 20:08_

---

_Comment by @T-256 on 2024-02-02 20:20_

In your case I think you could also do:

```py
from __future__ import annotations
from typing import TYPE_CHECKING

from typing_extensions import Self

if TYPE_CHECKING:
    from types import TracebackType


class Example:
    def __enter__(self) -> Self:
        return self

    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_val: BaseException | None,
        traceback: TracebackType | None,
) -> None:
        return None
```

---

_Comment by @Sachaa-Thanasius on 2024-02-02 23:43_

While that's true, I would rather not break type introspection for tools like `typing.get_type_hints()`. Having something in the type checking block's else clause prevents that, even if the type isn't exactly right. Hoping there's a "best of both worlds" option here, where I don't break that and also have ruff's rule recognize what's happening here. If not, though, I understand.

---

_Comment by @CarrotManMatt on 2024-11-27 00:39_

I am running into this bug when importing the types inside a type-checking block & using quoted annotations rather than `from __future__ import annotations`. A resolution for this bug would be appreciated

---
