```yaml
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
synced_at: 2026-01-12T15:54:50Z
```

# F821: false negatives when a file has `from __future__ import annotations`

---

_@AlexWaygood_

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

_Label `linter` added by @zanieb on 2024-03-11 16:42_

---

_Closed by @AlexWaygood on 2024-03-12 17:07_

---

_Comment by @AlexWaygood on 2024-03-21 16:48_

The PR fixing this was reverted in #10513

---

_Reopened by @AlexWaygood on 2024-03-21 16:48_

---

_Closed by @AlexWaygood on 2024-03-22 18:11_

---
