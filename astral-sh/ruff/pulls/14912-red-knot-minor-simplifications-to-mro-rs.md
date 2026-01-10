```yaml
number: 14912
title: "[red-knot] Minor simplifications to `mro.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-mro
created_at: 2024-12-11T13:09:44Z
updated_at: 2024-12-11T13:28:46Z
url: https://github.com/astral-sh/ruff/pull/14912
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Minor simplifications to `mro.rs`

---

_Pull request opened by @AlexWaygood on 2024-12-11 13:09_

## Summary

No functional change. Remove a few unnecessary intermediate calls, and have `db` be the first parameter for `ClassBase::try_from_ty()`, for consistency with other functions and methods in red-knot.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `internal` added by @AlexWaygood on 2024-12-11 13:09_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 13:09_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-11 13:09_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-11 13:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-11 13:09_

---

_Merged by @AlexWaygood on 2024-12-11 13:14_

---

_Closed by @AlexWaygood on 2024-12-11 13:14_

---

_Branch deleted on 2024-12-11 13:14_

---

_Comment by @github-actions[bot] on 2024-12-11 13:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:86 on 2024-12-11 13:20_

Nit: I always find `map_or_else` somewhat hard to read because the else becomes before the map (huh?)

```suggestion
            [single_base] => ClassBase::try_from_ty(db, *single_base)
                .map(|single_base| {
                    std::iter::once(ClassBase::Class(class))
                        .chain(single_base.mro(db))
                        .collect()
                })
                .ok_or_else(|| MroErrorKind::InvalidBases(Box::from([(0, *single_base)]))),
```

---

_@MichaReiser reviewed on 2024-12-11 13:20_

---

_@AlexWaygood reviewed on 2024-12-11 13:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:86 on 2024-12-11 13:28_

I don't really have a strong preference either way. I weakly prefer `map_or_else`, but I agree with you that the argument order is kind of confusing. I won't object if you push another PR switching to this idiom ;)

---
