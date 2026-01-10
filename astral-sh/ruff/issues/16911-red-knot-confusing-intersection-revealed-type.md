```yaml
number: 16911
title: "[red-knot] Confusing Intersection revealed type"
type: issue
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
created_at: 2025-03-22T11:30:56Z
updated_at: 2025-03-24T11:12:44Z
url: https://github.com/astral-sh/ruff/issues/16911
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Confusing Intersection revealed type

---

_Issue opened by @MatthewMckee4 on 2025-03-22 11:30_

### Summary

As you can see below, When using intersection with
```py
from typing import reveal_type
from knot_extensions import Intersection, Not

def _(
    a: Intersection[Not[str], int],
    b: Intersection[Not[str], float],
    c: Intersection[Not[str], complex],
):
    reveal_type(a) # revealed: `int & str`
    reveal_type(b) # revealed: `int & str | float & str`
    reveal_type(c) # revealed: `int & str | float & str | complex & str`
```

Im aware this maybe isn't the best example, but it seems there is a problem and the intersection shouldnt expand here. Im aware that complex converts to union[int, float, complex] so maybe theres not an easy way to fix this.

### Version

_No response_

---

_Renamed from "Confusing Intersection revealed type" to "[red-knot] Confusing Intersection revealed type" by @MatthewMckee4 on 2025-03-22 11:33_

---

_Label `red-knot` added by @MichaReiser on 2025-03-22 12:06_

---

_Closed by @MatthewMckee4 on 2025-03-24 11:12_

---
