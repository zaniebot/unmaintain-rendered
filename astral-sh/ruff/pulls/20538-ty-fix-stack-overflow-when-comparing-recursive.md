```yaml
number: 20538
title: "[ty] fix stack overflow when comparing recursive `NamedTuple` types with `is_disjoint_from`"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: recursive-nominal-instance-is-disjoint-from
created_at: 2025-09-23T16:41:51Z
updated_at: 2025-09-25T14:20:10Z
url: https://github.com/astral-sh/ruff/pull/20538
synced_at: 2026-01-12T15:57:04Z
```

# [ty] fix stack overflow when comparing recursive `NamedTuple` types with `is_disjoint_from`

---

_@mtshiba_

## Summary

I found this bug while working on #20528.
The minimum reproducible code is:

```python
from __future__ import annotations

from typing import NamedTuple
from ty_extensions import is_disjoint_from, static_assert

class Path(NamedTuple):
    prev: Path | None
    key: str

static_assert(not is_disjoint_from(Path, Path))
```

A stack overflow occurs when a nominal instance type inherits from `NamedTuple` and is defined recursively.
This PR fixes this bug.

## Test Plan

mdtest updated


---

_Comment by @github-actions[bot] on 2025-09-23 16:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-23 16:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @mtshiba on 2025-09-23 16:59_

---

_Review requested from @carljm by @mtshiba on 2025-09-23 16:59_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-09-23 16:59_

---

_Review requested from @sharkdp by @mtshiba on 2025-09-23 16:59_

---

_Review requested from @dcreager by @mtshiba on 2025-09-23 16:59_

---

_@sharkdp approved on 2025-09-23 17:28_

Thank you!

---

_Merged by @sharkdp on 2025-09-23 17:29_

---

_Closed by @sharkdp on 2025-09-23 17:29_

---

_Branch deleted on 2025-09-24 01:39_

---

_Label `ty` added by @ntBre on 2025-09-25 14:20_

---
