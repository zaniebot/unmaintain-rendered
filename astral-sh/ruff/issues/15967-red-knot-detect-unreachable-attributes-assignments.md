```yaml
number: 15967
title: "[red-knot] Detect unreachable attributes assignments"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-02-05T13:13:05Z
updated_at: 2025-04-14T07:23:22Z
url: https://github.com/astral-sh/ruff/issues/15967
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] Detect unreachable attributes assignments

---

_Issue opened by @sharkdp on 2025-02-05 13:13_

This is probably not a high-priority feature, but it would be great if we could detect attribute assignments in unreachable code sections, and eliminate them from the set of possible answers:

```py
class C:
    def __init__(self) -> None:
        if True:
            self.x = 1
        else:
            self.x = "a"

        return

        self.y = "unreachable"

reveal_type(C().x)  # should be `Unknown | Literal[1]` (i.e. not include `Literal["a"]`)

# Should be an error:
C().y
```

Note that we have one existing test for this here:

https://github.com/astral-sh/ruff/blob/16f2a93fca20082179df082bc16a2c5095933772/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L434-L448

---

_Renamed from "Hide attributes assignments that occur in statically-known-to-be-false branches" to "[red-knot] Attributes assignments in statically-known branches" by @sharkdp on 2025-02-05 13:13_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 13:13_

---

_Renamed from "[red-knot] Attributes assignments in statically-known branches" to "[red-knot] Detect unreachable attributes assignments" by @sharkdp on 2025-02-05 13:14_

---

_Added to milestone `Red Knot Beta` by @carljm on 2025-03-27 18:35_

---

_Removed from milestone `Red Knot Beta` by @carljm on 2025-03-27 18:36_

---

_Added to milestone `Red Knot Public Release` by @carljm on 2025-03-27 18:36_

---

_Closed by @sharkdp on 2025-04-14 07:23_

---

_Closed by @sharkdp on 2025-04-14 07:23_

---
