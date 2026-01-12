```yaml
number: 14176
title: "[red-knot] Minor fix in intersection type comment"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-intersection-type-explanation
created_at: 2024-11-07T20:14:08Z
updated_at: 2024-11-08T10:44:43Z
url: https://github.com/astral-sh/ruff/pull/14176
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Minor fix in intersection type comment

---

_@sharkdp_

## Summary

Minor fix in intersection type comment introduced in #14138 




---

_Review requested from @carljm by @sharkdp on 2024-11-07 20:14_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-07 20:14_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-07 20:14_

---

_@carljm approved on 2024-11-07 20:17_

---

_@sharkdp reviewed on 2024-11-07 20:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3212 on 2024-11-07 20:20_

It's better not to talk about the size of `~f(A)`. We should rather compare `f(A & ~B)` with `f(A) & ~f(B)`. The former can be larger (`bool`) than the latter (`Literal[True]`), as demonstrated in the example below.

---

_@carljm reviewed on 2024-11-07 20:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3212 on 2024-11-07 20:22_

Yes, this is the part I was having the hardest time thinking through clearly

---

_@carljm reviewed on 2024-11-07 20:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3212 on 2024-11-07 20:22_

The fully worked example makes it much clearer

---

_@carljm approved on 2024-11-07 20:22_

---

_Merged by @sharkdp on 2024-11-07 20:23_

---

_Closed by @sharkdp on 2024-11-07 20:23_

---

_Branch deleted on 2024-11-07 20:23_

---

_Comment by @github-actions[bot] on 2024-11-07 20:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @dhruvmanila on 2024-11-08 10:44_

---
