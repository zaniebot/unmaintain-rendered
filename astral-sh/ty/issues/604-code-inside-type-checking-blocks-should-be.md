```yaml
number: 604
title: "code inside `TYPE_CHECKING` blocks should be treated like stubs"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2025-06-07T13:41:18Z
updated_at: 2025-06-07T13:50:40Z
url: https://github.com/astral-sh/ty/issues/604
synced_at: 2026-01-12T15:54:23Z
```

# code inside `TYPE_CHECKING` blocks should be treated like stubs

---

_@DetachHead_

```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
   def foo() -> int: ... # Function always implicitly returns `None`, which is not assignable to return type `int` (invalid-return-type)
```
https://play.ty.dev/cd6ee5d1-98e2-48b8-be7c-6bfcda4bbbd1

there's no point adding an implementation here to fix the error because it doesn't actually get executed at runtime, so it would be nice if ty treated it the same as a `.pyi` file

---

_Closed by @AlexWaygood on 2025-06-07 13:50_

---
