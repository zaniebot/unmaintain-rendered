```yaml
number: 648
title: Statements after infinite loops are not considered unreachable
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-06-13T13:40:10Z
updated_at: 2025-06-17T07:24:30Z
url: https://github.com/astral-sh/ty/issues/648
synced_at: 2026-01-10T02:08:20Z
```

# Statements after infinite loops are not considered unreachable

---

_Issue opened by @sharkdp on 2025-06-13 13:40_

The following example should not emit a diagnostic
```py
def f():
    while True:
        pass

    foo  # error: unresolved-reference
```
https://play.ty.dev/9108a9b2-63fd-42d7-8a3c-5810509cdd4a

---

_Label `bug` added by @sharkdp on 2025-06-13 13:40_

---

_Label `control flow` added by @sharkdp on 2025-06-13 13:40_

---

_Assigned to @sharkdp by @sharkdp on 2025-06-13 13:40_

---

_Closed by @sharkdp on 2025-06-17 07:24_

---
