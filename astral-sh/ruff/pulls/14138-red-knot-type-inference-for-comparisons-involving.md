```yaml
number: 14138
title: "[red-knot] Type inference for comparisons involving intersection types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/intersection-types-compare-expressions
created_at: 2024-11-06T19:50:47Z
updated_at: 2024-11-07T19:51:16Z
url: https://github.com/astral-sh/ruff/pull/14138
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Type inference for comparisons involving intersection types

---

_@sharkdp_

## Summary

This adds type inference for comparison expressions involving intersection types.


closes #13854

## Eye candy

```py
x = get_random_int()

if x != 42:
    reveal_type(x == 42)  # revealed: Literal[False]
    reveal_type(x == 43)  # bool
```


## Test Plan

New Markdown-based tests.

---

_Comment by @github-actions[bot] on 2024-11-06 20:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-11-06 23:53_

---

_@T-256 reviewed on 2024-11-07 09:18_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:41 on 2024-11-07 09:18_

```suggestion
    reveal_type(x != "something else")  # revealed: bool
    reveal_type("something else" != x)  # revealed: bool
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:41 on 2024-11-07 09:25_

Thanks!

---

_@sharkdp reviewed on 2024-11-07 09:25_

---

_Marked ready for review by @sharkdp on 2024-11-07 18:01_

---

_Review requested from @carljm by @sharkdp on 2024-11-07 18:01_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-07 18:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-07 18:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3198 on 2024-11-07 19:22_

```suggestion
        // If none of the simplifications above apply, we still need to return *some*
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3221 on 2024-11-07 19:24_

Great write-up! Very clear.

---

_@carljm approved on 2024-11-07 19:24_

🎉 

---

_Merged by @sharkdp on 2024-11-07 19:51_

---

_Closed by @sharkdp on 2024-11-07 19:51_

---

_Branch deleted on 2024-11-07 19:51_

---
