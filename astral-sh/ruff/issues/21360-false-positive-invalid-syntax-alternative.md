```yaml
number: 21360
title: "False positive: `invalid-syntax` alternative patterns bind different names"
type: issue
state: closed
author: BarrensZeppelin
labels:
  - bug
assignees: []
created_at: 2025-11-10T10:01:28Z
updated_at: 2025-11-10T15:51:53Z
url: https://github.com/astral-sh/ruff/issues/21360
synced_at: 2026-01-12T15:54:57Z
```

# False positive: `invalid-syntax` alternative patterns bind different names

---

_@BarrensZeppelin_

### Summary

This file produces an `invalid-syntax` error, but it contains valid syntax:

```python3
import ast

match None:
    case ast.Subscript(n, ast.Constant() | ast.Slice()) | ast.Attribute(n):
        pass
```

https://play.ruff.rs/a88bae2d-d65a-4495-a33d-384a09fbbb4f

If I remove `| ast.Slice()`, the error goes away, so it seems like state is incorrectly shared between different alternative patterns.

### Version

ruff 0.14.4

---

_Comment by @MichaReiser on 2025-11-10 10:23_

Thanks. I hoped that this were fixed by https://github.com/astral-sh/ruff/pull/21104 but there still seems to be a bug with this check.

---

_Assigned to @ntBre by @MichaReiser on 2025-11-10 10:23_

---

_Label `bug` added by @MichaReiser on 2025-11-10 10:23_

---

_Closed by @ntBre on 2025-11-10 15:51_

---
