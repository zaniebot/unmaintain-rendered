```yaml
number: 14426
title: "[red-knot] Types for subexpressions of annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/annotation-subexpressions-2
created_at: 2024-11-18T08:36:36Z
updated_at: 2024-11-18T12:04:03Z
url: https://github.com/astral-sh/ruff/pull/14426
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Types for subexpressions of annotations

---

_@sharkdp_

## Summary

This patches up various missing paths where sub-expressions of type annotations previously had no type attached. Examples include:
```py
tuple[int, str]
#     ~~~~~~~~

type[MyClass]
#    ~~~~~~~

Literal["foo"]
#       ~~~~~

Literal["foo", Literal[1, 2]]
#              ~~~~~~~~~~~~~

Literal[1, "a", random.illegal(sub[expr + ession])]
#               ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

## Test Plan

```
cargo nextest run -p red_knot_workspace -- --ignored linter_af linter_gz
```

---

_Label `red-knot` added by @sharkdp on 2024-11-18 08:36_

---

_Review requested from @carljm by @sharkdp on 2024-11-18 08:36_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-18 08:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-18 08:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:59 on 2024-11-18 08:52_

I think the point of this test was meant to be to show that arbitrary name expressions weren't accepted as parameters to `Literal`... I think it would be better to assign `hello = "hello"` above the `Literal` slice rather than asserting the error here. Then it would be clearer what is being tested, I think 

---

_Comment by @github-actions[bot] on 2024-11-18 08:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4459 on 2024-11-18 11:15_

nit: I'd prefer to do something like this, so that we only have to call `self.store_expression_type()` once:

```rs
                let ty = if return_todo {
                    Type::Todo
                } else {
                    Type::tuple(self.db, &element_types)
                };

                // Here, we store the type for the inner `int, str` tuple-expression,
                // while the type for the outer `tuple[int, str]` slice-expression is
                // stored in the outer `infer_type_expression` call:
                self.store_expression_type(tuple_slice, ty);

                ty
```

---

_@AlexWaygood approved on 2024-11-18 11:16_

---

_@AlexWaygood approved on 2024-11-18 11:51_

---

_Merged by @sharkdp on 2024-11-18 12:03_

---

_Closed by @sharkdp on 2024-11-18 12:03_

---

_Branch deleted on 2024-11-18 12:03_

---
