```yaml
number: 18241
title: "[ty] Ignore `ClassVar` declarations when resolving instance members"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/ignore-classvar-declarations-for-instance-members
created_at: 2025-05-21T12:03:07Z
updated_at: 2025-05-21T17:23:37Z
url: https://github.com/astral-sh/ruff/pull/18241
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Ignore `ClassVar` declarations when resolving instance members

---

_Pull request opened by @sharkdp on 2025-05-21 12:03_

## Summary

Make sure that the following definitions all lead to the same outcome (bug originally noticed by @AlexWaygood)

```py
from typing import ClassVar

class Descriptor:
    def __get__(self, instance, owner) -> int:
        return 42

class C:
    a: ClassVar[Descriptor]
    b: Descriptor = Descriptor()
    c: ClassVar[Descriptor] = Descriptor()

reveal_type(C().a)  # revealed: int  (previously: int | Descriptor)
reveal_type(C().b)  # revealed: int
reveal_type(C().c)  # revealed: int
```

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-05-21 12:03_

---

_Renamed from "[ty] Ignore ClassVar declarations when resolving instance members" to "[ty] Ignore `ClassVar` declarations when resolving instance members" by @sharkdp on 2025-05-21 12:03_

---

_Comment by @github-actions[bot] on 2025-05-21 12:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-05-21 12:09_

---

_Review requested from @carljm by @sharkdp on 2025-05-21 12:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-21 12:09_

---

_Review requested from @dcreager by @sharkdp on 2025-05-21 12:09_

---

_@AlexWaygood approved on 2025-05-21 13:46_

Nice!

---

_@carljm approved on 2025-05-21 16:06_

---

_Merged by @sharkdp on 2025-05-21 17:23_

---

_Closed by @sharkdp on 2025-05-21 17:23_

---

_Branch deleted on 2025-05-21 17:23_

---
