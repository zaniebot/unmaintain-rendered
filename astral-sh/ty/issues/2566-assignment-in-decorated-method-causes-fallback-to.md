```yaml
number: 2566
title: "Assignment in decorated method causes fallback to `Unknown`"
type: issue
state: open
author: philj56
labels: []
assignees: []
created_at: 2026-01-19T17:03:34Z
updated_at: 2026-01-20T14:00:30Z
url: https://github.com/astral-sh/ty/issues/2566
synced_at: 2026-01-20T14:40:26Z
```

# Assignment in decorated method causes fallback to `Unknown`

---

_@philj56_

### Summary

If a member variable with an explicit type hint is assigned to in a method with any sort of decorator, `ty` falls back to typing the variable as `Unknown`, unless the decorated method also contains a type hint.

Example:
```python
from functools import cache
from typing import reveal_type


class WithoutDecorator:
    def __init__(self):
        self.x: int = 3

    def set_x(self) -> None:
        self.x = 4


class WithDecorator:
    def __init__(self):
        self.x: int = 3

    @cache
    def set_x(self) -> None:
        self.x = 4


class WithDecoratorAndAnnotation:
    def __init__(self):
        self.x: int = 3

    @cache
    def set_x(self) -> None:
        self.x: int = 4

reveal_type(WithoutDecorator().x)  # int
reveal_type(WithDecorator().x)  # Unknown | int
reveal_type(WithDecoratorAndAnnotation().x)  # int
```

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Renamed from "Assignment in PySide6 slots causes fallback to `Unknown`" to "Assignment in decorated method causes fallback to `Unknown`" by @philj56 on 2026-01-20 13:57_

---
