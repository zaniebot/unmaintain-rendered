```yaml
number: 481
title: Incorrect literal math with positive and negative operands
type: issue
state: closed
author: correctmost
labels:
  - bug
  - runtime semantics
assignees: []
created_at: 2025-05-21T21:31:47Z
updated_at: 2025-05-22T14:42:30Z
url: https://github.com/astral-sh/ty/issues/481
synced_at: 2026-01-10T02:34:10Z
```

# Incorrect literal math with positive and negative operands

---

_Issue opened by @correctmost on 2025-05-21 21:31_

### Summary

```python
from typing import reveal_type

reveal_type(256 % -129)
reveal_type(-129 % 16)

reveal_type(-10 // 8)
reveal_type(10 // -6)
```

Actual results:
```
3 | reveal_type(256 % -129)
  |             ^^^^^^^^^^ `Literal[127]`

4 | reveal_type(-129 % 16)
  |             ^^^^^^^^^ `Literal[-1]`

6 | reveal_type(-10 // 8)
  |             ^^^^^^^^ `Literal[-1]`

7 | reveal_type(10 // -6)
  |             ^^^^^^^^ `Literal[-1]`
  |
```

Expected results:
```
3 | reveal_type(256 % -129)
  |             ^^^^^^^^^^ `Literal[-2]`

4 | reveal_type(-129 % 16)
  |             ^^^^^^^^^ `Literal[15]`

6 | reveal_type(-10 // 8)
  |             ^^^^^^^^ `Literal[-2]`

7 | reveal_type(10 // -6)
  |             ^^^^^^^^ `Literal[-2]`
  |
```

Pyright had similar bugs:
- https://github.com/microsoft/pyright/issues/10167
- https://github.com/microsoft/pyright/issues/10076

### Version

ty 0.0.1-alpha.6

---

_Comment by @AlexWaygood on 2025-05-21 21:36_

@brandtbucher, wanna take a look?

---

_Comment by @brandtbucher on 2025-05-21 21:51_

Put me in, coach!

---

_Assigned to @brandtbucher by @AlexWaygood on 2025-05-21 21:54_

---

_Label `bug` added by @AlexWaygood on 2025-05-21 22:04_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-21 22:04_

---

_Closed by @carljm on 2025-05-22 14:42_

---
