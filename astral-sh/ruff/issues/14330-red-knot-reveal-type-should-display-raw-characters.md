```yaml
number: 14330
title: "[red-knot] `reveal_type` should display raw characters"
type: issue
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
created_at: 2024-11-14T05:19:22Z
updated_at: 2024-11-15T08:14:05Z
url: https://github.com/astral-sh/ruff/issues/14330
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] `reveal_type` should display raw characters

---

_@dhruvmanila_

Looking at something like:
```py
from typing import Literal, reveal_type

x: Literal["\n"] = "\n"

reveal_type(x)
```

We get:
```
red-knot: Revealed type is `Literal["
  "]` [revealed-type]
Pyright: Type of "e" is "Literal['\n']"
mypy: Revealed type is "Literal['\n']"
```

I think it would be useful to not render the characters and instead display it as raw instead similar to Pyright and mypy.

---

_Label `red-knot` added by @dhruvmanila on 2024-11-14 05:19_

---

_Comment by @MichaReiser on 2024-11-14 06:19_

We could try using https://doc.rust-lang.org/std/primitive.str.html#method.escape_debug when displaying Literal

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-11-14 07:18_

---

_Closed by @dhruvmanila on 2024-11-15 08:14_

---

_Closed by @dhruvmanila on 2024-11-15 08:14_

---
