```yaml
number: 1158
title: LSP crashes with a stack overflow for recursive types
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
  - fatal
assignees: []
created_at: 2025-09-09T14:52:52Z
updated_at: 2025-09-11T18:25:07Z
url: https://github.com/astral-sh/ty/issues/1158
synced_at: 2026-01-10T02:06:25Z
```

# LSP crashes with a stack overflow for recursive types

---

_Issue opened by @sharkdp on 2025-09-09 14:52_

### Summary

Start with
```py
type Recursive = Recursive | None


def _(rec: Recursive):
    
```
and type `rec` in the function body (slowly, potentially with explicit completion requests). This leads to crashes both in the playground and locally of `ty server`. It seems like an LSP-specific thing, because `reveal_type(rec)` ans similar function bodies don't produce any crashes.

### Version

_No response_

---

_Label `bug` added by @sharkdp on 2025-09-09 14:52_

---

_Label `server` added by @sharkdp on 2025-09-09 14:52_

---

_Label `fatal` added by @carljm on 2025-09-09 16:41_

---

_Assigned to @carljm by @carljm on 2025-09-11 14:35_

---

_Closed by @carljm on 2025-09-11 18:25_

---
