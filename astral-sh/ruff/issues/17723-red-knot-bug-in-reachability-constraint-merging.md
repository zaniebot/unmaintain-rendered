```yaml
number: 17723
title: "[red-knot] Bug in reachability constraint merging"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-29T20:52:08Z
updated_at: 2025-04-30T07:32:14Z
url: https://github.com/astral-sh/ruff/issues/17723
synced_at: 2026-01-12T15:54:56Z
```

# [red-knot] Bug in reachability constraint merging

---

_@sharkdp_

Combining reachability constraints does not seem to work as expected. Consider the following example where we fail to see that `print(x)` is unreachable:
```py
def _(cond: bool):
    if cond:
        if True:
            pass
        else:
            print(x)  # should not be reachable, i.e. not emit an error
```
https://types.ruff.rs/cc26b966-0df2-4111-8448-2a1d15d95586

---

_Label `bug` added by @sharkdp on 2025-04-29 20:52_

---

_Label `red-knot` added by @sharkdp on 2025-04-29 20:52_

---

_Assigned to @sharkdp by @sharkdp on 2025-04-29 20:52_

---

_Renamed from "[red-knot]" to "[red-knot] Bug in reachability constraint merging" by @sharkdp on 2025-04-29 20:52_

---

_Closed by @sharkdp on 2025-04-30 07:32_

---
