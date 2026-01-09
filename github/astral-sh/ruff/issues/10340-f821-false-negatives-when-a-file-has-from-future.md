---
number: 10340
title: "F821: false negatives when a file has `from __future__ import annotations`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - linter
assignees: []
created_at: 2024-03-11T14:10:30Z
updated_at: 2024-03-22T18:11:18Z
url: https://github.com/astral-sh/ruff/issues/10340
synced_at: 2026-01-07T13:12:15-06:00
---

# F821: false negatives when a file has `from __future__ import annotations`

---

_Issue opened by @AlexWaygood on 2024-03-11 14:10_

Ruff currently emits no F821 errors on this `.py` file, but the lines marked with `XXX` all fail at runtime due to undefined names:

```py
from __future__ import annotations

from typing import TypeAlias, Optional, Union

MaybeCStr: TypeAlias = Optional[CStr]  # XXX
MaybeCStr2: TypeAlias = Optional["CStr"]  # always okay
CStr: TypeAlias = Union[C, str]  # XXX
CStr2: TypeAlias = Union["C", str]  # always okay

class C: ...

class Leaf: ...
class Tree(list[Tree | Leaf]): ...  # XXX
class Tree2(list["Tree | Leaf"]): ...  # always okay
```

Note:
- Ruff correctly emits three errors on this file when `from __future__ import annotations` is not included
- Ruff correctly emits no errors on this file if it's a `.pyi` stub file.

---

_Label `bug` added by @AlexWaygood on 2024-03-11 14:10_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-03-11 14:10_

---

_Referenced in [astral-sh/ruff#10341](../../astral-sh/ruff/pulls/10341.md) on 2024-03-11 14:17_

---

_Label `linter` added by @zanieb on 2024-03-11 16:42_

---

_Referenced in [astral-sh/ruff#10362](../../astral-sh/ruff/pulls/10362.md) on 2024-03-12 16:01_

---

_Closed by @AlexWaygood on 2024-03-12 17:07_

---

_Referenced in [astral-sh/ruff#10451](../../astral-sh/ruff/issues/10451.md) on 2024-03-18 09:15_

---

_Comment by @AlexWaygood on 2024-03-21 16:48_

The PR fixing this was reverted in #10513

---

_Reopened by @AlexWaygood on 2024-03-21 16:48_

---

_Referenced in [astral-sh/ruff#10524](../../astral-sh/ruff/pulls/10524.md) on 2024-03-22 11:29_

---

_Closed by @AlexWaygood on 2024-03-22 18:11_

---
