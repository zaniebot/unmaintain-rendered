```yaml
number: 19863
title: "UP047 Fails to indentify functions that have `Callable` in their signature"
type: issue
state: open
author: Kroppeb
labels:
  - bug
  - rule
assignees: []
created_at: 2025-08-11T13:12:06Z
updated_at: 2025-08-11T14:22:31Z
url: https://github.com/astral-sh/ruff/issues/19863
synced_at: 2026-01-10T11:09:59Z
```

# UP047 Fails to indentify functions that have `Callable` in their signature

---

_Issue opened by @Kroppeb on 2025-08-11 13:12_

### Summary

I noticed something weird with UP047, `foo` triggers a warning while `foo2` does not.

```python
from collections.abc import Callable
from typing import TypeVar

T = TypeVar("T")


def foo(t: list[T]) -> T:
    return t[0]


def foo2(t: list[T], c: Callable[[T], None]) -> T:
    c(t[0])
    return t[1]
```

Playground: https://play.ruff.rs/cd7af9a8-0532-40fe-83c7-4282a658808c

### Version

v0.12.7

---

_Label `bug` added by @ntBre on 2025-08-11 13:17_

---

_Label `rule` added by @ntBre on 2025-08-11 13:18_

---

_Comment by @MichaReiser on 2025-08-11 14:05_

Thanks for opening an issue. This was also discussed in our Discord ([conversation](https://discord.com/channels/1039017663004942429/1070132471699607623/1402996211404902531)). 


Specifically, @AlexWaygood replied

> There was a bug in collections.abc.Callable on Python 3.9 IIRC, which means it isn't safe to replace typing.Callable with collections.abc.Callable on all Python versions (it works most of the time on py39, but not in some edge cases, I think?) 
Should be safe on the newest Python versions, though. Can't remember exactly when the bug was fixed

---

_Comment by @ntBre on 2025-08-11 14:20_

I talked to Alex, and I don't think that Python bug should affect this rule, which is only replacing the type of generics, not importing `Callable` from a different module. I believe he was thinking of [deprecated-import (UP035)](https://docs.astral.sh/ruff/rules/deprecated-import/#deprecated-import-up035), which will replace `typing.Callable` with `collections.abc.Callable` after Python 3.10.

---

_Comment by @AlexWaygood on 2025-08-11 14:22_

Yup, @ntBre is correct — my bad! sorry about that 🫠

---
