---
number: 14328
title: "`subclass-builtin` (`FURB189`) - document exactly why `UserDict`, `UserList` etc are preferred over `dict` and `list`"
type: issue
state: closed
author: DetachHead
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-11-14T00:30:00Z
updated_at: 2025-03-23T17:24:41Z
url: https://github.com/astral-sh/ruff/issues/14328
synced_at: 2026-01-10T01:22:55Z
---

# `subclass-builtin` (`FURB189`) - document exactly why `UserDict`, `UserList` etc are preferred over `dict` and `list`

---

_Issue opened by @DetachHead on 2024-11-14 00:30_

[the documentation for this rule](https://docs.astral.sh/ruff/rules/subclass-builtin) says that subclassing the builtins is "error prone" but doesn't provide an example of such an error.

attempting to find information about these "User" classes and why they exist is difficult. [the official docs](https://docs.python.org/3/library/collections.html#collections.UserDict) don't explain why it exists, and as usual, stackoverflow is full of [outdated information from python 2 that says *not* to use it](https://stackoverflow.com/questions/1392396/advantages-of-userdict-class)

---

_Label `documentation` added by @dylwil3 on 2024-11-14 00:42_

---

_Label `good first issue` added by @dylwil3 on 2024-11-14 00:42_

---

_Comment by @dylwil3 on 2024-11-14 05:55_

Maybe this is still up to date? (I didn't look closely): https://realpython.com/inherit-python-dict/

---

_Comment by @DetachHead on 2024-11-14 06:29_

thanks. there's a lot of filler in that article so i'll summarize. it's because `dict` does not use its own dunders which makes it more difficult to correctly change its functionality. for example if you want to create a dict where all keys are converted to uppercase, overriding `__setitem__` is not sufficient because `dict.update` does not use it:

```py
from collections import UserDict
from typing import override


class UppercaseDict[T](dict[str, T]):
    def __init__(self, value: dict[str, T]):
        super().__init__({key.upper(): value for key, value in value.items()})

    @override
    def __setitem__(self, key: str, value: T, /):
        return super().__setitem__(key.upper(), value)


builtin_dict = UppercaseDict({"a": 1})
builtin_dict.update({"b": 2})
# wrong
print(builtin_dict)  # {'A': 1, 'b': 2}


class UppercaseUserDict[T](UserDict[str, T]):
    def __init__(self, value: dict[str, T]):
        super().__init__({key.upper(): value for key, value in value.items()})

    @override
    def __setitem__(self, key: str, value: T, /):
        return super().__setitem__(key.upper(), value)


user_dict = UppercaseUserDict({"a": 1})
user_dict.update({"b": 2})
# correct
print(user_dict)  # {'A': 1, 'B': 2}
```

---

_Comment by @dan-wilton on 2025-03-23 14:47_

Looking for a good place to get started in contributing to `ruff`, this looks like it! Will update the docs now ❤ 

---

_Referenced in [astral-sh/ruff#16927](../../astral-sh/ruff/pulls/16927.md) on 2025-03-23 15:24_

---

_Closed by @ntBre on 2025-03-23 17:24_

---
