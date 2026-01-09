---
number: 20748
title: "New Rule: Detect/Fix Circuitous Imports"
type: issue
state: closed
author: Redoubts
labels: []
assignees: []
created_at: 2025-10-07T15:24:56Z
updated_at: 2025-10-07T16:11:10Z
url: https://github.com/astral-sh/ruff/issues/20748
synced_at: 2026-01-07T13:12:16-06:00
---

# New Rule: Detect/Fix Circuitous Imports

---

_Issue opened by @Redoubts on 2025-10-07 15:24_

### Summary

I think it would be cool if there were a rule that checked and auto-fixed imports if they were unintentionally importing from a module that also was importing that module. For example, consider

```
from mylib.a import A1
from mylib.b import B, A2
from mylib.c import C, A3
```

A1, A2, and A3 are all actually defined in `mylib.a`; `mylib.b` and `mylib.c` just happen to also be using A2 and A3. The A2 and A3 imports in the above snippet should be marked as fixable errors. 

This should be considered bad because

1) you might be importing more things than necessary. If you were just grabbing `A2` from `mylib.b`, then loading `mylib.b` was needless.
2) this is fragile, as A2 might move around in `mylib.b` or be removed one day. 

Possibly only for first-party and local imports

---

_Comment by @ntBre on 2025-10-07 16:11_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/19559.

But I agree that it sounds like a useful rule! We just need multi-file analysis/type inference to enable it.

---

_Closed by @ntBre on 2025-10-07 16:11_

---
