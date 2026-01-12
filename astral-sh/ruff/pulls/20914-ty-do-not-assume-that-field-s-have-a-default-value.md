```yaml
number: 20914
title: "[ty] Do not assume that `field`s have a default value"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1366
created_at: 2025-10-16T09:39:10Z
updated_at: 2025-10-16T10:50:43Z
url: https://github.com/astral-sh/ruff/pull/20914
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Do not assume that `field`s have a default value

---

_@sharkdp_

## Summary

fixes https://github.com/astral-sh/ty/issues/1366

## Test Plan

Added regression test


---

_Review requested from @carljm by @sharkdp on 2025-10-16 09:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-16 09:39_

---

_Review requested from @dcreager by @sharkdp on 2025-10-16 09:39_

---

_Label `bug` added by @sharkdp on 2025-10-16 09:39_

---

_Label `ty` added by @sharkdp on 2025-10-16 09:39_

---

_Comment by @github-actions[bot] on 2025-10-16 09:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 09:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1649 on 2025-10-16 10:27_

is this correct? This seems to imply that a `Field` instance without a default is assignable to literally _anything_?

Can we make the special case here slightly narrower, like this?

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 8fa72656e4..d538308d80 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1634,10 +1634,12 @@ impl<'db> Type<'db> {
             // This allows field definitions like `name: str = field(default="")` in dataclasses
             // to pass the assignability check of the inferred type to the declared type.
             (Type::KnownInstance(KnownInstanceType::Field(field)), right)
-                if relation.is_assignability() =>
+                if relation.is_assignability() && field.default_type(db).is_some() =>
             {
-                field.default_type(db).when_none_or(|default_type| {
-                    default_type.has_relation_to_impl(
+                field
+                    .default_type(db)
+                    .expect("Checked in branch arm")
+                    .has_relation_to_impl(
                         db,
                         right,
                         inferable,
@@ -1645,7 +1647,6 @@ impl<'db> Type<'db> {
                         relation_visitor,
                         disjointness_visitor,
                     )
-                })
             }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7610 on 2025-10-16 10:27_

```suggestion
                            write!(f, "[{}]", default_ty.display(self.db))?;
```

---

_@AlexWaygood approved on 2025-10-16 10:28_

---

_@sharkdp reviewed on 2025-10-16 10:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1649 on 2025-10-16 10:34_

> This seems to imply that a `Field` instance without a default is assignable to literally _anything_?

Yes, I think we want that. Consider something like the following, where the `field` call is really only there to exclude the `id` field from the `__init__` signature. This field does not have a default value.
```py
id: int = field(init=False)
```

---

_@AlexWaygood reviewed on 2025-10-16 10:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1649 on 2025-10-16 10:37_

Ah I see. Fair enough! Do we have tests that will fail if we made the bad refactor I proposed here?

---

_@sharkdp reviewed on 2025-10-16 10:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1649 on 2025-10-16 10:49_

Yes, all kinds of tests fail if we do this.

---

_Merged by @sharkdp on 2025-10-16 10:49_

---

_Closed by @sharkdp on 2025-10-16 10:49_

---

_Branch deleted on 2025-10-16 10:49_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1649 on 2025-10-16 10:50_

thanks!

---

_@AlexWaygood reviewed on 2025-10-16 10:50_

---
