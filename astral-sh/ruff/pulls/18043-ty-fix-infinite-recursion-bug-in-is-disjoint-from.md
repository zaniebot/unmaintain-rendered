```yaml
number: 18043
title: "[ty] fix infinite recursion bug in `is_disjoint_from`"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: fix-is_disjoint_from
created_at: 2025-05-12T13:34:18Z
updated_at: 2025-05-12T14:41:50Z
url: https://github.com/astral-sh/ruff/pull/18043
synced_at: 2026-01-12T15:56:11Z
```

# [ty] fix infinite recursion bug in `is_disjoint_from`

---

_@mtshiba_

## Summary

I found this bug while working on #18041. The following code leads to infinite recursion.

```python
from ty_extensions import is_disjoint_from, static_assert, TypeOf

class C:
    @property
    def prop(self) -> int:
        return 1

static_assert(not is_disjoint_from(int, TypeOf[C.prop]))
```

The cause is a trivial missing binding in `is_disjoint_from`. This PR fixes the bug and adds a test case (this is a simple fix and may not require a new test case?).

## Test Plan

A new test case is added to `mdtest/type_properties/is_disjoint_from.md`.


---

_Review requested from @carljm by @mtshiba on 2025-05-12 13:34_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-05-12 13:34_

---

_Review requested from @sharkdp by @mtshiba on 2025-05-12 13:34_

---

_Review requested from @dcreager by @mtshiba on 2025-05-12 13:34_

---

_Comment by @github-actions[bot] on 2025-05-12 13:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `bug` added by @AlexWaygood on 2025-05-12 13:38_

---

_Label `ty` added by @AlexWaygood on 2025-05-12 13:38_

---

_@AlexWaygood approved on 2025-05-12 13:43_

Wow, that's subtle. Great catch, thank you!

---

_Merged by @AlexWaygood on 2025-05-12 13:44_

---

_Closed by @AlexWaygood on 2025-05-12 13:44_

---

_Branch deleted on 2025-05-12 14:30_

---

_Comment by @carljm on 2025-05-12 14:41_

Thank you!!

---
