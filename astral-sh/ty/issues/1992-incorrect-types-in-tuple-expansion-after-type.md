```yaml
number: 1992
title: Incorrect types in tuple expansion after type narrowing
type: issue
state: closed
author: judahrand
labels: []
assignees: []
created_at: 2025-12-17T09:44:33Z
updated_at: 2025-12-17T10:48:11Z
url: https://github.com/astral-sh/ty/issues/1992
synced_at: 2026-01-10T01:55:00Z
```

# Incorrect types in tuple expansion after type narrowing

---

_Issue opened by @judahrand on 2025-12-17 09:44_

### Summary

```python
from typing import reveal_type


class SomeClass:
    pass


def func() -> tuple[SomeClass, str, int] | SomeClass:
    return SomeClass(), "hello", 2


res = func()
if isinstance(res, SomeClass):
    reveal_type(res)  # Expected: SomeClass
else:
    model, s, i = res
    reveal_type(res)  # Expected: tuple[SomeClass, str, int]
    reveal_type(model)  # Expected: SomeClass
    reveal_type(s)  # Expected: str
    reveal_type(i)  # Expected: int
```

<img width="693" height="378" alt="Image" src="https://github.com/user-attachments/assets/a05eefa6-d1a0-41d8-92b7-fdcc34b2ac5d" />

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @judahrand on 2025-12-17 09:45_

@AlexWaygood 

Ah I think duplicate of https://github.com/astral-sh/ty/issues/1978

---

_Closed by @judahrand on 2025-12-17 09:45_

---
