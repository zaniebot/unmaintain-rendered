```yaml
number: 920
title: Incorrect inference of equality operation between literal enum types
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-07-31T14:07:43Z
updated_at: 2025-08-18T17:45:45Z
url: https://github.com/astral-sh/ty/issues/920
synced_at: 2026-01-10T02:06:24Z
```

# Incorrect inference of equality operation between literal enum types

---

_Issue opened by @AlexWaygood on 2025-07-31 14:07_

### Summary

```py
from enum import Enum

class Foo(Enum):
    A = 1
    B = 2

    def __eq__(self, other) -> bool:
        return False

reveal_type(Foo.A == Foo.A)
```

We reveal `Literal[True]` here, but at runtime the answer is `False`. The bug lies here:

https://github.com/astral-sh/ruff/blob/a3f28baab4fc085e1448484be23df7036d13191d/crates/ty_python_semantic/src/types/infer.rs#L7896-L7905

It's only safe to do the kind of precise type inference we're attempting there if we know that `__eq__` and `__ne__` have not been overridden on the enum class. I believe we already check this correctly in other places, e.g. when doing type narrowing

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-07-31 14:07_

---

_Label `good first issue` added by @sharkdp on 2025-07-31 14:20_

---

_Label `help wanted` added by @carljm on 2025-08-05 02:21_

---

_Closed by @sharkdp on 2025-08-18 17:45_

---
