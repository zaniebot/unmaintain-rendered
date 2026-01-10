```yaml
number: 2435
title: total_ordering when mixed with literals fails
type: issue
state: open
author: behrmann
labels: []
assignees: []
created_at: 2026-01-10T12:26:52Z
updated_at: 2026-01-10T12:26:52Z
url: https://github.com/astral-sh/ty/issues/2435
synced_at: 2026-01-10T12:50:07Z
```

# total_ordering when mixed with literals fails

---

_Issue opened by @behrmann on 2026-01-10 12:26_

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
