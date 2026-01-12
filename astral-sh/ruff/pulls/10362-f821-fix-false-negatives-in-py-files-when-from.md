```yaml
number: 10362
title: "F821: Fix false negatives in `.py` files when `from __future__ import annotations` is active"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - linter
assignees: []
merged: true
base: main
head: f821-false-negs
created_at: 2024-03-12T16:01:13Z
updated_at: 2024-03-12T17:07:47Z
url: https://github.com/astral-sh/ruff/pull/10362
synced_at: 2026-01-12T15:55:31Z
```

# F821: Fix false negatives in `.py` files when `from __future__ import annotations` is active

---

_@AlexWaygood_

## Summary

Fixes #10340.

Currently, ruff emits no F821 rules on the following `.py` file, but the lines marked with XXX all fail at runtime due to undefined names, despite `from __future__ import annotations` being active:

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

This PR fixes that.

Much like #10341, it took me an annoying amount of time to figure out how to fix this, but it turned out to be hilariously easy to fix once I saw where to make the change 🙃

## Test Plan

`cargo test`


---

_Review requested from @charliermarsh by @AlexWaygood on 2024-03-12 16:01_

---

_Renamed from "F821: Fix false negatives in `.pyi` files when `from __future__ import annotations` is active" to "F821: Fix false negatives in `.py` files when `from __future__ import annotations` is active" by @AlexWaygood on 2024-03-12 16:08_

---

_Comment by @AlexWaygood on 2024-03-12 16:08_

(Original title of the PR said "`.pyi` files", but I meant "`.py` files", the exact opposite -- sorry!)

---

_Label `bug` added by @AlexWaygood on 2024-03-12 16:11_

---

_Label `linter` added by @AlexWaygood on 2024-03-12 16:11_

---

_Comment by @github-actions[bot] on 2024-03-12 16:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-03-12 17:05_

---

_Merged by @AlexWaygood on 2024-03-12 17:07_

---

_Closed by @AlexWaygood on 2024-03-12 17:07_

---

_Branch deleted on 2024-03-12 17:07_

---
