---
number: 2315
title: Losing generic type information when using list on Iterable
type: issue
state: closed
author: AndBoyS
labels: []
assignees: []
created_at: 2026-01-03T12:24:16Z
updated_at: 2026-01-03T12:32:38Z
url: https://github.com/astral-sh/ty/issues/2315
synced_at: 2026-01-10T01:51:14Z
---

# Losing generic type information when using list on Iterable

---

_Issue opened by @AndBoyS on 2026-01-03 12:24_

### Summary

```
from typing_extensions import reveal_type
from typing import TypeVar
from collections.abc import Iterable

T = TypeVar("T")


def func(it: Iterable[T]):
    reveal_type(list(it))  # Revealed type: list[Unknown]
```

Python version 3.10

### Version

0.0.8

---

_Comment by @AlexWaygood on 2026-01-03 12:32_

Thanks! This is actually already fixed on main; a fix will be included in the next release

---

_Closed by @AlexWaygood on 2026-01-03 12:32_

---
