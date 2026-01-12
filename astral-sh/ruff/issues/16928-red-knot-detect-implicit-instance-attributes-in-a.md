```yaml
number: 16928
title: "[red-knot] detect implicit instance attributes in a generic method"
type: issue
state: closed
author: carljm
labels:
  - bug
  - ty
assignees: []
created_at: 2025-03-23T20:51:44Z
updated_at: 2025-05-02T08:20:39Z
url: https://github.com/astral-sh/ruff/issues/16928
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] detect implicit instance attributes in a generic method

---

_@carljm_

```py
class C:
    def f(self):
        self.x: int = 1
    
    def g[T](self, t: T) -> T:
        self.y: str = "foo"
        return t

reveal_type(C().x)  # correctly reveals `int`
reveal_type(C().y)  # should reveal `str`, but instead errors with `unresolved-attribute`
```

Generic functions have an additional type-params scope in between the outer scope and the function body scope. `SemanticIndexBuilder::is_method_of_class` fails to account for this, so we fail to record attribute assignments that occur in a generic method.

Relatedly, we now have "is this function a method of a class" logic separately both in `SemanticIndexBuilder::is_method_of_class` and in `TypeInferenceBuilder::infer_function_body` (as part of detecting whether a function is a method of a protocol). It may be worth seeing if we can unify that logic to a single location (maybe a method on `Scope`?).

---

_Label `bug` added by @carljm on 2025-03-23 20:51_

---

_Label `red-knot` added by @carljm on 2025-03-23 20:51_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-01 14:39_

---

_Closed by @sharkdp on 2025-05-02 08:20_

---
