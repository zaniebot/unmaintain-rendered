---
number: 15962
title: "[red-knot] Support unpacking attribute assignments: `self.x, self.y = …`"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-02-05T11:50:51Z
updated_at: 2025-02-07T10:30:54Z
url: https://github.com/astral-sh/ruff/issues/15962
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Support unpacking attribute assignments: `self.x, self.y = …`

---

_Issue opened by @sharkdp on 2025-02-05 11:50_

The goal of this ticket is to get rid of the `TODO`s in [this test](https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/red_knot_python_semantic/resources/mdtest/attributes.md#attributes-defined-in-tuple-unpackings), and support attribute assignments in tuple/list unpackings:

```py
class C:
    def __init__(self) -> None:
        self.a, self.b = (1, "a")

c_instance = C()

reveal_type(c_instance.a)  # should be: Unknown | Literal[1]
reveal_type(c_instance.b)  # should be: Unknown | Literal["a"]
```

part of: https://github.com/astral-sh/ruff/issues/14164

---

_Referenced in [astral-sh/ruff#14164](../../astral-sh/ruff/issues/14164.md) on 2025-02-05 11:50_

---

_Renamed from "Support tuple-unpacking assignments: `self.x, self.y = …`" to "[red-knot] Support unpacking attribute assignments: `self.x, self.y = …`" by @sharkdp on 2025-02-05 11:51_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 11:51_

---

_Assigned to @sharkdp by @sharkdp on 2025-02-05 11:51_

---

_Referenced in [astral-sh/ruff#16004](../../astral-sh/ruff/pulls/16004.md) on 2025-02-06 22:09_

---

_Closed by @sharkdp on 2025-02-07 10:30_

---

_Closed by @sharkdp on 2025-02-07 10:30_

---
