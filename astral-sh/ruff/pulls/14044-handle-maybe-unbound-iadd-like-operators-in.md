```yaml
number: 14044
title: "Handle maybe-unbound `__iadd__`-like operators in augmented assignments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/bound
created_at: 2024-11-01T15:51:37Z
updated_at: 2024-11-01T17:15:38Z
url: https://github.com/astral-sh/ruff/pull/14044
synced_at: 2026-01-12T15:55:46Z
```

# Handle maybe-unbound `__iadd__`-like operators in augmented assignments

---

_@charliermarsh_

## Summary

One of the follow-ups from augmented assignment inference, now that `Type::Unbound` has been removed.

---

_Marked ready for review by @charliermarsh on 2024-11-01 16:00_

---

_Review requested from @carljm by @charliermarsh on 2024-11-01 16:00_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-11-01 16:00_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-01 16:00_

---

_Review requested from @sharkdp by @charliermarsh on 2024-11-01 16:00_

---

_Label `red-knot` added by @charliermarsh on 2024-11-01 16:01_

---

_Comment by @github-actions[bot] on 2024-11-01 16:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1600 on 2024-11-01 16:11_

```suggestion
                        UnionType::from_elements(
                            self.db,
                            [augmented_return_ty, binary_return_ty]
                        )
```

---

_@AlexWaygood reviewed on 2024-11-01 16:11_

---

_@carljm approved on 2024-11-01 17:03_

---

_Merged by @charliermarsh on 2024-11-01 17:15_

---

_Closed by @charliermarsh on 2024-11-01 17:15_

---

_Branch deleted on 2024-11-01 17:15_

---
