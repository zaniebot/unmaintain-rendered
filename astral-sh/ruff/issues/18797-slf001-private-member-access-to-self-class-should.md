```yaml
number: 18797
title: SLF001 (private member access) to self-class should work with type narrowing
type: issue
state: open
author: sliedes
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-06-19T17:42:56Z
updated_at: 2025-06-20T16:07:44Z
url: https://github.com/astral-sh/ruff/issues/18797
synced_at: 2026-01-12T15:54:56Z
```

# SLF001 (private member access) to self-class should work with type narrowing

---

_@sliedes_

### Summary

Consider this code. Here, `use_other1` does not cause a rule violation, as expected. I would expect type narrowing in `use_other2` to result in the same result of no warning, but ruff warns with SLF001.

ruff 0.12.0.

```python
# ruff: noqa: D100, D101, D102, D107

from __future__ import annotations


class Parent:
    _x: int


class Child(Parent):
    def __init__(self) -> None:
        self._x = 0

    def use_other1(self, other: Child) -> None:
        self._x = other._x  # ruff is OK with this

    def use_other2(self, other: Parent) -> None:
        if not isinstance(other, Child):
            raise TypeError("Expected an instance of Child")
        self._x = other._x  # ruff considers this SLF001 (Private member accessed)
```

---

_Comment by @sliedes on 2025-06-19 17:44_

Related looking issues and PRs: https://github.com/astral-sh/ruff/issues/17197 https://github.com/astral-sh/ruff/pull/16149

---

_Comment by @ntBre on 2025-06-20 12:43_

Thanks for the report! I think this may need type inference, at least in the general case, to resolve the subclass relationship.

---

_Label `rule` added by @ntBre on 2025-06-20 12:43_

---

_Label `type-inference` added by @ntBre on 2025-06-20 12:43_

---

_Comment by @sliedes on 2025-06-20 14:50_

Oh, you mean ruff doesn't already do type inference similar to mypy et al? :) I'm surprised, I always thought it does, because it works too well/is too useful for what I imagined is possible without :-).

If I may ask, what's the general case?

---

_Comment by @ntBre on 2025-06-20 16:07_

No it doesn't! That's great to hear that it's not too noticeable :) There's some limited local/single-file inference but not full type inference.

The "general case" is actually closely related. Ruff only does single-file analysis currently, so we could only resolve parent classes within a single file. We lump type inference and multi-file analysis together, at least in our GitHub labels, since our type checker will have both.

---
