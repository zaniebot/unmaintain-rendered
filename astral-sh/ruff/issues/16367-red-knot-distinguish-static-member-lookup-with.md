```yaml
number: 16367
title: "[red-knot] Distinguish static-member lookup with and without instance variables"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-02-25T11:27:50Z
updated_at: 2025-03-07T21:03:29Z
url: https://github.com/astral-sh/ruff/issues/16367
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] Distinguish static-member lookup with and without instance variables

---

_Issue opened by @sharkdp on 2025-02-25 11:27_

### Summary

We currently distinguish between `Type::member` (with descriptor protocol) and `Type::static_member` (without descriptor protocol). The former corresponds to `obj.attr`, the latter corresponds to `getattr_static(obj, "attr")`. However, to model some details in the descriptor protocl correctly, we would also need to distinguish between a static member lookup *with* and *without* instance variables. The lookup without instance variables corresponds to `find_name_in_mro` [here](https://docs.python.org/3/howto/descriptor.html#invocation-from-an-instance). We currently approximate both using `member_static`.

One example where this leads to incorrect behavior is the following example:
```py
def some_function(x: int) -> str:
    return "a"

class C:
    def __init__(self):
        self.function = some_function


c = C()
c.function(1)  # We currently emit errors here because `function` is treated as a bound method
```


---

_Label `bug` added by @sharkdp on 2025-02-25 11:27_

---

_Label `red-knot` added by @sharkdp on 2025-02-25 11:27_

---

_Assigned to @sharkdp by @sharkdp on 2025-02-25 11:27_

---

_Closed by @sharkdp on 2025-03-07 21:03_

---

_Closed by @sharkdp on 2025-03-07 21:03_

---
