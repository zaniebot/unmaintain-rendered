```yaml
number: 1333
title: "`bind_self` mistakenly binds non-positional parameters"
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2025-10-10T12:15:01Z
updated_at: 2025-10-13T18:44:29Z
url: https://github.com/astral-sh/ty/issues/1333
synced_at: 2026-01-12T15:54:25Z
```

# `bind_self` mistakenly binds non-positional parameters

---

_@sharkdp_

Observe how `*args` is wrongly "bound" here:

```py
from ty_extensions import CallableTypeOf

class C:
    def method(*args, **kwargs) -> None: ...

def _(c: CallableTypeOf[C().method]):
    reveal_type(c)  # (**kwargs) -> None
```

---

_Label `bug` added by @sharkdp on 2025-10-10 12:15_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-10 12:15_

---

_Closed by @sharkdp on 2025-10-13 18:44_

---
