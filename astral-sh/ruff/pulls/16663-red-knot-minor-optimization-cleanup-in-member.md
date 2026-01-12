```yaml
number: 16663
title: "[red-knot] Minor optimization/cleanup in member lookup"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/member-lookup-optimization
created_at: 2025-03-12T07:52:49Z
updated_at: 2025-03-12T08:11:07Z
url: https://github.com/astral-sh/ruff/pull/16663
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Minor optimization/cleanup in member lookup

---

_@sharkdp_

## Summary

A follow up to address [this comment]:

> Similarly here, it might be a little more performant to have a single `Type::instance()` branch with an inner match over `class.known()` rather than having multiple branches with `if class.is_known()` guards

[this comment]: https://github.com/astral-sh/ruff/pull/16416#discussion_r1985159037



---

_Label `red-knot` added by @sharkdp on 2025-03-12 07:52_

---

_Review requested from @carljm by @sharkdp on 2025-03-12 07:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-12 07:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-12 07:52_

---

_Comment by @github-actions[bot] on 2025-03-12 07:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-03-12 08:03_

---

_Merged by @sharkdp on 2025-03-12 08:11_

---

_Closed by @sharkdp on 2025-03-12 08:11_

---

_Branch deleted on 2025-03-12 08:11_

---
