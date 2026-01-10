---
number: 13602
title: "`UP043` only applies to `typing.Generator`, not `collections.abc.Generator`"
type: issue
state: closed
author: cake-monotone
labels:
  - bug
assignees: []
created_at: 2024-10-02T11:06:59Z
updated_at: 2024-10-03T12:06:16Z
url: https://github.com/astral-sh/ruff/issues/13602
synced_at: 2026-01-10T01:22:53Z
---

# `UP043` only applies to `typing.Generator`, not `collections.abc.Generator`

---

_Issue opened by @cake-monotone on 2024-10-02 11:06_

### UP043
> `UP043`
> Python 3.13 introduced the ability for type parameters to specify default values. As such, the default type arguments for some types in the standard library (e.g., Generator, AsyncGenerator) are now optional.
> Omitting type parameters that match the default values can make the code more concise and easier to read.

```py
Generator[int, None, None] -> Generator[int]
```

### issue

Currently, UP043 only applies to typing.Generator, but it should also support collections.abc.Generator.

### Example 
```py
from collections.abc import Generator
from typing import Generator as tG

a = Generator[int, None, None] # No warns
b = tG[int, None, None]  # UP043
```
https://play.ruff.rs/e09a4a44-7af6-4b17-ab37-ce0ba2d1ccf6

---

_Comment by @cake-monotone on 2024-10-02 11:11_

May I work on this one? 

---

_Comment by @AlexWaygood on 2024-10-02 13:59_

> May I work on this one?

Sure!

---

_Assigned to @cake-monotone by @AlexWaygood on 2024-10-02 13:59_

---

_Label `bug` added by @AlexWaygood on 2024-10-02 13:59_

---

_Referenced in [astral-sh/ruff#13611](../../astral-sh/ruff/pulls/13611.md) on 2024-10-03 11:59_

---

_Closed by @charliermarsh on 2024-10-03 12:06_

---

_Closed by @charliermarsh on 2024-10-03 12:06_

---
