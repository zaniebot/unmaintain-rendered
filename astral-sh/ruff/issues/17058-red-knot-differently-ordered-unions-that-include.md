```yaml
number: 17058
title: "[red-knot] Differently ordered unions that include `Callable` types are not always considered equivalent "
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
created_at: 2025-03-29T22:22:04Z
updated_at: 2025-04-02T17:43:35Z
url: https://github.com/astral-sh/ruff/issues/17058
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Differently ordered unions that include `Callable` types are not always considered equivalent 

---

_@AlexWaygood_

### Summary

Currently, this mdtest fails, even though it should pass:

```py
def f3(a1: int, /, *args1: int, **kwargs2: int) -> None: ...
def f4(a2: int, /, *args2: int, **kwargs1: int) -> None: ...

static_assert(is_equivalent_to(CallableTypeOf[f3] | bool | CallableTypeOf[f4], CallableTypeOf[f4] | bool | CallableTypeOf[f3]))
```

Note that this is despite the fact that the simpler assertion `static_assert(is_equivalent_to(CallableTypeOf[f3], CallableTypeOf[f4])` passes: the `Callable` types must appear in unions for the bug to occur.

I hoped that applying this diff to `type_ordering.rs` would fix this bug, but the assertion still fails:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/type_ordering.rs b/crates/red_knot_python_semantic/src/types/type_ordering.rs
index 79f0abe1d..c0b160036 100644
--- a/crates/red_knot_python_semantic/src/types/type_ordering.rs
+++ b/crates/red_knot_python_semantic/src/types/type_ordering.rs
@@ -83,9 +83,10 @@ pub(super) fn union_or_intersection_elements_ordering<'db>(
         (Type::Callable(CallableType::WrapperDescriptorDunderGet), _) => Ordering::Less,
         (_, Type::Callable(CallableType::WrapperDescriptorDunderGet)) => Ordering::Greater,
 
-        (Type::Callable(CallableType::General(_)), Type::Callable(CallableType::General(_))) => {
-            Ordering::Equal
-        }
+        (
+            Type::Callable(CallableType::General(left)),
+            Type::Callable(CallableType::General(right)),
+        ) => left.cmp(right),
         (Type::Callable(CallableType::General(_)), _) => Ordering::Less,
         (_, Type::Callable(CallableType::General(_))) => Ordering::Greater,
```

Adding some `reveal_type` calls to the test indicates why: the two unions have _different_ `Callable` types in them (even though the two `Callable` types are equivalent, they have different names for the parameters):

```py
def f3(a1: int, /, *args1: int, **kwargs2: int) -> None: ...
def f4(a2: int, /, *args2: int, **kwargs1: int) -> None: ...

def f(x: CallableTypeOf[f3] | bool | CallableTypeOf[f4], y: CallableTypeOf[f4] | bool | CallableTypeOf[f3]):
    reveal_type(x)  # revealed: ((a1: int, /, *args1: int, **kwargs2: int) -> None) | bool
    reveal_type(y)  # revealed: ((a2: int, /, *args2: int, **kwargs1: int) -> None) | bool
```

and our implementation of `is_equivalent_to` for `UnionType`s currently assumes that two sorted unions can only ever be equivalent if they have identical Salsa IDs:

https://github.com/astral-sh/ruff/blob/ab1011ce70913a2ba66b51b569baed3bcb434602/crates/red_knot_python_semantic/src/types.rs#L5081-L5116

There are two possible fixes here:
1. Change things so that two equivalent `Callable` types are always guaranteed to have the same Salsa IDs (discussed as a possibility in https://github.com/astral-sh/ruff/pull/16698#discussion_r2003121350)
2. Change the implementation of `UnionType::is_equivalent_to` so that it looks more similar to `UnionType::is_gradual_equivalent_to`, which takes care to iterate over all elements rather than simply checking the Salsa IDs: https://github.com/astral-sh/ruff/blob/ab1011ce70913a2ba66b51b569baed3bcb434602/crates/red_knot_python_semantic/src/types.rs#L5144-L5148

---

_Label `bug` added by @AlexWaygood on 2025-03-29 22:22_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-29 22:22_

---

_Comment by @AlexWaygood on 2025-03-29 22:22_

Cc. @dhruvmanila 

---

_Renamed from "[red-knot] Differently ordered unions that include `Callable` types are not always considered equal" to "[red-knot] Differently ordered unions that include `Callable` types are not always considered equivalent " by @AlexWaygood on 2025-03-29 23:01_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-03-31 22:04_

---

_Closed by @AlexWaygood on 2025-04-02 17:43_

---
