```yaml
number: 2435
title: "`total_ordering` generated methods should derive their signature from the user-provided comparison method"
type: issue
state: closed
author: behrmann
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2026-01-10T12:26:52Z
updated_at: 2026-01-10T19:36:00Z
url: https://github.com/astral-sh/ty/issues/2435
synced_at: 2026-01-12T15:54:26Z
```

# `total_ordering` generated methods should derive their signature from the user-provided comparison method

---

_@behrmann_

### Summary

Thanks for adding `@total_ordering` support with [ruff#22181](https://github.com/astral-sh/ruff/pull/22181)!

Adapting the motivating example from #1202, mixing this with literals does not yet work:
```python
import functools


@functools.total_ordering
class Comparable:
    def __init__(self, value: int):
        self.value = value

    def __eq__(self, other: object) -> bool:
        if isinstance(other, Comparable):
            return self.value == other.value
        if isinstance(other, int):
            return self == Comparable(other)
        return False

    def __lt__(self, other: object) -> bool:
        if isinstance(other, Comparable):
            return self.value < other.value
        if isinstance(other, int):
            return self < Comparable(other)
        return False

    # # Uncommenting this implementation satisfies ty
    # def __le__(self, other) -> bool:
    #     if isinstance(other, Comparable):
    #         return self.value <= other.value
    #     if isinstance(other, int):
    #         return self <= Comparable(other)
    #     return False



a = Comparable(1)
b = 2

print(a <= b)
```

Both mypy and pyright accept this code. In mkosi code similar to this is used for [version checking](https://github.com/systemd/mkosi/blob/main/mkosi/versioncomp.py), where the underlying algorithm works on strings, but version objects, strings and integers need to be covered.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Comment by @AlexWaygood on 2026-01-10 12:33_

Thank you!

Cc. @charliermarsh. The generated `__le__` method currently has a different signature to the user-provided `__lt__` method, but it should have the same signature:

```py
import functools

@functools.total_ordering
class Comparable:
    def __init__(self, value: int):
        self.value = value

    def __eq__(self, other: object) -> bool:
        if isinstance(other, Comparable):
            return self.value == other.value
        if isinstance(other, int):
            return self == Comparable(other)
        return False

    def __lt__(self, other: object) -> bool:
        if isinstance(other, Comparable):
            return self.value < other.value
        if isinstance(other, int):
            return self < Comparable(other)
        return False

reveal_type(Comparable.__lt__)  # revealed: `def __lt__(self, other: object) -> bool`
reveal_type(Comparable.__le__)  # revealed: `(self: Comparable, other: Comparable) -> bool`

a = Comparable(1)
b = 2

print(a <= b)
```

---

_Label `bug` added by @AlexWaygood on 2026-01-10 12:33_

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-10 12:33_

---

_Renamed from "total_ordering when mixed with literals fails" to "`total_ordering` generated methods should derive their signature from the user-provided comparison method" by @AlexWaygood on 2026-01-10 12:34_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-10 12:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-10 13:04_

---

_Comment by @charliermarsh on 2026-01-10 13:04_

Will fix!

---

_Closed by @charliermarsh on 2026-01-10 19:35_

---
