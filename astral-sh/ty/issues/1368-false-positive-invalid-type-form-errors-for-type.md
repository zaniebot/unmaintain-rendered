```yaml
number: 1368
title: "False-positive `invalid-type-form` errors for type aliases inside `Literal` slices"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-10-16T14:03:58Z
updated_at: 2025-11-02T17:39:56Z
url: https://github.com/astral-sh/ty/issues/1368
synced_at: 2026-01-12T15:54:25Z
```

# False-positive `invalid-type-form` errors for type aliases inside `Literal` slices

---

_@AlexWaygood_

### Summary

This issue reproduces on `main` if you use PEP-695 type aliases, and starts causing a lot more false positives on https://github.com/astral-sh/ruff/pull/20107. Pyright and mypy are both fine with this snippet, but we currently emit `invalid-type-form` on it:

```py
from typing import Literal
type X = Literal["foo"]

# error: [invalid-type-form] "Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member"
x: Literal[X]
```

 

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-16 14:03_

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-16 14:04_

---

_Assigned to @carljm by @carljm on 2025-10-30 16:24_

---

_Closed by @carljm on 2025-11-02 17:39_

---
