```yaml
number: 19510
title: "[ty] Infer single-valuedness for enums based on `int`/`str`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/int-and-str-enums
created_at: 2025-07-23T13:28:33Z
updated_at: 2025-07-23T13:55:45Z
url: https://github.com/astral-sh/ruff/pull/19510
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Infer single-valuedness for enums based on `int`/`str`

---

_Pull request opened by @sharkdp on 2025-07-23 13:28_

## Summary

We previously didn't recognize `Literal[Color.RED]` as single-valued, if the enum also derived from `str` or `int`:
```py
from enum import Enum

class Color(str, Enum):
    RED = "red"
    GREEN = "green"
    BLUE = "blue"

def _(color: Color):
    if color == Color.RED:
        reveal_type(color)  # previously: Color, now: Literal[Color.RED]
```

The reason for that was that `int` and `str` have "custom" `__eq__` and `__ne__` implementations that return `bool`. We do not treat enum literals from classes with custom `__eq__` and `__ne__` implementations as single-valued, but of course we know that `int.__eq__` and `str.__eq__` are well-behaved.

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-07-23 13:28_

---

_Renamed from "[ty] Infer single-valuedness for enums based on int/str" to "[ty] Infer single-valuedness for enums based on `int`/`str`" by @sharkdp on 2025-07-23 13:28_

---

_Comment by @github-actions[bot] on 2025-07-23 13:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-07-23 13:34_

---

_Review requested from @carljm by @sharkdp on 2025-07-23 13:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-23 13:34_

---

_Review requested from @dcreager by @sharkdp on 2025-07-23 13:34_

---

_@AlexWaygood approved on 2025-07-23 13:52_

I was also wondering if we could generalize this logic so that it applies to all instances of `@final` classes that do not override `__eq__`/`__ne__` from `object`/`int`/`str` (not just instances of enum classes).

It's really the finality of enum classes that allows us to safely determine whether instances of enums are single-valued or not: because an enum class is final, we know that there are no possible subclasses of the enum class, therefore there are no possible subtypes of the enum type that could have badly behaved `__eq__` or `__ne__` methods. And that reasoning applies to any final class, not just enums

---

_Merged by @sharkdp on 2025-07-23 13:55_

---

_Closed by @sharkdp on 2025-07-23 13:55_

---

_Branch deleted on 2025-07-23 13:55_

---
