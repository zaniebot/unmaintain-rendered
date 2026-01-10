```yaml
number: 1580
title: Diagnostic shows wrong type in unpack assignment
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-11-18T10:47:50Z
updated_at: 2025-12-06T16:07:16Z
url: https://github.com/astral-sh/ty/issues/1580
synced_at: 2026-01-10T01:56:40Z
```

# Diagnostic shows wrong type in unpack assignment

---

_Issue opened by @sharkdp on 2025-11-18 10:47_

The diagnostic message in the following example should say *"Object of type **`int`** is not assignable to `str`"*:
```py
x: str = ""

def returns_tuple() -> tuple[int, int]:
    return (0, 0)

# Object of type `tuple[int, int]` is not assignable to `str`
x, y = returns_tuple()
```
https://play.ty.dev/29ce53bf-0300-4f30-b00d-c30480f7f497

---

_Label `bug` added by @sharkdp on 2025-11-18 10:47_

---

_Label `diagnostics` added by @sharkdp on 2025-11-18 10:47_

---

_Added to milestone `Stable` by @carljm on 2025-11-18 16:12_

---

_Comment by @prakhar1144 on 2025-12-04 18:55_

Working on a fix for this. Thanks!

---

_Assigned to @prakhar1144 by @sharkdp on 2025-12-05 08:26_

---

_Comment by @prakhar1144 on 2025-12-06 06:20_

Looks like the issue no longer exists.

The linked playground in issue description already shows "Object of type `int` is not assignable to `str`" as the diagnostic message.

I tried in a fresh playground as well: https://play.ty.dev/73b97488-fb3d-4481-bae5-810e7d42bc48

---

_Comment by @carljm on 2025-12-06 16:07_

Thanks for checking, @prakhar1144 ! 

---

_Closed by @carljm on 2025-12-06 16:07_

---
