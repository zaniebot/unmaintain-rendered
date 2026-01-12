```yaml
number: 16717
title: "[red-knot] Use `try_call_dunder` for augmented assignment"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/augmented-assignment
created_at: 2025-03-13T22:33:18Z
updated_at: 2025-03-14T19:36:12Z
url: https://github.com/astral-sh/ruff/pull/16717
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Use `try_call_dunder` for augmented assignment

---

_@sharkdp_

## Summary

Uses the `try_call_dunder` infrastructure for augmented assignment and fixes the logic to work for types other than `Type::Instance(…)`. This allows us to infer the correct type here:
```py
x = (1, 2)
x += (3, 4)
reveal_type(x)  # revealed: tuple[Literal[1], Literal[2], Literal[3], Literal[4]]
```
Or in this (extremely weird) scenario:
```py
class Meta(type):
    def __iadd__(cls, other: int) -> str:
        return ""

class C(metaclass=Meta): ...

cls = C
cls += 1

reveal_type(cls)  # revealed: str
```

Union and intersection handling could also be improved here, but I made no attempt to do so in this PR.

## Test Plan

New MD tests

---

_Label `red-knot` added by @sharkdp on 2025-03-13 22:33_

---

_Comment by @github-actions[bot] on 2025-03-13 22:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[red-knot] Use try_call_dunder for augmented assignment" to "[red-knot] Use `try_call_dunder` for augmented assignment" by @sharkdp on 2025-03-14 11:55_

---

_Marked ready for review by @sharkdp on 2025-03-14 11:56_

---

_Review requested from @carljm by @sharkdp on 2025-03-14 11:56_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-14 11:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-14 11:56_

---

_@AlexWaygood approved on 2025-03-14 19:22_

Fantastic!

---

_Merged by @sharkdp on 2025-03-14 19:36_

---

_Closed by @sharkdp on 2025-03-14 19:36_

---

_Branch deleted on 2025-03-14 19:36_

---
